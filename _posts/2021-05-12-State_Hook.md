--- 
layout: post  
title: React Hook - useState 21.05.12
subtitle: 
categories: dev
tags: react
comments: true  
--- 


# Hook과 함수 컴포넌트

- 함수 표현식 컴포넌트

```js
const Ex = (props) => {
    // 여기서 Hook (useState 등) 사용 가능
    return <div />;
}
```

- 함수 선언식 컴포넌트

```js
function Ex(props) => {
    // 여기서 Hook (useState 등) 사용 가능
    return <div />;
}
```

# useState

일반적으로 함수 컴포넌트는 state 등 리액트의 기능을 사용하지 못하지만 Hook을 사용하면 함수 컴포넌트에서 state 기능을 활용할 수 있다.

하나의 컴포넌트에 여러 개의 useState를 사용할 수 있지만, 객체와 배열을 초기값으로 할당함으로써 데이터를 묶어서 사용 할 수도 있다. 

- syntax 

```js
// import
import react, { useState } from 'react';

// useState 
const [state 변수, state 갱신 함수] = useState('state 변수에 넣어줄 초기값')
// 변수 = 초기값

// state 갱신 함수
function 함수() {
    state 갱신 함수(변경할 값)
}

// 함수 실행
함수()
// 변수 = 변경할 값

// 클래스 컴포넌트의 this.setState와 달리 state를 갱신하는 것은 병합하는 것이 아니라 대체
```

- ex code

```js
// Hook useState import
import react, { useState } from 'react';

function Ex() {
    // 배열 구조 분해 할당.
    // [state 변수, state를 갱신 할 수 있는 함수] = useState(첫번째 인자의 초기값)
    const [count, setCount] = useState(0);
    // count = 0
    // 다수의 useState 사용 가능
    const [hanmburger, setHanmburger] = useState('햄버거');

    return (
        <div>
            <div> {hamburger} 최대 몇개까지 ? </div>
            <p> {count} </p>
            // setCount로 state 갱신. 갱신 될 때마다 컴포넌트를 렌더링.
            <button onClick={() => setCount(count+1)}> 더 먹기 </button>
        </div>
    )
}
```