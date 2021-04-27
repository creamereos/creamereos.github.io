---  
layout: post  
title: recursion & memoization 21.04.19
subtitle: 
categories: dev
tags: js algorithm
comments: true  
--- 

memoization : 프로그래밍(ex : 재귀 호출) 시 반복 되는 결과를 메모리에 저장해서 다음에 같은 결과가 나올 때 이전에 저장한 메모리에서 불러와서 실행하는 방법. (속도가 빠르다.)

### memoization 예제 코드

##### 피보나치 수열

- recursion

```js
let fibo = (number) => {
    // base case
    if (number < 2) {
        return number;
    } 
    // recursive case
    return fibo(number-2) + fibo(number-1)
}
// 위 코드를 삼항 연산자로 한줄로 작성할 수도 있다.
let fibo = (n) => {
    return n < 2 ? n : fibo(n-2) + fibo(n-1)
}

fibo(10) // 55
```

일반적인 재귀 함수로 fibonacci 수열을 구현하면 중복되는 계산 때문에 시간이 오래걸린다.

시간 단축을 위해 memoization이 필요하다.

- recursion + memoization

```js
// 계산 값을 저장하기 위해 memo 배열 사용.
// 초기 값은 피보나치 수열의 0번째와 1번째 값을 저장.
let memo = [0, 1]

let fiboMemo = (n) => {
    // 만약 memo의 n번째 인덱스의 type이 숫자가 아니라면
    // (즉, n번째 피보나치 수열이 메모에 저장되어 있지 않다면)
    if (typeof memo[n] !== 'number') {
        // memo[n]의 값을 재귀를 통해 저장해준다.
        memo[n] = fiboMemo(n-1) + fiboMemo(n-2);
    }
    return memo[n]
}
```

### 성능 상세 비교 recursion vs recursion + memoization

- recursion

```js
let count = 0;
let fibo = (number) => {
    count++;
    console.log('재귀 횟수:', count);
    // base case
    if (number < 2) {
        return number;
    } 
    // recursive case
    return fibo(number-2) + fibo(number-1)
}

fibo(20); // 재귀 횟수 : 301번
```

- recursion + memoization

```js
let memo = [0, 1];
let count = 0;
let fiboMemo = (n) => {
    if(typeof memo[n] !== 'number') {
        count++;
        console.log('재귀 횟수: ', count)
        memo[n] = fiboMemo(n-2) + fiboMemo(n-1);
    }
    return memo[n]
}

fiboMemo(20); // 재귀 횟수:  19
```

- recursion + memoization + O(N) 알고리즘 

```js
let fibo = (n) => {
    let memo = [0, 1];
    let fiboMemo = (n) => {
        if (memo[n] !== undefined) {
            return memo[n];
        }
        memo[n] = fiboMemo(n-2) + fiboMemo(n-1);
    }
    return memo[n];
}
```
