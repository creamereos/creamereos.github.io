---  
layout: post
title: TS x React Style & Theme Component 
categories: dev
tags: ts
comments: true
---

- inline style type

```ts
import React from "react";
import Styled from "styled-component";

// isBlue의 Type이 지정되지 않았으므로 inline type으로 type을 지정해준다.
// *TIP : isBlue의 Type 역시 interface로 별도로 지정 가능하지만 
// style의 경우 inline으로 type을 지정하는게 깔끔하다.
const Container = Styled.span<{ isBlue: boolean }>`
    color: ${props => (props.isBlue ? "blue" : "black" )};
`;

// count의 type을 지정
interface InterProps {
    count: number;
}

const Numbers: React.FunctionComponent = ({ count }) => (
    <Container isBlue={count > 10}> { count } </Container>
);
```

- theme : variable을 지정하여 코드의 반복을 줄일 수 있다. + 자동 완성

theme.tsx

```tsx
export default {
    fakeBlueColor: "red";
};
```

index.tsx

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { ThemeProvider } from "styled-components";
import theme from "./theme";

const plus = (a:number, b:number) => a + b;

ReactDOM.render(
  <ThemeProvider theme = { theme }>
    <App />
  </ThemeProvider>,
  document.getElementById('root')
);
```

Nubmer.tsx

```tsx
import React from "react";
import Styled from "styled-components";

const Container = Styled.span<{isBlue:boolean}>`
    color: ${props => (props.isBlue ? props.theme.BlueColor : "black")};
`;

interface InterProps {
    count: number;
}

const Number: React.FunctionComponent<InterProps> = ({ count }) => (
    <Container isBlue={count > 3}>{count}</Container>
);

export default Number;
```

styled.d.ts : Style Type 자동 완성

```ts
import 'styled-components'

// and extend them!

declare module 'styled-components' {
    export interface DefaultTheme {
        blueColor: string;
    }
}
```