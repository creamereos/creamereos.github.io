---  
layout: post
title: JS Coding Test - 최대 공약수 / 최소공배수
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 

[최대 공약수 / 최소공배수](https://programmers.co.kr/learn/courses/30/lessons/12940)

# coding test
두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요. 배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다. 예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.
- 제한 사항 : 두 수는 1이상 1000000이하의 자연수입니다.

# pseudo code
 
~~~
[최대 공약수]
for n을 i(1~m)으로 나눴을 때 나머지가 0인 i와
m을 i(1~m)으로 나눴을 때 나머지가 0인 i를 배열(max)에 push.
배열(max)의 마지막 부분을 pop으로 꺼낸 후 answer의 [0]번째에 넣어준다.

[최소 공배수]
m과 n을 곱한 것을 answer[0]으로 나눠준 후 answer[1]에 push.
~~~

# Solution

~~~
function solution(n, m) {
    let answer = [];
    let max = [];
    //최대 공약수
    for(let i=1; i<=m; i++){
        if(n % i ===0 && m % i===0){
            max.push(i);
        }
    }
    answer[0]=max.pop();

    //최소 공배수 = m*n/최대 공약수
    answer[1]=m*n/answer[0];
    return answer;
}
~~~

# other Solution

~~~
function greatestCommonDivisor(a, b) {return b ? greatestCommonDivisor(b, a % b) : Math.abs(a);}
function leastCommonMultipleOfTwo(a, b) {return (a * b) / greatestCommonDivisor(a, b);}
function gcdlcm(a, b) {
    return [greatestCommonDivisor(a, b),leastCommonMultipleOfTwo(a, b)];
}
~~~

# Study

### 논리 연산자
논리 연산자는 보통 Boolean값과 함께 쓰이며, Boolean값을 반환

#### OR 연산자
OR연산자는 두개의 역슬래쉬 기호로 표시.인수 중 하나라도 true가 있으면 true를 반환하고 그렇지 않으면 false를 반환한다.

~~~
result = false || true;
console.log(result);	// true
~~~


#### AND 연산자
AND 연산자는 두개의 앤퍼센트 기호로 표시.두 피연산자가 모두 참일 경우에만 true이며 그렇지 않으면 false를 반환한다.

~~~
result = true && true;
console.log(result);	// true
~~~

- AND연산자)는 OR연산자보다 우선순위가 높다.

~~~
a && b || c && d = (a && b) || (c && d)
~~~

#### NOT연산자
NOT연산자는 느낌표로 표시. 피연산자를 boolean 유형으로 변환하여 true/false로 값을 나눈 후 그 결과값의 반대값을 반환한다.

~~~
n1 = !true               // !t returns false
n2 = !false              // !f returns true

!!를 이용하여 이중으로 NOT을 사용할 수 있다.
~~~

### 최대공약수 GCD(Greatest Common Divisor) :

##### 유클리드 호제법
2개의 자연수  a, b에 대해서 a를 b로 나눈 나머지를 r이라 하면 (단 a>b), a와 b의 최대공약수는 b와 r의 최대공약수와 같다. 이 성질에 따라, b를 r로 나눈 나머지 r0를 구하고, 다시 r을 r0로 나눈 나머지를 구하는 과정을 반복하여 나머지가 0이 되었을 때 나누는 수가 a와 b의 최대공약수이다.

~~~
GCD(A, b) = GCD(B,A%B)
if A % B =0 -> GCD = B
else GCD(B,A%B)
~~~

### 최소공배수 LCM(Least Common Multiple) :
최소공배수 = 두 자연수의 곱 / 최대공약수
