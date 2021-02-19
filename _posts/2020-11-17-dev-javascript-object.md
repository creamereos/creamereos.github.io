---  
layout: post
title: JS - object
subtitle:
categories: dev
tags: js
comments: true
---
만약 인덱스로 문자를 사용하고 싶다면 객체 dictionary를 사용해야 한다. (배열은 인덱스를 숫자로 사용) value를 담는 그릇.

### object 생성 및 사용 방법

- object 생성 방법 1

```javascript
let 변수 = {key1:value1,key2:value2}
```

- object 생성 방법 2

```javascript
let 변수 = {key1:value1,key2:value2}

변수[key1]= value1;
변수[key2]= value2;
```

- object 생성 방법 3. new Object

```javascript
let 변수명 = new Object();

변수[key1]= value1;
변수[key2]= value2;
```

- object의 value 가져오기

```javascript
let 변수명 = new Object();

변수[key1; // value1
변수.key2; // value2;
변수['key'+'1']; // value1
```

### for in : object, array의 for 반복 문 

- for in
```javascript
- key 출력
for(let 변수 in 객체or배열){
    console.log(변수);
}

-value 출력
for(let 변수 in 객체or배열){
    console.log(객체or배열[변수]);
}
```

- for in ex)
```javascript
let object = {'terry':90, 'dongdong':93, 'jin':99}

-key 출력
for(key in object){
    console.log(key);
}

// terry, dongdong, jin


-value 출력
for(key in object){
    console.log(object[key]);
}

//90, 93, 99
```

### object에 담길 수 있는 value

- syntax

```javascript
let 변수0 = {
    변수1 : {key1: value1, key3: value3, key3: value3},
    // 데이터 모음. key안에 key:value를 담을 수 있다.
    변수2 : function(){
    // 함수
        for(let 변수3 in this.변수1){
            document.write(변수3+':'+this.변수1[변수3]+"<br />");
            // this는 약속되어있는 변수. 함수가 속해 있는 객체를 가리킨다. 즉 여기서는 변수0을 가리킴.
        }
    // key안에 function도 담을 수 있다.
    }
};
변수.show();
```

- 예시 코드

```javascript
var grades = {
    'list': {'egoing': 10, 'k8805': 6, 'sorialgi': 80},
    'show' : function(){
        for(var name in this.list){
            document.write(name+':'+this.list[name]+"<br />");
        }
    }
};
grades.show();
```