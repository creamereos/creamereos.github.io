--- 
layout: post  
title: Redux Code Reivew - 21.05.18
subtitle: 
categories: dev
tags: redux
comments: true  
--- 

### react-redux Provider 

Provider : react-redux 라이브러리에 내장되어있는, 리액트 앱에 store 를 손쉽게 연동 할 수 있도록 도와주는 컴포넌트. 이 컴포넌트를 불러온 다음에, 연동 할 컴포넌트를 감싼 후 Provider 컴포넌트의 props로 store 값을 설정.

- syntax

```js
<Provider store 프롭스>
    <store를 연동할 컴포넌트 />
</Provider>
```

- ex code

```js
// react package import
import React from 'react';
// react dom package impot
import ReactDOM from 'react-dom';
// App component import
import App from './App';
// store 파일 import.
import store from './store/store';
// react-redux package의 provider import
import { Provider } from 'react-redux';

// ReactDom.render(render할 컴포넌트, render할 위치)
ReactDOM.render(
  // react에 store 연동 : Provier의 props로 store를 설정하고
  // 연동할 Component(이 코드에서는 App)를 감싸준다.
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

### SPA(Single Page Application) x CSR(client side routing)

https://velopert.com/3417

- SPA(Single Page Application) : 페이지가 1개인 어플리케이션. 

- CSR(Client Side Routing) : 렌더링 라우팅을 서버 사이드가 아닌 클라이언트 사이드에서 담당

- SPA x CSR(Client Side Routing) : 렌더링을 서버 사이드에서 담당하면 렌더링을 위한 서버 자원이 사용되고, 불필요한 트래픽도 낭비된다. 그러므로 CSR을 사용하여 이 문제를 해결한다. 앱의 규모가 커지면 JS 파일 사이즈가 너무 커져서 로딩 속도가 느려지는 단점이 있지만 Code Splitting(라우트를 파일 별로 나누기)을 사용하여 트래픽과 로딩 속도를 개선 가능하다.

##### react-router-dom

CSR을 위한 써드파티 라이브러리. react-router-dom에서 다음 3가지 기능을 활용하여 CSR을 구현한다.

- BrowserRouter : 전체 컴포넌트를 감싸준다.

- Switch : 분기 처리와 관련된 모든 컴포넌트를 Switch로 감싸준다.

- Route : 분기 처리 할 각각의 컴포넌트를 Route로 감싸준다. Props로 path를 가지며 path를 정확하게 지정해주기 위해 exact prop를 사용한다.

**주의 사항** : Route의 props의 path가 / 인경우 반드시 exact를 props에 작성한다.

- Link : Path로 이동하는 버튼 (별도 컴포넌트에 작성)

### react-router-dom : ex Code

- App.js

```js
// React import
import React from 'react';
// react-router-dom에서 routing에 필요한 모듈 import
// client side routing을 위해 필요
import {
  // A as B, A라는 변수 대신 B라는 변수로 사용하겠다.
  BrowserRouter as Router,
  Switch,
  Route,
} from "react-router-dom";

function App() {
    return (
        // Router(===BrowerRouter)로 전체 컴포넌트를 감싸준다.
        <Router>
            // 분기처리하지 않는 컴포넌트라(분기 처리 해도 공통으로 들어가는 컴포넌트)도 BrowserRouter로 감싸준다.
            <컴포넌트1 />
            // Swtich로 routing(분기 처리)할 component를 감싸준다.
            <Switch>
                // 분기 처리 할 각 컴포넌트를 Route로 감싸준다.
                // Route는 props로 path를 가진다.
                // exact={true}를 작성하지 않는다면 
                // '/'로 시작하는 모든 주소가 컴포넌트2를 보여준다.
                <Route exact={true} path='/'>
                    <컴포넌트2 />
                </Route>
                // 분기 처리 할 각 컴포넌트를 Router로 감싸준다.
                // '/about'으로 접속하면 컴포넌트 3를 보여준다.
                <Route path='/component3'>
                    <컴포넌트3 />
                </Route>
            </Switch>
            <컴포넌트4 />
        </Router>
    );
}
```

- 컴포넌트 1

```js
import React from 'react';
import { Link } from 'react-router-dom';

function 컴포넌트1() {
    return (
        <div id="menu">
            // 버튼2 클릭 시 '/'를 가진 path로 이동
            // 즉, 컴포넌트2 렌더링
            <Link to="/">버튼2</Link>
            // 버튼3 클릭 시 '/component3'를 가진 path로 이동
            // 즉, 컴포넌트3 렌더링
            <Link to="/component3">버튼3</Link>
        </div>
    )
}
```

### Redux x React Hook : useSelector 

- Redux Store의 데이터 추출하여 컴포넌트와 state를 연결. 

- 컴포넌트에서 useSelector 메소드를 통해 store의 state에 접근

- 사용법

```js
// useSlector로 store의 state에 접근 한 후 
// state에 함수를 실행한 return 값을 변수에 할당.
const 변수 = useSelector(함수);
```

- 예제 코드

```js
// 컴포넌트.js

const count = useSelector(state => state.counterReducer.count);

// 여러 reducer의 값 한번에 가져오기
const {counter, person }  = useSelector(state => ({
     count : state.counterReducer.count,
     person: state.personReducer.person,
}));
```

- 액션 생성자 함수 : 

