---  
layout: post
title: JS - stack
categories: dev
tags: js
comments: true
---

stack : 가장 윗부분에서만 자료의 추가와 삭제가 일어나므로 실행속도가 빠르고 구현이 쉬운 효율적인 자료구조.

![](/assets/img/post/2020-11-29-20-35-56.png)

- stack은 element list로 구성되며 TOP 이라 불리는 list의 한쪽 끝으로만 element에 접근할수있다.

- LIFO; Last-in, First-out 자료구조 : 나중에 입력하는게 가장 먼저 나온다.이런 특성 때문에 스택의 탑에 있지 않은 요소에는 접근할수없다. 스택의 밑바닥에 있는 요소에 접근하려면 모든요소를 제거하는 수밖에 없다.

- stack에 element 추가 : push

- stack에 element 제거 : POP (마지막 element 꺼내기)

- stack의 TOP 요소 확인 : PEEK



