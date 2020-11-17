---  
layout: post
title: JS - if, while, for, break
subtitle:
categories: dev
tags: javascript
comments: true
---

### if, else if, else
 if 문의 조건이 true면 중괄호의 시작({}부터 중괄호의 끝(})까지의 구간이 실행. false이면 중괄호 구간이 실행되지 않음.

```javascript
        let id = prompt('아이디를 입력해주세요.');
        let password = prompt ('비밀번호를 입력해주세요.');
        if((id==='creamer' ||id==='terry') && password==='12345'){  alert('로그인 성공');
        } else if (password!=='12345'){
            alert('비밀번호가 일치하지 않습니다.');
        } else {
            alert('아이디가 일치하지 않습니다.');
        }
```

- && : AND 연산자
- || : OR 연산자

### loop & iterate


##### while

```javascript
while(조건){
    반복해서 실행할 코드;
}
```
조건이 false가 될 때 반복 종료.

```javascript
let i = 0;
// 종료조건으로 i의 값이 10보다 작다면 true, 같거나 크다면 false가 된다. 즉 9까지 반복한다.
while(i < 10){
    // 반복이 실행될 때마다 i의 값이 1씩 증가한다.
    i++;
}
```

##### for 

```javascript
for(초기화; 반복조건; 반복이 될 때마다 실행되는 코드){
    반복해서 실행될 코드
}
```


```javascript
for(let i=0; i<10; i++){
    i+1;
}
```

##### break
반복문을 종료하고 반복문 밖으로.

```javascript
for(초기화; 반복조건; 반복이 될 때마다 실행되는 코드){
    if(조건){
        break;
    }
    반복해서 실행될 코드
}
```
if의 조건이 true면 break로 for 반복문을 빠져나옴.

```javascript
for(let i = 0; i < 10; i++){
    if(i === 5) {
        break;
    }
    document.write('coding everybody'+i+'<br />');
}

//coding everybody 0
// coding everybody 1
// coding everybody 2
// coding everybody 3
// coding everybody 4
```

##### continue

건너뛰기(순간 종료, 반복문은 유지)

```javascript
for(초기화; 반복조건; 반복이 될 때마다 실행되는 코드){
    if(조건){
        continue;
    }
    반복해서 실행될 코드
}
```
if의 조건이 true면 continue로 해당 반복을 건너 뛰고 for 반복문을 그 이후 부터 진행.

```javascript
for(let i = 0; i < 10; i++){
    if(i === 5) {
        continue;
    }
    document.write('coding everybody'+i+'<br />');
}

// coding everybody 0
// coding everybody 1
// coding everybody 2
// coding everybody 3
// coding everybody 4
// coding everybody 6
// coding everybody 7
// coding everybody 8
// coding everybody 9
```

##### 중첩 for

```javascript
for(초기화; 반복조건; 반복이 될 때마다 실행되는 코드){
    for(초기화; 반복조건; 반복이 될 때마다 실행되는 코드){
         반복해서 실행될 코드
    }
}
```

```javascript
for(let i=0; i<10; i++){
    for(let j=0; j<10; j++){
        document.write(String(i)+String(j)+'<br />');
    }
}
```
i가 1번 실행되고 j가 10번 실행 * 10(i가 10번)