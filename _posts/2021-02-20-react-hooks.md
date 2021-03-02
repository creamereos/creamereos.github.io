---  
layout: post
title: React Hooks - useState
categories: dev
tags: react
comments: true
---

##### react hooks : react의 state 들을 연결하는 기본적인 방법. Class를 사용하지 않는 함수형 프로그래밍. Class, Render, this, state, setState 등을 사용 안해도 됨

- react hook  사용 전

```js
import React, { Component } from "react";

class App extends Component {
  state = {
    count: 0
  };
  modify = (n) => {
    this.setState({
        count: n
    });
  };
  render() {
    const { count } = this.state;
    return (
      <>
        <div>{count}</div>
        <button onClick={() => this.modify(count + 1)}>PLUS</button>
      </>
    );
  }
}

export default App;
```

- react hook - useState 사용 후 

- useState는 value 값과 이를 변경하는 방법을 가진다.
[값, 변경하는 방법] = useState(초기 값)

```js
// react에서 React, useState 모듈 임포트
import React, { useState } from "react";

// 함수형 프로그래밍
// App 함수 정의
const App = () => {
    // count와 setCount는 변수명이므로 변경 가능.
    // [값, 변경하는 방법] = useState(초기 값)
    // useStates는 array[]이다.
  const [count, setCount] = useState(0);
    //const count = useState(0)[0];
    // const setCount = useState(0)[1]; 
  return (
    // rendering 할 react 문법은 {}에 넣어준다.
    <>
      {count} <br />
      <button onClick={() => setCount(count + 1)}>PLUS</button>
      // react hook을 사용하면 기능 추가가 더 쉽다.
      <button onClick={() => setCount(count - 1)}>MINUS</button>
    </>
  );
};

export default App;
```

- useInput : 업데이트 기능

```js
import React, { useState } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

// function 
const useInput = (initialValue, validator) => {
    // useState는 Array 형태를 갖는다. 
    // [값, 변경하는 방법] = useState(초기 값)
  const [value, setValue] = useState(initialValue);
    //  onChange는 event function
  const onChange = (event) => {
    const {
        // value = event
      target: { value }
    } = event;
    let willUpdate = true;
    if (typeof validator === "function") {
      willUpdate = validator(value);
    }
    if (willUpdate) {
      setValue(value);
    }
  };
  return { value, onChange };
};

const App = () => {
  const notE = (value) => !value.includes("@");
  const name = useInput("Mr.", notE);
  return (
    <div className="App">
      {/* {...name}은 value={name.value} onChange={name.onChange}와 같은 뜻 */}
      {/* ...을 사용하면 unpacking */}
      <input placeholder="이름을 입력하세요" {...name} />
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

- useState ex code : useTabs

```js
import React, { useState } from "react";
import ReactDOM from "react-dom";

// object array
const content = [
  {
    tab: "section1",
    content: "Section1 Content"
  },
  {
    tab: "section2",
    content: "Section2 Content"
  }
];

// allTabs는 validator
const useTabs = (initialTab, allTabs) => {
    // allTabs이 비어있거나 allTabs이 array가 아니면 
  if (!allTabs || !Array.isArray(allTabs)) {
    return;
  }
  const [currentIndex, SetCurrentIndex] = useState(initialTab);
  return {
    currentItem: allTabs[currentIndex],
    changeItem: SetCurrentIndex
  };
};

const App = () => {
  const { currentItem, changeItem } = useTabs(0, content);
  return (
    <div className="App">
      {content.map((section, index) => (
        <button onClick={() => changeItem(index)}>{section.tab}</button>
      ))}
      <div>{currentItem.content}</div>
    </div>
  );
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```