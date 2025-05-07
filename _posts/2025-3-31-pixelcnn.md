---
key:
title: 'pixelcnn-1 전처리'
excerpt: 'pixelcnn'
tags: [pixelcnn]
---

```py
# 첫 번째 셀: 라이브러리 임포트
import os
import numpy as np
import matplotlib.pyplot as plt
from tqdm import tqdm

import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torch.utils.data import DataLoader, Dataset
from torchvision import datasets, transforms
from torchvision.utils import save_image, make_grid

from google.colab import drive
drive.mount('/content/drive')

# 저장 경로 설정
import os
DRIVE_PATH = '/content/drive/MyDrive/pixelcnn_results'
os.makedirs(DRIVE_PATH, exist_ok=True)
print(f"결과가 저장될 경로: {DRIVE_PATH}")
```

```py
# 두 번째 셀: 하이퍼파라미터 설정 
# PixelCNN 모델의 하이퍼파라미터
PARAMS = {
    # 데이터셋 설정
    'dataset_name': 'cifar10',  # 'mnist' 또는 'cifar10'
    'img_size': 32,
    'batch_size': 128,
    
    # 모델 아키텍처 설정
    'hidden_channels': 256,     # 128에서 256으로 증가
    'n_residual': 15,           # 5에서 15로 증가
    'kernel_size': 7,           # 첫 번째 레이어의 커널 크기
    
    # 학습 설정
    'epochs': 30,               # 15에서 30으로 증가
    'lr': 3e-4,
    'weight_decay': 1e-5,       # L2 정규화 추가
    'lr_scheduler_factor': 0.5,
    'lr_scheduler_patience': 5,
    
    # 생성 설정
    'temperature': 0.6,         # 0.8에서 0.6으로 감소
    'n_per_class': 2,
    
    # 저장 설정
    'save_dir': DRIVE_PATH,     # 구글 드라이브 경로로 변경
    'model_name': 'pixelcnn_improved'
}

# 설정 저장 함수
def save_config(config, filepath):
    """설정 저장"""
    import json
    with open(filepath, 'w') as f:
        json.dump(config, f, indent=4)

# 설정 파일 저장
save_config(PARAMS, f'{PARAMS["save_dir"]}/config.json')
```

```py
def save_config(config, filepath):
    """설정 저장"""
    import json
    with open(filepath, 'w') as f:
        json.dump(config, f, indent=4)
```

이 함수는 두 개의 인자를 받습니다:

- config: 저장할 설정 정보가 담긴 딕셔너리

- filepath: 설정 파일을 저장할 경로

```py
with open(filepath, 'w') as f:
```

with 문은 파일을 안전하게 열고 닫는 컨텍스트 매니저를 제공합니다. 'w' 모드는 쓰기 모드를 의미하며, 파일이 존재하지 않으면 새로 생성합니다.

```py
json.dump(config, f, indent=4)
```

json.dump() 함수는 Python 객체를 JSON 형식으로 변환하여 파일에 저장합니다. indent=4 매개변수는 JSON 파일에 4칸 들여쓰기를 적용하여 가독성을 높입니다.

즉 여기서 PARAMS는 매개변수 모은거고 두번째 인자는 save_dir경로에 config.json형태로 설정을 저장한다.

 n_residual : 레지듀얼 블럭, 추후에 다시 자세히 공부, 대충 레이어를 중간에 건너 띄면서 학습한다는 듯

```py
# 세 번째 셀: 데이터 전처리 및 로딩 함수 (약간 수정)
def load_dataset(dataset_name='cifar10', batch_size=128, img_size=32):
    if dataset_name.lower() == 'cifar10':
        # CIFAR-10 데이터셋 설정
        transform = transforms.Compose([
            transforms.Resize((img_size, img_size)),
            transforms.ToTensor()
        ])
        
        train_dataset = datasets.CIFAR10('./data', train=True, download=True, transform=transform)
        test_dataset = datasets.CIFAR10('./data', train=False, download=True, transform=transform)
        n_classes = 10
        in_channels = 3
        
    elif dataset_name.lower() == 'mnist':
        # MNIST 데이터셋 설정
        transform = transforms.Compose([
            transforms.Resize((img_size, img_size)),
            transforms.ToTensor()
        ])
        
        train_dataset = datasets.MNIST('./data', train=True, download=True, transform=transform)
        test_dataset = datasets.MNIST('./data', train=False, download=True, transform=transform)
        n_classes = 10
        in_channels = 1
        
    else:
        raise ValueError(f"지원하지 않는 데이터셋: {dataset_name}")
        
    # 데이터로더 생성
    train_loader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True, num_workers=4, pin_memory=True)
    test_loader = DataLoader(test_dataset, batch_size=batch_size, shuffle=False, num_workers=4, pin_memory=True)
    
    return train_loader, test_loader, n_classes, in_channels
```

torchvision의 데이터셋에는 minst, cifar10 등 다양하게 있으니까 저런식으로 구별해놨다, 

```py
train_dataset = datasets.CIFAR10('./data', train=True, download=True, transform=transform)
```

download=True 매개변수와 함께 사용되면, PyTorch는 지정된 경로(여기서는 './data')에 CIFAR10 데이터셋을 다운로드하고 저장

#### shuffle: 데이터 다양성을 통한 학습 안정화

shuffle 매개변수는 매 에포크마다 데이터의 순서를 무작위로 섞을지 여부를 결정합니다. 이는 단순한 옵션처럼 보이지만, 모델 학습에 미치는 영향은 상당합니다.

##### 왜 데이터를 섞어야 할까요?

데이터를 섞지 않으면 모델은 항상 같은 순서로 데이터를 학습하게 됩니다. 이는 두 가지 문제를 일으킬 수 있습니다:

1. 순서 편향(Order Bias): 모델이 데이터의 순서 패턴을 학습하여 일반화 능력이 저하될 수 있습니다.

1. 국소적 최적화(Local Optimization): 비슷한 샘플이 연속적으로 처리되면 모델이 특정 유형의 데이터에 과적합될 위험이 있습니다.

훈련 데이터의 경우 shuffle=True로 설정하여 모델이 매 에포크마다 다른 순서로 데이터를 학습하게 하는 것이 바람직합니다. 반면 테스트나 검증 데이터에서는 일관된 평가를 위해 shuffle=False로 설정하는 것이 일반적입니다.

#### num_workers: 병렬 처리로 데이터 로딩 가속화

num_workers는 데이터를 로드하는 데 사용할 병렬 프로세스의 수를 지정합니다. 이 매개변수는 특히 대규모 데이터셋이나 복잡한 전처리 과정이 필요한 경우 학습 속도를 크게 향상시킬 수 있습니다.

##### 최적의 num_workers 값은?

num_workers의 최적값은 시스템 환경과 데이터셋에 따라 달라집니다:

- 너무 적으면: 데이터 로딩이 병목 현상을 일으켜 GPU가 유휴 상태로 대기하는 시간이 발생합니다.

- 너무 많으면: 시스템 리소스를 과도하게 사용하여 오히려 전체 성능이 저하될 수 있습니다.

일반적으로 시스템의 CPU 코어 수에 맞추거나 약간 적게 설정하는 것이 좋습니다. 예를 들어 8코어 시스템에서는 4-6개의 워커가 적절할 수 있습니다. 성능 최적화를 위해서는 다양한 값으로 실험해 보는 것이 좋습니다.

#### pin_memory: GPU 전송 최적화

pin_memory는 CPU 메모리 페이지를 고정(pinning)하여 CPU에서 GPU로의 데이터 전송을 가속화하는 옵션입니다. 이 옵션은 GPU 학습 시 특히 유용합니다.

##### 동작 원리

일반적으로 CPU 메모리는 가상 메모리 시스템에 의해 관리되며, 물리적 위치가 변경될 수 있습니다. 그러나 GPU로 데이터를 전송할 때는 메모리의 물리적 위치가 고정되어 있어야 효율적입니다.

pin_memory=True로 설정하면:

1. 텐서가 페이지 잠금(page-locked) 메모리에 할당됩니다.

1. 이 메모리는 스왑되거나 재배치되지 않습니다.

1. 결과적으로 CPU에서 GPU로의 데이터 전송이 빨라집니다.

GPU 학습 시에는 pin_memory=True로 설정하는 것이 성능 향상에 도움이 됩니다. 단, CPU 메모리 사용량이 약간 증가할 수 있으므로 메모리가 제한된 환경에서는 주의해야 합니다.









