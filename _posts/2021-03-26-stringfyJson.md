---  
layout: post  
title: JSON.stringify() 21.03.25
subtitle: 재귀
categories: dev
tags: js
comments: true  
--- 

- **JSON(JavaScript Object Notation)** : JSON은 서로 다른 프로그램 사이에서 데이터를 교환하기 위한 포맷. (자바스크립트 외 다양한 언어에서도 사용)


### JSON.stringify - seriealize (직렬화) :  객체를 문자열로 변환

```js
const message = {
  sender: "김코딩",
  receiver: "박해커",
  message: "해커야 오늘 저녁 같이 먹을래?",
  createdAt: "2021-01-12 10:10:10"
}

let transferableMessage = JSON.stringify(message)
console.log(transferableMessage)
// `{"sender":"김코딩","receiver":"박해커","message":"해커야 오늘 저녁 같이 먹을래?","createdAt":"2021-01-12 10:10:10"}`
```

### JSON.parse : 문자열을 객체로 변환
 -deserialize (역 직렬화)

```js
let packet = `{"sender":"김코딩","receiver":"박해커","message":"해커야 오늘 저녁 같이 먹을래?","createdAt":"2021-01-12 10:10:10"}`

let obj = JSON.parse(packet)
console.log(obj)
/*
{
  sender: "김코딩",
  receiver: "박해커",
  message: "해커야 오늘 저녁 같이 먹을래?",
  createdAt: "2021-01-12 10:10:10"
}
*/
```


# JSON 규칙

- 문자열은 반드시 쌍따옴표로 감싼다. JSON에선 작은따옴표나 백틱을 사용할 수 없다.

```js
JSON.stringify('json');               
// '"json"'
```

- 객체 프로퍼티 이름은 반드시 쌍따옴표로 감싸야한다.
- 키와 값 사이, 그리고 키-값 쌍 사이에는 공백이 있어서는 안됩니다.


```js
JSON.stringify({ x: 5 }, { y: 1});            
// '{"x":5}{"y":1}'
```

- JSON.stringify는 객체뿐만 아니라 원시값(문자,숫자,불리언,null 등)에도 적용 가능


