---  
layout: post
title: TS x React Interface state & property Ex code
categories: dev
tags: ts
comments: true
---

### Interface

- Ex Code : state - class component

```ts
import React, { Component } from "react";
// "react" module에서 React와 property인 { Component }를 import 
import { createGlobalStyle } from "styled-components";
// "styled-components" module에서 property인 { createGlobalStyle } import

interface interState {
  counter: number;
}
// state의 type이 정의 되지 않았으므로 interface를 이용해서 정의

// Component <> Syntax
// class Component<P = {}, S = {}, SS = any>
// P는 property object. S는 state object.
class App extends Component<{}, interState> {
// state의 type을 정의한 interface를 Component의 <> 사이에 넣어준다.
  state = {
//default state(상태)의 object를 정의해준다.  
    counter: 0 
  };

// render : html 요소(element), 또는 React 요소 등의 코드가 눈으로 볼 수 있도록 그려지는 것
  render() {
    //   render() 실행. 

    // this는 class App을 가리킨다. 
    // const는 block scope가 있으므로 사용 할 부분의 {} 안에 작성해준다.
    const { counter } = this.state;
    // 위 코드는 const this.state.counter = 0 와 같다.
    // 즉, class app의 state의 counter가 variable이고 그 value는 0이다.

    // render 할 return element(HTML, React, etc..)를 작성
    return (
      <div>
        // 위에서 정의한 변수 { counter } 표시. 즉, 초기 값은 0
        {counter}
        // button click 시 (this가 가리키는) class app 안에 있는 add function을 실행 시킨다.
        <button onClick={this.add}>
          Add
        </button>
      </div>
    );
  }
//  add function 설정
  add = () => {
    //  (this가 가리키는) class app 안에 있는 state의 값을 setState로 변경 
    // prev라는 arguments를 정의해주고
    this.setState(prev => {
    // return 값으로 현재 상태(prev)의 counter에 1을 더해준다.
      return {
        counter: prev.counter + 1
      };
    })
  }
}

// App Class를 다른 파일에서 사용할 수 있도록 export한다.
export default App;
```

위와 같은 코드를 분리해서 만들기

- Ex Code : props (property) - function component

Number.tsx

```ts
import React from "react";
import Styled from "styled-components"
// react, styled-components 각 각의 module을 import하여 각각 React와 Styled라는 variable에 저장

const Container = Styled.span``;
// Styled components를 이용하여 <span>을 생성을 variable인 Container에 담아준다.
// ``(backtick) 사이에서 CSS를 사용 가능

// interface를 이용하여 property의 type을 정의해준다. 
interface interProps {
    count: number;
}

// function의 type은 React.FunctionComponent
// FunctionComponent에 property interface 적용 방법

// function인 Number의 type을 정의해주기 위해 FunctionComponet를 사용하고, <> 사이에 interface를 담은 variabel인 InterProps 작성
const Number:FunctionComponent<interProps> => ({ count }) => <Container> {count} </Container>
// Number function을 사용하면 count의 값이 <sapn>에 담겨 return된다.

export default Number;
```

App.tsx

```ts
import React from "react";
import Number from "./Number";
// 현재 폴더에 있는 Number.tsx 파일을 import하여 Number에 담아준다.

interface interState {
    counter: number
}

class App extends Component<{}, interState> {
    state = {
        counter: 0
    }
    render() {
        const {counter} = this.state;
        return (
            <div>
                <Number count={counter} />
                // Number(= <span>{count}</span> 를 가지고와서 count 값에 {counter}를 넣어준다.
                <button onClick={this.add}>Add<button>
            </div>
        )
    }
    add = () => {
        this.setState(prev => {
            return {
                counter: prev.counter + 1
            };
        }
      )
    }
}

export deafult App;
```