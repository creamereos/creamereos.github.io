---  
layout: post
title: Components / Props / State / WithRouter
categories: dev
tags: react
comments: true
---

### Components & Props 
개념적으로 JS 함수와 비슷. props라고 하는 임의의 임력을 받은 후 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환.

props는 속성(Property)을 나타내는 데이터

##### componet 정의 방법

- 함수 컴포넌트 : 위 함수는 데이터를 가진 하나의 “props” (props는 속성을 나타내는 데이터) 객체 인자를 받은 후 React 엘리먼트를 retrun

```js
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>
}
```

- ES6 Class 컴포넌트 정의 : 추가 기능 사용 가능

```js
class Welcome extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```


- State

- WithRouter
