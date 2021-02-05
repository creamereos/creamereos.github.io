---  
layout: post
title: TS x React Setting 
categories: dev
tags: ts
comments: true
---

- TS가 세팅된 React 설치

```
$ npx create-react-app 폴더명 --template typescript
```

- tsconfig.js 에서 TS의 설정 가능

- TS에서 styled-components 설치(React에서 CSS 사용) 및 허용 방법

```
$ yarn add styled-components

$ yarn add @types/styled-components
```

- App.tsx 파일 내용 변경 (class, component, lender() 추가)

```ts
class App extends Component {
  render() {
    return (
      <div>

      </div>
    );
  }
}
export default App;
```
- 사용하는 library가 @types에 없을 때 문제 해결 방법 (유명한 library를 사용하면 발생할 일이 거의 없음)

tsconfig.js
```js
"noImplicitAny": true,
// 코드에 type이 없는 것들이 있을 수 있는데 이건 types를 못찾아서 그런거다.
```

