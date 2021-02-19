---  
layout: post
title: JS - array concat
subtitle:
categories: dev
tags: js
comments: true
---
array : 연관된 데이터를 모아서 통으로 관리하기 위해서 사용하는 데이터 타입. 변수가 하나의 데이터를 저장하기 위한 것이라면 배열은 여러 개의 데이터를 하나의 변수에 저장하기 위한 것.

- concat : 다수의 elements를 array에 추가

```javascript
let li = ['a', 'b', 'c', 'd', 'e'];
li = li.concat(['f', 'g']);
li // ['a', 'b', 'c', 'd', 'e', 'f', 'g']
```
- unshift : array의 시작점에 element를 array에 추가

```javascript
let li = ['a', 'b', 'c', 'd', 'e'];
li.unshift('z');

li //['z', 'a', 'b', 'c', 'd', 'e']
```

- splice : array의 특정구간을 추출하거나, 특정구간에 특정 array를 추가. . 대상 구간에 해당하는 삭제될 elements는 리턴된다. 원본 array가 수정된다. (slice는 원본 array 유지)

```javascript
- syntax
let array = ['a', 'b', 'c', 'd', 'e'];
array.splice(array의 index 번호, 부터 삭제할 갯수, '사이에 넣을 elements(다수 가능)');

let li = ['a', 'b', 'c', 'd', 'e'];
li.splice(2, 0, 'X');

li // ["a", "b", "X", "c", "d", "e"]
```

- shift : array의 첫번째 elemnet를 제거

```javascript
let li = ['a', 'b', 'c', 'd', 'e'];
li.shift();
'a'

li // ['b', 'c', 'd', 'e'];
```

- pop : array의 마지막 elemnet를 제거

```javascript
let li = ['a', 'b', 'c', 'd', 'e'];
li.pop();
'e'

li // ['a', 'b', 'c', 'd'
```

- sort : 오름차순 정렬

```javascript
let li = ['c', 'e', 'a', 'b', 'd'];
li.sort();
["a", "b", "c", "d", "e"]
```

- reverse : 역순 정렬

```javascript
let li = ['c', 'e', 'a', 'b', 'd'];
li.reverse();

li // ["d", "b", "a", "e", "c"]
```