---  
layout: post
title: JS - 숫자와 문자
subtitle:
categories: dev
tags: js
comments: true  
---     

### Number

- Math.pow : 제곱

```javascript
Math.pow(3,2)
//9
```

- Math.sqrt: 루트

```javascript
- Math.sqrt(9)
//3
```

- Math.round : 반올림

```javascript
Math.round(10.5)
//11
```

- Math.ceil : 올림

```javascript
Math.ceil(10.5)
// 11
```

- Math.floor : 내림

```javascript
Math.pow(10.5)
// 10
```

- Math.random : 랜덤 

```javascript
Math.random()
// Random

Math.random() * n
// n이하의 난수(소수)

Math.round(Math.random() * n)
// n이하의 난수(정수)
```

### String

- escape : \역슬래시 뒤에 나온 것은 기호가 아닌 문자로 해석

```javascript
console.log('creamer's coding everybody')

// SyntaxError: missing ) after argument list

console.log('creamer\'s coding everybody')

//creamer's coding everybody
```

- 줄 바꿈

\n

```javascript
console.log('hello\nworld')

//hello
world
```

- 문자 더하기

+

```javascript
console.log('hello' +' '+ 'world')
//hello world

console.log('1'+'1')
// 11

console.log(1+1)
// 2
```
1(숫자형:number)과 '1'(문자열:string)은 같지 않다.

```javascript
typeof '1'
// string

typeof 1
// number
```

- indexOf string의 index 찾기

```javascript
'hello'.indexOf('h')
// 0

'hello'.indexOf('e')
// 1

'hello'.indexOf('l')
// 2 (l이 2개가 아니라 가장 처음에 찾은 l이 2번째 index에 있다.)

'hello'.indexOf('o')
// 4
```
