---  
layout: post
title: JS - 정규 표현식
subtitle:
categories: dev
tags: js
comments: true
---

문자열에서 특정한 문자를 찾아내는 툴.

### 컴파일 : 패턴 생성 방법

1. 정규표현식 리터럴

```javascript
const pattern = /찾고자 하는 문자/;

const pattern1 = /"a"/;
```

2. 정규표현식 객체 생성자

```javascript
const pattern1 = new RegEXP("a");
```

### 정규표현식 메소드 실행 방법

- RegExp.exec() : 추출

```javascript
변수명.exec('문자열');

pattern1.exec('abcdef'); 
// ["a"]

pattern1.exec('bcdefg'));
// null (a가 없을 때)

const pattern2 = new RegEXP("a.");
// .은 1개의 문자. 즉 a와 a뒤의 문자 1개

pattern2.exec('abcdef'));
// ["ab"]
```

- RegExp.test() : 존재 유무 테스트
test는 인자 안에 pattern1에 해당되는 문자열이 있으면 true, 없으면 false를 return.

```javascript
pattern1.test('abcdef'); 
// true

pattern1.test('bcdefg'); 
// false
```

### 문자열 메소드 실행 방법

- String.match 
RegExp.exec()와 비슷.

```javascript
문자열.match(정규 표현식 변수);

'abcdef'.match(pattern)); // ["a"]

'bcdefg'.match(pattern); // null
```

- String.replace()
문자열에서 패턴을 검색해서 이를 변경한 후에 변경된 값을 리턴한다.

```javascript
문자열.replace(정규 표현식 변수. 변경 후 문자열);

'abcdef'.replace(pattern1, 'A');  // Abcdef
```

### 옵션 
 정규표현식 패턴을 만들 때 옵션을 설정할 수 있다. 옵션에 따라서 검출되는 데이터가 달라진다.

- i : i를 붙이면 대소문자를 구분하지 않는다.

```javascript
let xi = /a/;
console.log("Abcde".match(xi)); // null
let oi = /a/i;
console.log("Abcde".match(oi)); // ["A"];
```

g : g를 붙이면 검색된 모든 결과를 리턴한다.

```javascript
const xg = /a/;
console.log("abcdea".match(xg));
//["a"]
const og = /a/g;
console.log("abcdea".match(og));
//["a", "a,"]
```

ig : i와 g를 붙이면 대소문자 구분 없이 검색된 모든 결과를 리턴

```javascript
const og = /a/ig;
console.log("abcdeA".match(og));
//["a", "A,"]
```

### 캡쳐
괄호안의 패턴은 마치 변수처럼 재사용할 수 있다. 이 때 기호 $를 사용.

```javascript
const pattern = /(\문자+)\s(\문자+)/;
// \문자 = 1개 이상의 문자, \s = 띄어쓰기
const str = "Hello World";
// 위 pattern과 형식이 같음.>>
var result = str.replace(pattern, "$2, $1");
// $2 = 패턴의 2번째 괄호, $1 = 패턴의 1번째 괄호
console.log(result);
// World, Hello
```