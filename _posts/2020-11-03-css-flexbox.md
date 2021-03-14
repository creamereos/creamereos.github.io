---  
layout: post
title: CSS - flexbox
subtitle: 
categories: dev
tags: css flexbox
comments: true  
--- 

# flex box

## [사용 순서]

### 1. display : flex; 선언

![스크린샷 2020-11-03 14.06.10](https://i.imgur.com/OXNdDka.png)

정렬을 하고자하는 요소들을 지니고 있는 부모의 값에 display : flex 선언.

~~~
display : flex;

display : inline-flex;
~~~

### 2. flex-direction : 정렬 방향 설정
가로 또는 세로 정렬 설정.

~~~
가로 방향
flex-direction : row;

세로 방향
flex-direction : column;

가로 방향 역순
flex-direction : row-revers;

세로 방향 역순
flex-direction : column-revers;
~~~

### 2.1 axis : 축

![스크린샷 2020-11-03 14.10.35](https://i.imgur.com/5Z5jz3W.png)

- 가로 방향 정렬
  Main axis : 정렬 방향
  Cross axis : 메인과 수직

~~~
flex-direction : row;
~~~

![스크린샷 2020-11-03 14.10.21](https://i.imgur.com/uLrExn2.png)

- 세로 방향 정렬
  Main axis : 정렬 방향
  Cross axis : 메인과 수직

~~~
flex-direction : column;
~~~

![스크린샷 2020-11-03 14.13.32](https://i.imgur.com/fSIhtnF.png)

- 역순 가로 방향 정렬
  Main axis : 정렬 방향
  Cross axis : 메인과 수직

~~~
flex-direction : row-reverse;
~~~

![스크린샷 2020-11-03 14.15.29](https://i.imgur.com/Oggdgom.png)

- 역순 세로 방향 정렬
  Main axis : 정렬 방향
  Cross axis : 메인과 수직

~~~
flex-direction : column-reverse;
~~~

### flex-wrap : 한줄 정렬 or 여러줄 정렬.

- nowrap : 한줄 정렬

![스크린샷 2020-11-03 14.17.47](https://i.imgur.com/MAGm3mv.png)

~~~
flex-wrap : nowrap;
~~~

- wrap : 여러줄 정렬

![스크린샷 2020-11-03 14.19.02](https://i.imgur.com/1ix7aKC.png)

~~~
flex-wrap : wrap;
~~~

# 축(axis)을 기준으로 정렬하는 CSS Property

![스크린샷 2020-11-03 14.21.12](https://i.imgur.com/tFPPF82.png)

# justify-content

- Main axis 기준으로 정렬

~~~
flex-direction : row;

가운데 정렬
justify-content : center;

시작점(왼쪽) 정렬 (row-reverse인 경우 시작점은 오른쪽)
justify-content : flex-start;

시작점(오른쪽) 정렬 (row-reverse인 경우 시작점은 왼쪽)
justify-content : flex-end;

각 요소 간격들을 동일하게 정렬
justify-content : space-between;

각 요소들의 양 옆 간격을 동일하게 조절
justify-content : space-around;
~~~

# align-items (align-content)

- Cross axis 기준으로 정렬

- align-items : 하나의 flex 라인에 흐르는 cross axis를 기준으로 설정.

~~~
flex-direction : row;

가운데 정렬
Cross axis : center;

시작점(위쪽) 정렬 (row-reverse인 경우 시작점은 아래쪽)
justify-content : flex-start;

시작점(아래쪽) 정렬 (row-reverse인 경우 시작점은 위쪽)
justify-content : flex-end;
~~~

- align-content : flex-wrap이 wrap이라 여러줄이 생기면 align-content 사용해야한다.

- align-content에서만 사용 가능

~~~
각 요소 간격들을 동일하게 정렬
justify-content : space-between;

각 요소들의 양 옆 간격을 동일하게 조절
justify-content : space-around;
~~~

# 실무 꿀팁 : 선 align-items 후 align-content.

먼저 align-items를 사용 이상하면 그 다음 align-content.

# order

![스크린샷 2020-11-03 14.43.13](https://i.imgur.com/0DS291A.png)

order 값을 주어서 요소의 순서를 자유롭게 바꿀 수 있다.


order: 1;
order: 3;
order: 2;
~~~
