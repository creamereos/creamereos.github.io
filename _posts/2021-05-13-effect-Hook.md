--- 
layout: post  
title: React Hook - useEffect 21.05.13
subtitle: 
categories: dev
tags: react
comments: true  
--- 

useEffect : component 렌더링 이후 useEffect 내부 코드 실행 + 함수 컴포넌트에서 *side effect를 수행 가능하다.

*Side Effect : 함수(또는 컴포넌트)의 입력 외에도 함수의 결과에 영향을 미치는 요인. 

class를 사용하여 side-effect를 처리하려면 life cycle method(componentDidMount, componentDidUpdate, componentWillUnmount)를 사용해야하지만 Hook의 useEffect를 사용한다면 더 코드를 간결하고 직관적으로 구현할 수 있다.

- syntax

```js

useEffect(함수 실행 코드)

```

### [리액트 컴포넌트 side effect 종류]

리액트가 DOM을 업데이트(렌더링)한 뒤 추가로 코드를 실행해야 하는 경우가 있다. 

리액트 컴포넌트에는 일반적으로 clean-up이 필요한 side effect와 필요하지 않은 side effect로 구분할 수 있다.

###### clean-up을 이용하지 않는 side effect

네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 clean-up(정리)이 필요 없는 side effect들이다.

- class component를 이용한 side effect 처리

```js
class Ex extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }

    // 컴포넌트 처음 실행 직후 실행
    componentDidMount() {
        // 중복 코드 발생
        document.title = `You clicked ${this.state.count} times`;
    }
    // state 업데이트 직후 실행
    componentDidUpdate() {
        // 중복 코드 발생
        document.title = `You clicked ${this.state.count} times`;
    }

    render() {
        return (
            <div>
                <p>You clicked {this.state.count} times</p>
                <button onClick={() => this.setState({ count: this.state.count + 1})}>Click Me!</button>
            </div>
        )
    }
}
```

- Hook useEffect를 활용하여 side-effect 처리

```js
import React, { useState, useEffect } from 'react';

function Ex() {
    const [count, setCount] = useEffect(0);

    // 기본적으로 첫번째 렌더링과 이후의 모든 업데이트에서 수행
    // 즉, 첫 렌더링 직후 실행 + state가 갱신 될 때 마다 실행
    useEffect(() => {
        // 이 부분이 (side) effect
        // component 내부에 있기 때문에 state에 바로 접근 가능
        document.title = `You clicked ${count} times`
    });

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick = {() => setCount(count + 1)})> Click Me!!! </button>
        </div>
    );
}
```

###### clean-up을 이용하는 side effect

clean-up(정리)이 필요한 side effect도 있다. (ex : 구독 설정)
clean-up을 하는 이유는 메모리 누수가 발생하지 않도록 하기 위함이다.

- class compnent를 이용하여 clean-up이 필요한 side effect 처리 : componentWillUnmount

```js
class FriendStatus extends React.Component {
    constructror(props) {
        super(props);
        // 현재 접속 여부를 구분하는 state
        this.state = { isOnline: null }

        // bind 사용 이유 : class기 때문에 this가 가리키는 인스턴스를 고정(묶어주기)
        // 호출되는 메소드(handleStatusChange)가 다른 곳에서 호출이 되기 때문에 this를 고정시켜줘야한다.
        // class 내에서는 bind this를 해주지 않으면 this가 undefined
        this.handleStatusChange = this.handleStatusChange.bind(this);
    }

    // component 첫 렌더링 직후 실행
    componentDidMount() {
        ChatAPI.subscribeToFriendStatus(
            this.props.friend.id,
            this.handleStatusChange
        );
    }

    // component 해제 시 실행
    // 컴포넌트가 소멸된 시점에(DOM에서 삭제된 후 === 렌더링에서 삭제 된 후) 실행 
    // 컴포넌트 내부에서 타이머나 비동기 API를 사용하고 있을 때, 이를 제거하기에 유용
    componentWillUnmount() {
        ChatAPI.unsubscribeFromFriendStatus(
            this.props.friend.id,
            this.handleStatusChange
        );
    }

    handleStatusChange(status) {
        this.setState({
            isOnline: status.isOnline
        })
    }

    render() {
        if (this.state.isOnline === null) {
            return 'Loading...';
        }
        return this.state.isOnline ? 'Online' : 'Offline';
    }
}
```

- Hooks useEffect를 사용하여 clean-up이 필요한 side effect 처리 : useEffect return

```js
class FriendStatus extends React.Component {
    constructror(props) {
        super(props);
        // 현재 접속 여부를 구분하는 state
        this.state = { isOnline: null }

        // bind 사용 이유 : class기 때문에 this가 가리키는 인스턴스를 고정(묶어주기)
        // 호출되는 메소드(handleStatusChange)가 다른 곳에서 호출이 되기 때문에 this를 고정시켜줘야한다.
        // class 내에서는 bind this를 해주지 않으면 this가 undefined
        this.handleStatusChange = this.handleStatusChange.bind(this);
    }

import React, { useState, useEffect } from 'react';

function FriendStatus() {
    const [isOnline, setIsOnline] = useState(null);

    componentDidMount() {
        ChatAPI.subscribeToFriendStatus(
            this.props.friend.id,
            this.handleStatusChange
        );
    }

    componentWillUnmount() {
        ChatAPI.unsubscribeFromFriendStatus(
            this.props.friend.id,
            this.handleStatusChange
        );
    }

    useEffect(ChatAPI.subscribeToFriendStatus(friend.id))
}

```