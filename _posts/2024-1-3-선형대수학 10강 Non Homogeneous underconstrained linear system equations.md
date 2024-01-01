---
key:
title: '선형대수학 10강 non Homogeneous underconstrained linear system equations and Echelon matrix'
excerpt: '딥러닝을 위해 필요한 내용을 다룹니다.'
tags: [선형대수학, 딥러닝]






---

이번에는 미지수가 방정식보다 많을 때 Ax=d인 경우의 해를 구하는 방식을 다룬다. 

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101223516629.png" alt="image-20240101223516629" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101223959133.png" alt="image-20240101223959133" style="zoom:67%;" />

(free 변수에 대한 선형결합으로 나타내야 하므로 w가 아니라 y입니다.)앞의 강의와 하는 과정이 똑같은데, 여기서 스페셜 솔루션(벡터의 선형결합으로 나타난 해집합)에 추가로 상수항으로 이루어진 particular sol.이 붙는다. 이 때 상수항들의 값은 pivot이 있는 행의 d값이라는걸 알 수가 있다. 그레서 u자리에 -2가, w자리에 1이 들어가는거다. 즉 [-2,0,1,0]이라는 4차원 벡터를 [-3,1,0,0]과 [1,0,-1,1] 이 두개의 벡터의 선형결합으로 만드는 벡터공간에 있는 모든 원소들을 더한다고 생각할 수 있다, 그거는 벡터공간을 **평행이동**하는거라고 생각 할 수 있다. 

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101224646064.png" alt="image-20240101224646064" style="zoom:67%;" />

이 때 벡터공간은 자기들 원소들로 표현을 더 이상 못하게 되고, 자기들 끼리의 원소로 선형결합을 하여 모든 벡터공간을 표현하지 못하므로(상수항이 생겼으므로), 그래서 선형성이 깨지게 된다. 사실 선형성이 깨졌기 때문에 그 때의 해 집합은 벡터공간이 아니라 **벡터집합**이라고 해야 맞는 표현이다.

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101225012924.png" alt="image-20240101225012924" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101225200042.png" alt="image-20240101225200042" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/kirity1/blogimages/main/uPic/image-20240101225923408.png" alt="image-20240101225923408" style="zoom:67%;" />

이 때 스페셜 솔루션(null space)부분을 구할 때 free 변수에 대해 선형결합을 표현 할 때, 열 벡터 기준으로 위에 있는 수들을 부호만 바꿔주면 된다, 이 부분은 pivot 변수에 대한 연립 방정식으로 표현하고 나서 w와 y로 바꿀 때 과정을 생각하면 당연하다.

