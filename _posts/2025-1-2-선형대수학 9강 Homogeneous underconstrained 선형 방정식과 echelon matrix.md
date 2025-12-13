---
key:
title: '선형대수학 Homogeneous underconstrained linear system equations and Echelon matrix'
excerpt: '딥러닝을 위해 필요한 내용을 다룹니다.'
tags: [선형대수학]





---

Under cconstrained한 경우, 즉 미지수가 방정식보다 많은 경우에 해는 무수히 많이 존재하고, 그 때 그 해는 아무렇게나 있는게 아니라 해 집합이 따로 있었다, 그 해 집합은 선형결합의 형태로 표현이 됀다고 했었고 그걸 벡터공간으로 생각 할 수 있었다, 그 중에서 영벡터공간, null스페이스를 구하는 방법에 대해 배운다.

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101000542924.png" alt="image-20240101000542924" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101000743912.png" alt="image-20240101000743912" style="zoom:67%;" />

이럴때 저 맨 아래에 있는 행렬을 엡실론 매트릭스라 한다, 위치를 바꾸는 퍼미션도 당연히 된다,

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101001056814.png" alt="image-20240101001056814" style="zoom:67%;" />

이런식으로 구했던 엡실론 매트릭스에 피봇이 있는 cloum에 피봇 자리는 1, 나머지는 0을 만드는 reduce form형태로 만든다.

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101001507544.png" alt="image-20240101001507544" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101210314222.png" alt="image-20240101210314222" style="zoom:67%;" />

Pivot 변수와 free변수 두 가지가 reduce echelon matrixd에 존재한다, 그리고 행에는 각 pivot 변수가 꼭 하나씩만 오게 된다, 애초에 pivot이 그 행에서 기준인 변수이기 때문에 하나씩만 있는건 당연하다, 그리고 식을 저렇게 pivot에 대하여 정리하는 식으로 만든다.

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101210657264.png" alt="image-20240101210657264" style="zoom:67%;" />

저 두개의 연립방정식은 결국 x라는 벡터를 u와 w에 대해 정리를 해서 관계식을 구했기 때문에 미지수를 줄일 수 있게 된다, 그리고 저 덧셈 벡터들을 또 v와 y의 벡터들의 선형결합으로 나눌 수 있고, v라는 변수를 가진 [-3,1,0,0]벡터와 y라는 변수를 가진[1,0,-1,1]벡터가 만들어내는 선형결합의 벡터들이(벡터공간이) underconstrained(미지수가 방정식보다 많을 경우)에서 Ax=0의 솔루션이다.

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101211400888.png" alt="image-20240101211400888" style="zoom:67%;" />

정리를 하자면, 솔루션(해)은 두개의 벡터의 모든 선형결합들인데, Ax=0 형태의 경우만이니까 이 두 벡터들의 선형결합은 null space라는 것이다. 이게 해집합의 정답인거다.

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101222027514.png" alt="image-20240101222027514" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101222307314.png" alt="image-20240101222307314" style="zoom:67%;" />







