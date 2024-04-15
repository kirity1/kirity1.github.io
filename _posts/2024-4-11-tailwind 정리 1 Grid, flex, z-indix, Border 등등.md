---
key:
title: 'tailwind 사용법 정리'
excerpt: '리엑트 위한 tailwind 내용을 정리합니다.'
tags: [css]
---

### flex

굳이 엘리먼트의 width height를 정하지 않더라도 박스 레이아웃을 만들고 그 안에 얼마나 영역을 차지할 것인지를 정하여 창 크기가 변함에 따라 알아서 크기를 조절하게 해줌

Display:flex;로 적용함

### Display & flex-direction

컨텐츠들을 행으로 혹은 열로 동적으로 나열할건지에 대한거

### flex wrap

<img src="/Users/gimjun-yeong/Library/Application Support/typora-user-images/image-20240415120451429.png" alt="image-20240415120451429" style="zoom:67%;" />

그냥 하면 자식요소가 꽉 찼을 때 넘어가는데 wrap으로 자동 줄 바꿈 해주는거

### z-index

태그들이 서로 겹칠 때 우선적으로 먼저 보여질 거를 정하는 순서같은거

### Boder 속성자들

solid : **경계선을 실선으로 표시**합니다. dotted : 경계선을 점선으로 표시합니다. dashed : 경계선을 짧은 직선으로 표시합니다. double : 경계선을 이중으로 **표시합니다**



### Grid

요소들을 행과 열의 그리드로 정렬할 수 있으며, 복잡한 레이아웃을 쉽게 작성할 수 있습니다.

먼저, Grid를 사용하기 위해서는 부모 요소에 `display: grid;` 속성을 적용해야 합니다. 이렇게 하면 그 요소의 자식 요소들이 그리드 아이템으로 작동하게 됩니다.

그리드는 행(row)과 열(column)로 구성됩니다. `grid-template-rows` 및 `grid-template-columns` 속성을 사용하여 행과 열의 크기를 정의할 수 있습니다. 예를 들어, 다음과 같이 작성할 수 있습니다.

```
cssCopy code
.grid-container {
  display: grid;
  grid-template-rows: 100px 200px; /* 높이가 100px와 200px인 두 개의 행 */
  grid-template-columns: 1fr 2fr; /* 1:2 비율로 두 개의 열 */
}
```

그리드 아이템은 부모 그리드 컨테이너의 셀(cell)에 배치됩니다. `grid-row` 및 `grid-column` 속성을 사용하여 각각의 아이템이 어느 행과 열에 위치할지 지정할 수 있습니다.

```
cssCopy code
.item {
  grid-row: 1 / 3; /* 첫 번째 행부터 세 번째 행까지 */
  grid-column: 2 / 3; /* 두 번째 열부터 세 번째 열까지 */
}
```

또한, Grid는 자동으로 그리드 아이템들을 배치하고 조절하는데, 이를 통해 반응형 레이아웃을 쉽게 구현할 수 있습니다. 예를 들어, `grid-template-rows` 및 `grid-template-columns` 속성을 사용하여 그리드 크기를 조정하거나, `grid-auto-rows` 및 `grid-auto-columns` 속성을 사용하여 그리드 아이템의 크기를 자동으로 조정할 수 있습니다.

### lg, md, sm

창 크기가 크거나, 작거나 할 때에 따라서 텍스트나 미디어, 등등의 크기를 줄이거나 하게 만듬 md: ~~~ 이렇게





### Grid

<img src="https://raw.githubusercontent.com/kirity1/blogimg/master/uPic/image-20240414164906131.png" alt="image-20240414164906131" style="zoom:67%;" />

grid는 저렇게 하나의 컨테이너를 만들고 그 안에서 자식속성들을 2차원의 형태로 나열한다, 그냥 그렇구나 싶을 수 있지만 

### Col-span,start,end 

<img src="https://raw.githubusercontent.com/kirity1/blogimg/master/uPic/image-20240415115213597.png" alt="image-20240415115213597" style="zoom:67%;" />

이렇게 특정 자식을 2개의 영역을 차지하게 만들 수도 있다,

<img src="https://raw.githubusercontent.com/kirity1/blogimg/master/uPic/image-20240415115426577.png" alt="image-20240415115426577" style="zoom:67%;" />

내가 지정하고 싶은 만큼 start와 end를 지정해서 영역을 나타낼 수 도 있다.
