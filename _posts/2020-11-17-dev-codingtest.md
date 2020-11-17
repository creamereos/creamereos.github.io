---  
layout: post  
title: K번째 수
subtitle: 
categories: dev
tags: codingtest js javascript
comments: true  
--- 

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.
배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

**[제한사항]**
- array의 길이는 1 이상 100 이하입니다.
- array의 각 원소는 1 이상 100 이하입니다.
- commands의 길이는 1 이상 50 이하입니다.
- commands의 각 원소는 길이가 3입니다.

**[입출력 예 설명]**
- [1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.
- [1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.
- [1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

# Pseudo Code

```javascript
1. array를 slice(c[i][0]-1, c[i][1])
2. 오름차 순으로 sort
3. answer에 [command[i][2]]-1 인덱스를 push.
4. 위 실행문을 for문 반복
5. answer return
```

# Answer

```javascript
function solution(a, c) {
    let answer = [];
    for(let i=0; i<c.length; i++){
        answer.push(a.slice(c[i][0]-1, c[i][1]).sort((a,b) => a-b)[c[i][2]-1]);
    }
    return answer;
}
```

# Other Answers

```javascript
function solution(array, commands) {
    return commands.map(v => {
        return array.slice(v[0] - 1, v[1]).sort((a, b) => a - b).slice(v[2] - 1, v[2])[0];
    });
}
```
- .map이 뭔지 공부하기.


# Study

### slice vs splice

- slice : 원본 배열에 영향을 주지 않는다.

```javascript
- syntax
array.slice[a,b] // a 인덱스 이상 b 인덱스 미만
```

```javascript
a = [0, 1, 2, 3, 4, 5]
a.slice[0,3] // [0,1,2]

a // [0, 1, 2, 3, 4, 5]
```

- splice : 원본 배열에 영향을 준다.

```javascript
a = [0, 1, 2, 3, 4, 5]
a.splice[0,3] // [0,1,2]

a // [3, 4, 5]
```

- **sort** : 
오름차 순 정렬시
 

```javascript
sort(); 보단 sort((a,b) => a - b)를 사용하자.

()소괄호 부분이 생략되면 배열의 element들은 string으로 취급되어 유니코드 순서대로 정렬된다.
const arr = [2, 1, 3, 10, 200];
arr.sort(); // [1, 10, 2, 200, 3]
```

- **array 내부의 array**


```javascript
array = [[2, 5, 3], [4, 4, 1], [1, 7, 3]]

array.length = 3 (배열 모음의 갯수)
array[1][2] = 4 (다중 인덱스로 선택 가능)
```
