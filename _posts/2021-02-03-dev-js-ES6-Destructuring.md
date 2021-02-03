---  
layout: post
title: ES6 - Destructuring
categories: dev
tags: js
comments: true
---

- Destructuring(비 구조화) : object나 array 등의 요소들 안의 변수를 밖으로 꺼내서 사용

### object Destructuring

- ES6 이전 

```js
const settings = {
    notifications: {
        follow: true,
        alert: true,
        unfollow: false
    },
    color: {
        theme: "dark"
    }
}

if(setting.notifications.follow) {
    // send email
}
```

- ES6 (Destructuring object)

```js
const settings = {
    notifications: {
        follow: true,
        alert: true,
        unfollow: false
    },
    color: {
        theme: "dark"
    }
}

const { 
    notifications: { follow },
    // 위 코드에서 변수는 follow
    // notifications: { follow } 부분은 const follow = settings.notification.follow; 와 같은 의미.
    color,
    // 변수는 color
    // color 부분은 const color = settings.color; 와 같은 의미.
    notifications: { comment = "기존에 없던 Object 값 넣어주기"},

    // object 생성해주기
    test: { text = "Text Test" } = {}
} = settings;
// settings 안에 있는 color와 notifications에 접근하여 그 안에 있는 follow를 가지고온다.

// object의 값 가져오기
console.log(follow);
// true

// object 전체 가져오기
console.log(color);
// {theme: "dark"}

// 기존 setting에 없는 object도 기본 값을 설정해서 넣어줄수 있다.
console.log(comment);
// "기존에 없던 Object 값 넣어주기"

// 기존 setting에 없는 object를 생성하고 그 안에 object를 생성하고 그 안에 기본 값을 설정해서 넣어줄수 있다
console.log(text);
// "Text Test"
```

- rename

```js
const settings = {
    color: {
        a1b2c3d4: "Dark"
    }
};

const {
    // rename : a1b2c3d4를 변수 theme에 담아준다. theme(=a1b2c3d4)이 없으면 Light를 넣어준다.
    color: { a1b2c3d4 : theme = "Light"}
} = settings;

console.log(theme);
// Drak
```

- update

```js
const settings = {
    color: {
        a1b2c3d4: "Light"
    }
};

let theme = "Dark";
console.log(theme);
// Dark

// () 안에 담아줘서 변수를 업데이트 가능하다.
({
    color: { a1b2c3d4 : theme = "them(a1b2c3d4)이 없는 경우에만 출력"}
} = settings);

console.log(theme);
// Light
```

### array Destructuring

가져온 정보를 조작하지 않을 때 사용하기 좋음.

- Destructuring 이전

```js
const days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

const mon = days[0];
const tue = days[1];
const wed = days[2];

console.log(mon, tue, wed);
// Mon Tue Wed
```

- Destructuring 이후

```js
const days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

// days란 배열의 각 element를 순서대로 변수 a, b, c, , e에 담아준다.
const [a, b, c, , e] = days;

console.log(a, b, c, e);
// Mon Tue Wed
```

- object function Destructuring

```js
// saveSettings 함수 만들기. arguments의 길이가 길기 때문에 따로 설정해주기
function saveSettings({ notifications, color: { theme } }) {
    console.log(color);
}

// arguments의
saveSettings({
    notifications: {
        follow: true,
        alert: true,
        mkt: false,
    },
    color: {
        theme: "green"
    }
});
```

- value 단축(key 값이랑 변수 명이 같을 때만 사용 가능)

```js
const follow = checkFollow();
const alert = checkAlert();

const settings = {
    notification: {
        // follow: follow, - 이 코드를 아래와 같이 단축 가능. 값 생략
        follow,
        // alert: alert - 이 코드를 아래와 같이 단축 가능. 값 생략
        alert
    }
}

// settings라는 object 안에 notification이 있고 그 안에 있는 follow와 alert은 각각 follow = checkFollow();,  alert = checkAlert();와 같다.
```

- variable swapping 

```js
let mon = "Sat";
let sat = "Mon";

// array 생성
[mon, sat] = ["Mon", "Sat"]; 
```

- skipping

```js
const numbers = [1,2,3,4,5];

// , 로 건너 뛰기 가능
const [,,, 4, 5] = numbers;

console.log(4, 5);
// 4, 5
```