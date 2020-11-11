---  
layout: post  
title: JavaScript Coding Test-나누어 떨어지는 숫자 배열
subtitle: 
categories: dev
tags: dev codingtest sort filter map
comments: true  
--- 
array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.
divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

**[제한사항]**
- arr은 자연수를 담은 배열입니다.
- 정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.
- divisor는 자연수입니다.
- array는 길이 1 이상인 배열입니다.

**[입출력 예 설명]**
- 입출력 예#1
arr의 원소 중 5로 나누어 떨어지는 원소는 5와 10입니다. 따라서 [5, 10]을 리턴합니다.

- 입출력 예#2
arr의 모든 원소는 1으로 나누어 떨어집니다. 원소를 오름차순으로 정렬해 [1, 2, 3, 36]을 리턴합니다.

- 입출력 예#3
3, 2, 6은 10으로 나누어 떨어지지 않습니다. 나누어 떨어지는 원소가 없으므로 [-1]을 리턴합니다.

# Pseudo Code

```javascript
- 배열에서 나머지가 0인 요소 구해서 넣어주기
for 배열 arr의 i번째 요소를 변수 divisor로 나누어 나머지(%)가 if 나머지가 0이라면 배열 answer에 i번째 요소를 psuh.

- 오름차순 정렬
answer 배열을 sort 해준다.
주의! 단순한 sort 함수로는 유니코드 순서로 정렬되니 다른 sort 함수를 사용해주어야한다. (ex. 단순 sort 정렬시 10보다 2가 뒤에 나온다.)

- 나머지가 0인 요소가 없는 경우
위 for & if 문을 통과하지 못한 요소는 배열 answer에 없기 때문에 answer의 length가 0인 경우 answer은 [-1]
```

# Answer

```javascript
function solution(arr, divisor){
    let answer = [];

    for(let i=0; i<arr.length; i++){
        if(arr[i]%divisor===0) answer.push(arr[i]);
    }
    answer.sort((a,b)=>a-b);

    if(answer.length===0) answer=[-1];

    return answer
}
```

# Other Answers

```javascript
function solution(arr, divisor) {
    var answer = arr.filter(v => v%divisor == 0);
    return answer.length == 0 ? [-1] : answer.sort((a,b) => a-b);
}
```

```javascript
function solution(arr, divisor) {
    var answer = [];
    arr.map((o) => {
        o % divisor === 0 && answer.push(o);
    })
    return answer.length ? answer.sort((a, b) => a - b) : [-1];
}
```

# Study

### sort()

```javascript
arr.sort([compareFunction])
```
**[compareFunction]**
- 이 부분이 생략되면 배열의 element들은 string으로 취급되어 유니코드 순서대로 정렬된다.

```javascript
const arr = [2, 1, 3, 10, 200];

arr.sort(); // [1, 10, 2, 200, 3]
```

- 이 함수는 두 개의 배열 element를 파라미터로 입력. 예를들어 a, b 두개의 element를 파라미터로 입력받을 경우,

이 함수가 리턴하는 값이 0보다 작을 경우,  a가 b보다 앞에 오도록 정렬하고, 만약 0을 리턴하면, a와 b의 순서를 변경하지 않는다.

```javascript
const arr = [2, 1, 3, 10, 200];

arr.sort(function(a, b){
  if(a > b) return 1;
  if(a === b) return 0;
  if(a < b) return -1;
}); 

// [1, 2, 3, 10, 200]
```

위 코드를 다음과 같이 단순화 할 수 있다.

```javascript
arr.sort(function(a, b){return a - b;});
// [1, 2, 3, 10, 200]

- 화살표 함수 사용
arr.sort((a,b)=>a-b);
// [1, 2, 3, 10, 200]
```

- 오름차 순 정렬(Number)

```javascript
arr.sort(function(a, b){return a - b;});
// [1, 2, 3, 10, 200]
```

- 내림차 순 정렬(Number)

```javascript
arr.sort(function(a, b){return b - a;});
// [200, 10, 3, 2, 1]
```