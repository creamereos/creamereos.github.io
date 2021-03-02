---  
layout: post
title: React Hooks - useEffect
categories: dev
tags: react
comments: true
---

- useEffect : componentWillUnmount, componentDidMount, componentWillUpdate와 기능이 비슷

```js
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";

const App = () => {
  const sayHello = () => console.log("Hello");
  const [number, setNumber] = useState(0);
  const [number2, setNumber2] = useState(0);
  // useEffect는 componentDidMount, componentWillUpdate와 같은 역할을 한다.
  // useEffect는 2개의 argument를 갖는다.
  // 1번째는 function으로써의 effect 기능
  // 2번째는 defendency(독립).
  // defendency에 해당 될 때에만 1번째 argument에 있는 function 실행
  // defendency는 array에 넣어준다. []빈 array일 경우 function은 실행되지 않는다.
  useEffect(sayHello, [number2]);
  return (
    <div className="App">
      <button onClick={() => setNumber(number + 1)}>{number}</button>
      <button onClick={() => setNumber2(number2 + 1)}>{number2}</button>
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

- useEffect ex code 1 : useTitle

```js

```