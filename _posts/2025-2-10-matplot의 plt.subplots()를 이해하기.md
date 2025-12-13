---
key:
title: 'plt.subplots()에 대해 이해하기'
excerpt: 'matpolt 이해'
tags: [인공지능]
---

인공지능을 공부하던 중, 파이썬으로 matplotlib를 이용하다보니, plt.subplots()를 쓰게 됐다, 그래서 이 메소드에 대해서 자세히 알아보고자 한다.

먼저 플롯(plot)이란 데이터가 그려진 그래프 비스무리한 그것이라 생각하면 될 듯하다. 즉 이 함수는 그래프를 그릴 때 쓰인다.

그래서 matplot이라는 이름도 plot이라는 형태로 수학적인 데이터들을 표현한다는 의미인거같다

그러면 `plt.subplots()` 메소드를 사용하여 이미지(데이터)를 시각화하는 방법을 배운다

`matplotlib` 라이브러리의 중요한 메소드 중 하나로, 한 번에 여러 개의 서브플롯을 생성할 수 있다. 이 메소드는 다음과 같은 두 가지 주요 객체를 반환한다:

- **Figure 객체**: 전체 플롯의 컨테이너 역할
- **Axes 객체 배열**: 실제 플롯이 그려지는 개별 영역을 나타냄
- <img src="https://matplotlib.org/stable/_images/sphx_glr_arranging_axes_002.png" alt="Arranging multiple Axes in a Figure — Matplotlib 3.9.0 documentation"  />

## 이런 느낌으로 subplots안에는 figure라는 프레임 안에 axes 객체들이 지금 같은 경우에는 2,2 행렬로 있다.

```py
import matplotlib.pyplot as plt

def plot_img(images, titles):
    # plt.subplots()를 사용하여 1행 n열의 서브플롯을 생성합니다.
    fig, axs = plt.subplots(nrows=1, ncols=len(images), figsize=(20, 20))
    
    # 이미지가 하나일 경우 axs를 리스트로 변환합니다.
    if len(images) == 1:
        axs = [axs]
    
    # 각 축(ax)에 이미지를 표시하고 제목을 설정합니다.
    for i, ax in enumerate(axs):
        ax.set_title(titles[i])
        ax.imshow(images[i], cmap='gray')
    
    # 모든 서브플롯을 표시합니다.
    plt.show()

```

`nrows=1`와 `ncols=len(images)`를 사용하면 한 행에 여러 개의 이미지를 나란히 배치,

이미지가 하나일 경우 그냥 그 axs를 하나의 리스트로 변환해 반환해서 이 리스트로 그래프를 그린다.

만약 이미지가 여러 개 일 경우, 각 축(ax)에 축들의 모음(axs)를 enumerate로 루프를 돌아서 각각의 ax마다 title과 imshow메소드를 통해서 이미지를 표시한다, 그 후 그려진 fig를 plt.show()메소드를 통해서 모든 서브플롯을 표시하는거다.