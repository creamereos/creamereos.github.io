---  
layout: post
title: JS - data type / reference`
categories: dev
tags: javascript
comments: true
---
데이터 타입이란 데이터의 형태를 의미한다. 데이터 타입은 크게 두가지로 구분할 수 있다. 객체와 객체가 아닌 것.

### 원시 데이터 타입(primitive type)
객체가 아닌 데이터 타입. (그 외의 모든 데이터 타입들은 객체다.)
- 숫자
- 문자열
- 불리언(true/false)
- null
- undefined

```javascript
var str = 'coding';
console.log(str.length);        // 6
console.log(str.charAt(0));     // "C"
```

문자열은 property와 method가 있으므로 객체다. 그런데 왜 문자열이 객체가 아니라고 할까? 그것은 내부적으로 문자열이 원시 데이터 타입이고 문자열과 관련된 어떤 작업을 하려고 할 때 **자바스크립트는 임시로 문자열 객체를 만들고 사용이 끝나면 제거**하기 때문이다. 이러한 처리는 내부적으로 일어난다. 그렇기 때문에 몰라도 된다. 하지만 원시 데이터 타입과 객체는 좀 다른 동작 방법을 가지고 있기 때문에 이들을 분별하는 것은 결국엔 필요하다.

```javascript
var str = 'coding';
str.prop = 'everybody';
console.log(str.prop);      // undefined
```

str.prop를 하는 순간에 자바스크립트 내부적으로 String 객체가 만들어진다. prop 프로퍼티는 이 객체에 저장되고 이 객체는 곧 제거 된다. 그렇기 때문에 prop라는 속성이 저장된 객체는 존재하지 않게된다. 이러한 특징은 일반적인 객체의 동작 방법과는 다르다. 

하지만 문자열과 관련해서 필요한 기능성을 객체지향적으로 제공해야 하는 필요 또한 있기 때문에 원시 데이터 형을 객체처럼 다룰 수 있도록 하기 위한 객체를 자바스크립트는 제공하고 있는데 그것이 **레퍼객체(wrapper object)**다.

### 객체 데이터 타입(참조 데이터 타입)

래퍼 객체(Wrapper Object):원시데이터를 감싸서 객체로 만들어 준다.
- Number
- String
- Boolean(true/false)


# reference

- 복제(원시데이터인 경우)

![](/assets/img/post/2020-11-29-11-37-57.png)

```javascript
var a = 1;
var b = a;
b = 2;
console.log(a); //1

var a = {'id':1};
var b = a;
b = {'id':2}; //새로운 객체 생성
console.log(a.id); //1
```

- 참조(객체인 경우) : 바로가기 같은 느낌

![](/assets/img/post/2020-11-29-11-38-39.png)

```javascript
var a = {'id':1};
var b = a;
b.id = 2;
console.log(a.id);  // 2
```

```javascript
a = 1;
a = {'id':1};
```

전자는 데이터형이 숫자이고 후자는 객체다. 숫자는 원시 데이터형(기본 데이터형, Primitive Data Types)이다. 자바스크립트에서는 원시 데이터형을 제외한 모든 데이터 타입은 객체이다. 객체를 다른 말로는 참조 데이터 형(참조 자료형)이라고도 부른다. **기본 데이터형은 복제 되지만 객체는 참조된다. 모든 객체는 참조 데이터형이다.**

**정리하면 변수에 담겨있는 데이터가 원시형이면 그 안에는 실제 데이터가 들어있고, 객체면 변수 안에는 데이터에 대한 참조 방법이 들어있다고 할 수 있다.**
출처 : https://opentutorials.org/course/743/6507

# function & reference

- 원시 데이터 타입

```javascript
var a = 1;
function func(b){
    b = 2;
}
func(a);
// var b = a;
// b = 2;

console.log(a);
//1
```

- 참조 (객체) 데이터 타입

```javascript
var a = {'id':1};
function func(b){
    b = {'id':2};
}
func(a);
// b = a;
// b = {'id':2}; //새로운 객체 생성

console.log(a.id);
//1
```
함수 func의 파라미터 b로 전달된 값은 객체 a이다. (b = a) b를 새로운 객체로 대체하는 것은 (b = {'id':2}) b가 가르키는 객체를 변경하는 것이기 때문에 객체 a에 영향을 주지 않는다.

하지만 아래는 다르다.

```javascript
var a = {'id':1};
function func(b){
    b.id = 2;
}
func(a);
// var b = a;
// b.id = 2;

console.log(a.id);  // 2
```
파라미터 b는 객체 a의 레퍼런스다. 이 값의 속성을 바꾸면 그 속성이 소속된 객체를 대상으로 수정작업을 한 것이 되기 때문에 b의 변경은 a에도 영향을 미치게 된다. 