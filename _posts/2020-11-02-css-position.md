---  
layout: post
title: CSS - position
subtitle: 
categories: dev
tags: css positon
comments: true  
--- 

# position
- element를 원하는 위치에 자유롭게 이동시키기 위한 property.

*property : html DOM 안에서 attribute 를 가리키는(혹은 대신하는) 표현

[attribute vs property]

<table>
  <tr>
    <th> attribute </th>
    <th> property </th>
  </tr>
  <tr>
    <td>html document/file 안에 존재</td>
    <td>html DOM tree안에서 존재</td>
  </tr>
  <tr>
    <th>정적. 값이 변하지 않음</th>
    <th>동적. 값이 변할 수 있음</th>
  </tr>
</table>

# position Type 별 기준점

### static
- 모든 element의 기본 postion 값.

### relative
- 기준점 : 원래 있던 position

### absolute
- 길막을 못하는 block으로 변경. (float과 비슷)
- 기준점 :postion이 static이 아닌 element(position:  , absolute, fixed, sticky) 중 원하는 element를 기준으로 설정 가능. 보통 relative를 기준으로 설정

### fixed
- absolute와 비슷하지만 기준점은 다름
- 기준점 : viewport(브라우저 전체 화면) 기준으로 설정.

### CSS Position 관련 property

##### top, right, left, bottom
- top, bottom 둘 중 가까운 쪽을 사용
- left, right 둘 중 가까운 쪽을 사용

##### z-index
수직 방향의 위치
![스크린샷 2020-11-02 16.54.18](https://i.imgur.com/V9MxkRs.png)

# postion function

### transform

- translae
현재 위치한 기준점을 기준으로 요소를 x, y, z 축으로 이동한다. (position:relative를 이용해 대체 가능)

~~~
transform : translate;(x축으로 이동할 거리, y축으로 이동할 거리)

transform : translateY;(y축으로 이동할 거리)

transform : translateX;(x축으로 이동할 거리)

transform : translateZ;(z축 즉 앞뒤로 이동할 거리) // Z축은 3D이기 때문에 perspective : 원근감값 을 지정해 주어야 한다.
~~~

~~~
ex)
position : absolute;
right : 0px;
top : 50%;

/* 가운데 배치를 위해 사용 */
transform: translateY(-50%);
~~~

- skew
x, y 축으로 요소를 비튼다

~~~
- transform : skew;(x축으로 비틀정도deg, y축으로 비틀정도 deg)

- transform : skewX;(x축으로 비틀정도deg)

- transform : skewY;(y축으로 비틀정도 deg)
~~~



- scale
x, y 축으로 대상을 확대한다.


~~~
 transform : scale(1.5);

 transform : scaleX(2);

 transform : scaleY(0.5);
~~~


- roate
x, y 축을 기준으로 대상을 회전한다.

~~~
 transform : roate(x축 회전 deg, y축 회전 deg)

 transform : roateX(x축 회전 정도 deg)

 transform : roateY(y축 회전 정도 deg)
~~~
