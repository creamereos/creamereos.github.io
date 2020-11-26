---  
layout: post
title: 후위표기법
categories: dev
tags: JS
comments: true
---

후위 표기법이란 피연산자(숫자)를 먼저 쓰고 그 뒤에 연산자가 나오는 형태의 식을 말합니다.

- ex) 후위 표기식 예시

```
중위 표기식: 2 + 3 x 4
후위 표기식 : 2 3 4 x +
```

후위표기식은 왼쪽부터 오른쪽으로 순차적으로 읽습니다. 피연산자(숫자)는 지나치고, 연산자(+,-,*,/)가 나오게 되면 연산자 앞의 두개의 숫자(피연산자)로 연산을 진행합니다. 

- ex) 후위 표기식 계산법

```
후위 표기식 : 2 3 4 x +

3 x 4 + 2
```

예를들어 2 3 4 × + 과 같은 후위표기식이 있다고 하면 첫번째 연산자 ×를 만난 시점에서 3×4을 진행하고, 다음 연산자 +에서 2과 3×4결과의 연산을 진행.

### 스택을 이용한 후위표기식 변환 알고리즘 규칙

- 피연산자(숫자)는 바로 출력합니다.
- 괄호 ( 가 나온 경우 스택에 push 해 줍니다.
- 괄호 ) 가 나온 경우, 스택의 Top부터 열린 괄호 ( 가 나오기까지의 노드들을 순서대로 pop하며 출력합니다.
- 연산자(+,-,*,/)가 나오고, 스택이 비어있는 경우 연산자를 바로 스택에 넣어줍니다.
- 만약 스택이 비어있지 않다면, 현재의 연산자와 스택의 요소들의 우선순위를 비교합니다. 현재 연산자가 스택의 top보다 우선순위가 낮거나 같다면 스택을 pop하고 출력해줍니다. 

- 이런식으로 현재 연산자보다 우선순위가 높은 top이 나올때까지 반복합니다. 마지막으로 연산자를 스택에 push 합니다.

예를 들어 2 + 2 * 2 라는 식이 있고 네번째 루프 즉 연산자 *의 루프에 있다고 가정해 봅시다. 스택의 top은 +이므로 *와 우선순위를 비교합니다. 곱셈은 덧셈보다 우선순위가 높으므로 출력하지 않고 그냥 *을 스택에 push 합니다.
만약 스택에 연산자가 남아 있는 경우 마지막에 모두 출력해줍니다.

```javascript
let stack = [];
let convert = [];
let temp = "";

let f = "(5*2)+ (200 + 25)/2";
f = f.replace(/(\s*)/g,"");

function prec(op) {
    switch(op) {
        case '(' :
        case ')' :
            return 0;
        case '+' :
        case '-' :
            return 1;
        case '*' :
        case '/' :
            return 2;
    }
    return 999;
}

for(let i=0; i<f.length; i++) {

    const char = f.charAt(i);

    switch(char) {
        case '(' :
            stack.push(char);
            break;
        case '+' : case '-' : case '*' : case '/' :
            while(stack[stack.length-1]!=null &&
                 (prec(char) <= prec(stack[stack.length-1]))) {
                temp+=stack.pop();

                if(isNaN(stack[stack.length-1])) {
                    convert.push(temp);
                    temp = ""                    
                }
            }
            stack.push(char);
            break;
        case ')' :
            let returned_op = stack.pop();
            while(returned_op != '(') {
                temp+=returned_op;
                returned_op = stack.pop();

                if(isNaN(stack[stack.length-1])) {
                    convert.push(temp);
                    temp = ""                    
                }
            }
            break;
        default : 
            temp+=char;
            if(isNaN(f.charAt(i+1)) || (i+1 == f.length) ) {
                convert.push(temp);
                temp = ""
            }
            break; 
    }
}

while(stack.length != 0) {
    convert.push(stack.pop());
}

let result = "";
for(let i in convert) {
    result+=convert[i];
    result+=" "; 
}
console.log(f);
console.log(result);
```