---  
layout: post
title: JS - 2차원 array
categories: dev
tags: javascript
comments: true
---

array는 1차원 array, 2차원 array,~ n차원 배열이 있을 수 있다. 3차원 array부터는 너무 복잡해서 잘 사용하지 않는다. 복잡하게 작성하는 것은 좋은 방법이 아니며 쉬운 방법으로 프로그래밍을 코딩하여 프로그램 가독성을 높여야 한다.

2차원 array : array 안에 array로 2차원 배열은 열과 행이 있는 공간에 데이터를 저장한다.

- 2차원 array

```javascript
let A = new Array( new Array(3), new Array(3));

A[0][0] = 1;
A[0][1] = 2;
A[0][2] = 3;
A[1][0] = 4;
A[1][1] = 5;
A[1][2] = 6;

A = [[1,2,3],[4,5,6]];
```

- 2차원 array의 legnth

```javascript
A.length // 2
A[0].length // 3
A[1].length // 3
```

- 중첩 for : 2차원 array 읽기

```javascript
for(let i=0; i < A.length; i++){
    for(let j=0; j< A[i].length; j++){
        console.log(A[i][j]);
    }  
}
//1
//2
//3
//4
//5
//6
```

- for in : 2차원 array 읽기

```javascript
for(let i in A){
    console.log(A[i]);
}
// [1,2,3]
// [4,5,6]
```
