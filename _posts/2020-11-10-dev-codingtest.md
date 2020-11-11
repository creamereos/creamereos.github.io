---  
layout: post
title: JS Coding Test - 완주하지 못한 선수
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

[제한 사항]
-마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

# pseudo code

~~~javascript 
participant 배열과 completion 배열을 각 각 sort.
for if 배열의 i번째가 같지 않으면 i번째가 completion 배열에 없는 것이니 participant[i]를 리턴
~~~

# Solution

~~~javascript
function solution(participant, completion) {
    participant.sort(); 
    completion.sort(); 
    for(let i=0;i<participant.length;i++){
        if(participant[i] !== completion[i]){
            return participant[i];
        }
    }
}
~~~

# Other Solutions

```javascript
var solution=(_,$)=>_.find(_=>!$[_]--,$.map(_=>$[_]=($[_]|0)+1))
```


```javascript
const solution = (p, c) => {
    p.sort()
    c.sort()
    while (p.length) {
        let pp = p.pop()
        if (pp !== c.pop()) return pp
    }
}
```
