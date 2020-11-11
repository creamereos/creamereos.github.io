---  
layout: post
title: HTML - Viewport CSS - media query
subtitle: 
categories: dev
tags: css html media mediaquery 
comments: true  
--- 

# viewport


# Viewport
뷰포트(Viewport) : 사용자에게 보여지는 웹페이지의 영역으로 반응형 웹을 만들 때 고려되는 사항이다. 반응형 웹(Responsive Web)은 모바일, 테블릿, PC 등 다양한 디바이스의 디스플레이에 맞춰 뷰포트가 변하는 웹 사이트를 말하는 것이다. 즉, 화면이 바뀔 때마다 화면에 맞춰 스타일이 바뀌게 되는 웹 형태를 말한다.

<meta>태그로 각 디바이스에 따른 뷰포트에 맞춰 렌더링 영역을 설정할 수 있다.

~~~
<meta name="viewport" content="width=device-width, initial-scale=1.0">
~~~
- content=width=device-width : 페이지의 너비를 기기의 뷰포트 너비대로 설정
- initial-scale=1.0 : 처음 페이지 로딩 시 확대/축소가 되지 않은 원래의 페이지 사이즈를 사용하도록 설정

### Viewport 기준 단위

- vw(Viewport Width) : 뷰포트의 width 기준

- vh(Viewport Height) :	뷰포트의 height 기준

- vmin(Viewport Minimum) : 뷰포트의 width와 height 중 작은 수치를 기준

~~~
ex code)
viewport 1 : width: 1200px, height:800px;

> 10vw : 120px / 10vh : 80px

> 10vmax : 120px / 10vmin : 80px
~~~

### viewport 단위와 %의 다른 점

- % 단위 : 자식의 너비와 높이는 부모에 의해 결정

- viewport 단위 : 부모와 상관없이 viewport에 의해 결정

# media query
resposive web(반응형 웹)을 만들기 위해 필요한 CSS 선언.

~~~
syntax:
@media screen and (조건){
  CSS 선언
}

ex:
@media screen and (min-width: 700px) and (max-width: 1200px){

}

- min-width: 이상
- max-width: 이하

가로가 최소 700이상 1200 이하이면 이 스타일을 적용.
~~~
