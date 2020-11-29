---  
layout: post
title: JS - switch
categories: dev
tags: JS
comments: true
---

switch 문은 if문과 같은 조건문이다.

- 장점 : 분기의 수가 많을 경우 코드가 더 간결
- 단점 : 비교연산 등 구현하기 애매한 상황이 발생 할 수도 있다.

- syntax : 
```javascript
switch (변수명){
    case 조건1 :
        조건1일 때 실행할 코드;
        break;
    case 조건2 :
        조건2일 때 실행할 코드;
        break;
    case 조건3 :
        조건3일 때 실행할 코드;
        break;

    default :
        조건 1,2,3 모두 아닐때 실행할 코드;
        // 경우에 따라 break;을 넣어줘야할때도 있다.
}
```

- ex code : 
```javascript
for(let i=0; i<f.length; i++) {
        const char = f[i];
        switch(char) {
            
            case '+' : case '-' : case '*' : case '/' :
                // 반복 : 스택이 비어있지 않는 경우 AND 
                while(stack[stack.length-1]!=null &&
                    (prec(char) <= prec(stack[stack.length-1]))) {
                    // 현재 연산자의 우선순위가 낮거나 같으면 temp에 pop한 값을 임시 저장
                    temp+=stack.pop();
                    // 만약에 stack에 남아 있는 숫자가 아니면, 즉 연산자인 경우 temp를 convert에 push 해 줌.
                    if(isNaN(stack[stack.length-1])) {
                        convert.push(temp);
                        temp = ""                    
                    }
                }
                stack.push(char);
                break;
            
            // default는 swithch문의 case에 해당하지 않는 경우. 즉, char가 숫자인 경우
            default : 
                // temp = temp + char;
                temp+=char;
                // 만약 f의 i+1 index가 숫자가 아닌 경우 OR i+1이 f의 length인 경우, 즉 마지막 index인 경우.
                if(isNaN(f[i+1]) || (i+1 == f.length) ) {
                    // 후위표기식에 temp를 push한다.
                    convert.push(temp);
                    // temp 초기화
                    temp = "";
                }
                // 위 default 실행을 break;
                break; 
        }
    }
```