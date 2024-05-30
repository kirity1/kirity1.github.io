---
key:
title: '리엑트 rfce와 farce의 차이점'
excerpt: '리엑트 정리합니다.'
tags: [react]
---

## rfce rafce같은 예약어 쓰는 방법

리엑트에서 컴포넌트를 새로 만들때 간편하게 예약어를 쓸 수 있는데

<img src="https://raw.githubusercontent.com/kirity1/blogimg/master/uPic/image-20240414154448109.png" alt="image-20240414154448109" style="zoom:67%;" />

extesion에서 es7+를 다운 받아서 사용하면 된다.

## rafce

<img src="https://raw.githubusercontent.com/kirity1/blogimg/master/uPic/image-20240414154631462.png" alt="image-20240414154631462" style="zoom:67%;" />

rafce를 써보면 이렇게 함수형 컴포넌트가 나오는데 여기서 a가 arrow, 즉 화살표인 익명함수이다

## rfce

<img src="https://raw.githubusercontent.com/kirity1/blogimg/master/uPic/image-20240414155527358.png" alt="image-20240414155527358" style="zoom:67%;" />

- **스코프 및 컨텍스트**:
  - **rfce**: 함수 선언 방식은 자체적인 스코프를 가지며, 함수 내부에서의 `this`는 해당 컴포넌트 자체를 가리킴
  - **rafce**: 화살표 함수는 렉시컬 스코프를 가지고 있어, 외부 스코프를 상속받습니다. 따라서 내부에서의 `this`는 함수가 정의된 위치의 외부 스코프를 가리킴, 즉 외부에서 이걸 불러온다는 말, 여기서 this는 외부의 것을 가르킨다는 말
- **코드 길이**:
  - **rafce**: 화살표 함수의 간결한 문법을 사용하기 때문에, 코드가 더 짧고 간결
  - **rfce**: 함수 선언문을 사용하기 때문에, 함수 이름과 `function` 키워드 등이 추가로 필요하여 코드가 더 길어질 수 있음
- **반환값의 암시성**:
  - **rafce**: 화살표 함수의 암시적 반환(Implicit Return)을 활용할 수 있습니다. 이는 함수 내부에서 별도로 `return` 키워드를 사용하지 않아도 한 줄로 반환값을 표현할 수 있음
  - **rfce**: 명시적으로 `return` 키워드를 사용하여 반환값을 지정해주어야 함



여기서 스코프는 전역 변수, 지역 변수할때 그거 비슷한 거로 생각하면 될 듯 하다.