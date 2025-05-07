---
key:
title: 'pixelcnn-2 전처리마스크랑 모델구축'
excerpt: 'pixelcnn'
tags: [pixelcnn]
---

```py
# 네 번째 셀: 모델 구현 (설정 객체 활용하도록 수정)
class MaskedConv2d(nn.Conv2d):
    def __init__(self, in_channels, out_channels, kernel_size, mask_type='A', stride=1, padding='same', **kwargs):
        if padding == 'same':
            padding = kernel_size // 2
        super(MaskedConv2d, self).__init__(in_channels, out_channels, kernel_size, stride=stride, padding=padding, **kwargs)
        self.register_buffer('mask', torch.ones_like(self.weight))
        
        # 마스크 생성
        _, _, h, w = self.weight.size()
        self.mask[:, :, h//2, w//2 + 1:] = 0  # 오른쪽 마스킹
        self.mask[:, :, h//2 + 1:, :] = 0     # 아래쪽 마스킹
        
        # A 타입 마스크는 현재 픽셀도 마스킹 (B 타입은 현재 픽셀 포함)
        if mask_type == 'A':
            self.mask[:, :, h//2, w//2] = 0
            
    def forward(self, x):
        self.weight.data *= self.mask  # 가중치에 마스크 적용
        return super(MaskedConv2d, self).forward(x)

class ResidualBlock(nn.Module):
    def __init__(self, in_channels, out_channels, mask_type='B'):
        super(ResidualBlock, self).__init__()
        self.conv1 = MaskedConv2d(in_channels, out_channels, kernel_size=3, mask_type=mask_type)
        self.bn1 = nn.BatchNorm2d(out_channels)
        self.conv2 = MaskedConv2d(out_channels, out_channels, kernel_size=3, mask_type=mask_type)
        self.bn2 = nn.BatchNorm2d(out_channels)
        self.relu = nn.ReLU(inplace=True)
        
        # 채널 차원이 다를 경우 1x1 컨볼루션으로 맞춤
        if in_channels != out_channels:
            self.skip = nn.Sequential(
                nn.Conv2d(in_channels, out_channels, kernel_size=1),
                nn.BatchNorm2d(out_channels)
            )
        else:
            self.skip = nn.Identity()
        
    def forward(self, x):
        residual = x
        out = self.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        out += self.skip(residual)
        out = self.relu(out)
        return out

class PixelCNN(nn.Module):
    def __init__(self, input_channels=3, hidden_channels=128, n_residual=15, n_classes=None, kernel_size=7):
        super(PixelCNN, self).__init__()
        self.n_classes = n_classes
        self.input_channels = input_channels
        
        # 조건부 임베딩 (있을 경우)
        if n_classes:
            self.embedding = nn.Embedding(n_classes, hidden_channels)
        
        # 첫 번째 레이어는 'A' 타입 마스크 사용
        self.conv_in = MaskedConv2d(input_channels, hidden_channels, kernel_size=kernel_size, mask_type='A')
        self.bn_in = nn.BatchNorm2d(hidden_channels)
        
        # 레지듀얼 블록들
        self.residual_blocks = nn.ModuleList([
            ResidualBlock(hidden_channels, hidden_channels, mask_type='B')
            for _ in range(n_residual)
        ])
        
        # 출력 레이어
        self.relu = nn.ReLU(inplace=True)
        self.conv_out1 = MaskedConv2d(hidden_channels, hidden_channels, kernel_size=3, mask_type='B')
        self.bn_out1 = nn.BatchNorm2d(hidden_channels)
        self.conv_out2 = MaskedConv2d(hidden_channels, hidden_channels, kernel_size=3, mask_type='B')
        self.bn_out2 = nn.BatchNorm2d(hidden_channels)
        self.conv_out3 = MaskedConv2d(hidden_channels, input_channels * 256, kernel_size=1, mask_type='B')
        
    def forward(self, x, label=None):
        # [0, 255] 범위를 [0, 1]로 정규화
        if x.max() > 1:
            x = x / 255.0
        
        batch_size, _, height, width = x.shape
        
        # 초기 합성곱
        out = self.relu(self.bn_in(self.conv_in(x)))
        
        # 조건부 임베딩 추가 (있을 경우)
        if self.n_classes and label is not None:
            label_emb = self.embedding(label).view(batch_size, -1, 1, 1)
            out = out + label_emb
        
        # 레지듀얼 블록
        for block in self.residual_blocks:
            out = block(out)
        
        # 출력 레이어
        out = self.relu(self.bn_out1(self.conv_out1(out)))
        out = self.relu(self.bn_out2(self.conv_out2(out)))
        out = self.conv_out3(out)
        
        # 출력 형태 변환: [batch, channels*256, height, width] -> [batch, channels, height, width, 256]
        out = out.view(batch_size, self.input_channels, 256, height, width)
        out = out.permute(0, 1, 3, 4, 2)  # [batch, channels, height, width, 256]
        
        return out
```



