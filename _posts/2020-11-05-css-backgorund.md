---  
layout: post
title: CSS - back ground
subtitle: 
categories: dev
tags: css background
comments: true  
--- 

# background

- background-color : 배경색 (hex/rgb/rgba)

- background-image : 이미지를 배경으로 넣기 url()

~~~
.div {
  background-image: url('/assets/img.png');
  또는
    background-image: url('이미지 주소 복사');
}
~~~

- background-repeat : repeat(이미지를 반복해서 넣기) / no-repeat

~~~
.div {
  background-reapet: repeat
  background-image: url('이미지 주소 복사');
}
~~~

- background-size: contain(요소 안에 이미지 전체가 다 들어감) / cover(요소 사이즈로 이미지 자르기) / custom(사이즈 정의)
백그라운드 이미지의 사이즈

~~~
background-size: auto 80% / contain / cover
~~~

- background-position: x / y

~~~
background-position: center center;
~~~
