---  
layout: post
title: JS Coding Test - 모의고사
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 
수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

[제한 조건]
- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

# pseudo code

~~~
[각 수포자의 규칙을 배열로]
1,2,3번 수포자 정답의 규칙적인 배열 math1,2,3을 만들어준다.

[각 수포자의 정답 수 체크]
각 수포자의 정답 수를 체크하기 위해 배열 count를 만들어준다.
for 반복문을 돌며 math 1,2,3을 answers 배열과 대조하여 index가 같다면 count 배열의 각 수를 1씩 올려준다.

[가장 많이 맞춘 사람 찾기]
반복문을 돌며 Math.max 함수를 이용하여 count 함수 중 가장 큰 수를 가진 index를 찾아준다.

index는 0부터 시작하므로 1을 더해서 정답을 return 해준다.
~~~

# Solution

~~~
function solution(answers) {
    let answer = [];
    const math1 = [1, 2, 3, 4, 5];
    const math2 = [2, 1, 2, 3, 2, 4, 2, 5];
    const math3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];
    const count = [0, 0, 0]

    for(let i=0; i<answers.length; i++){
        if(math1[i%5] == answers[i]) count[0]++;
        if(math2[i%8] == answers[i]) count[1]++;
        if(math3[i%10] == answers[i]) count[2]++;
    }

    const max = Math.max(count[0], count[1], count[2]);

    for(let i=0; i<count.length; i++){
        if(max == count[i]) answer.push(i+1);
    }

    return answer;
}
~~~


# Study

### Math.max
입력값으로 받은 0개 이상의 숫자 중 가장 큰 숫자를 반환합니다. 만약 아무 요소도 주어지지 않았다면 -Infinity로 반환합니다. 만약 한 개 이상의 요소가 숫자로 변환되지 않는다면 NaN로 반환됩니다.

~~~
Math.max([값1[, 값2[, ...]]])

Math.max(10, 20);   //  20
Math.max(-10, -20); // -10
Math.max(-10, 20);  //  20
~~~

#### 나머지 연산자
a = 작은 수
b = 큰 수

~~~
a % b = a;
~~~
