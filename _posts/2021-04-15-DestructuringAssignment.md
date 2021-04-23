---  
layout: post  
title: 객체 구조 분해 할당 복습 JS 21.04.15
subtitle: 
categories: dev
tags: js
comments: true  
--- 

구조 분해 할당(Destructuring assignment)은 array나 object의 속성을 분해하여 그 value를 개별 변수에 담을 수 있게하는 JS의 표현식.

- 기본 할당

```js
// syntax

// 객체 선언 후 키&값 할당
let 객체 = { 키1: 값1, 키2, 값2 }

// 구조 분해 할당 후 선언 : 각 키를 변수로 선언 후 값 할당 가능.
let { 키1, 키2 } = 객체
// 키1에 객체의 값1이 할당되고 키2에 객체의 값2가 할당된다.
```

```js
let creamer = { name: 'terry', job: 'dev' }

console.log(name);
console.log(job);
// name과 job은 변수가 아닌 creamer의 key이므로 어떠한 value도 표시되지 않는다.

let { name, job } = creamer;
// name과 key라는 변수에 creamer 객체가 가진 각 value를 할당한다.

console.log(name);
// terry
console.log(job);
// job
```

- 별도의 객체 선언 없이 바로 할당

```js
// syntax
let 변수1, 변수2;

// 괄호()는 선언 없이 바로 할당을 위해 필요한 문법이다.
({변수1, 변수2} = {변수1 : 값1, 변수2: 값2})
// === let {변수1, 변수2} = {변수1 : 값1, 변수2: 값2}
// 괄호 없이 사용하고 싶다면 선언을 해줘야한다.

// 괄호 없이 사용하는 경우
{변수1, 변수2} = {변수1 : 값1, 변수2: 값2}
왼쪽에 있는 {변수1, 변수2}는 객체 리터럴이 아닌 블록으로 간주되기 때문이다.
```

```js
let name, job;

({name, job} = {name : 'terry', job: 'dev'})
```

- 새로운 변수 이름으로 할당

```js
// syntax
let 객체 = { 키1: 값1, 키2: 값2 }

let { 키1: 변수1, 키2: 변수2 } = 객체;
// 키1의 변수명을 변수 1로 변경하고 키1의 값 할당
// 키2의 변수명을 변수 2로 변경하고 키2의 값 할당
```

```js
let creamer = { name: 'terry', job: 'dev' }

let {name: engName, job: secondJob} = creamer;

console.log(name);
console.log(job);
// 변수명을 변경해서 할당 했기 때문에 아무것도 나오지 않는다. 

console.log(engName);
// terry
console.log(secondJob);
// dev
```

- 기본 값 할당 : 객체를 해체하여 나온 값이 undefined인 경우, 변수에 기본값을 할당 가능

```js
// syntax
let { 변수명1(키1) = 기본값1, 변수명2(키2) = 기본값2 } = { 키1: 값1 }
//  객체에 키2&값2가 undefined이므로 변수명2(키2)에는 기본값2가 할당된다.
// 객체의 키1은 값1이 undefinde가 아니므로 변수명1(키1)은 객체의 값1 할당
```

```js
// name에 기본 값으로 creamer 할당, job의 기본값으로 dev 할당
// 객체에는 name의 값만 있으므로 name만 변경되고 job은 기본 값을 할당한다.
let { name = 'creamer', job = 'dev' } = { name: 'terry' }

console.log(name)
// terry
console.log(job);
// dev
```

- 기본값을 갖는 새로운 이름의 변수에 할당하기

```js
// syntax
let { 키1(변수명1) : 변경할 변수명1 = 기본값1, 키2(변수명2) : 변경할 변수명2 =  기본값2 }
= { 키1: 값1 }

// 키1(변수명1)을 변경할 변수명1로 변경한 후 값1을 할당한다.

// 키2의 값이 없으므로(undefined) 키2(변수명2)는 변경할 변수명 2로 변경된 후 기본값2를 할당한다.
```

```js
let { name: engName = 'creamer', job: secondJob = 'dev' } = { name: 'terry' }

console.log(engName)
// terry
console.log(secondJob)
// dev
```

- 함수 파라미터로 기본값 설정하기

```js
let helloDev = function({ greeting = 'hello', language: lang = 'JS' } = { } ) {
    console.log(greeting, lang)
}

helloDev()
// hello JS

helloDev( { lang: 'CSS' } )
// hello JS

helloDev( { language: 'CSS' } )
// hello CSS
// lange이 아닌 language로 써줘야한다.
// 인자로 들어가는 객체는 파라미터의 오른쪽 변을 의미하기 때문이다.
```

```js
// // 함수의 파라미터의 우측변에 빈 객체를 할당하지 않고 함수를 작성 가능 하지만 
// 빈 객체를 없앤다면 함수 호출 시 반드시 인자가 하나 들어가야한다.
let helloDev = function({ greeting = 'hello', language: lang = 'JS' } ) {
    console.log(greeting, lang)
}

helloDev();
// VM491:1 Uncaught TypeError: Cannot read property 'greeting' of undefined

// 위 함수는 무조건 객체를 인자로 사용해야할 경우 유용하다.
```

- 중첩된 객체 및 배열의 구조 분해

```js
let metadata = {
    title: "Scratchpad",
    translations: [
       {
        locale: "de",
        localization_tags: [ ],
        last_edit: "2014-04-14T08:43:37",
        url: "/de/docs/Tools/Scratchpad",
        title: "JavaScript-Umgebung"
       }
    ],
    url: "/en-US/docs/Tools/Scratchpad"
};

// title을 engTitle로 변경 한 후 거기에 metadata의 title의 값을 할당
// translations 안에 있는 배열 안에 있는 객체 안에있는 url의 이름을 URL로 변경 한후 해당 값 할당
let { title: engTitle, translations: [ { url: URL } ] } = metadata;

console.log(engTitle)
// "Scratchpad"
console.log(URL)
// /de/docs/Tools/Scratchpad
```

- for of 반복문과 구조 분해

```js
// 객체들을 가진 배열 people
let people = [
  {
    name: "Mike Smith",
    family: {
      mother: "Jane Smith",
      father: "Harry Smith",
      sister: "Samantha Smith"
    },
    age: 35
  },
  {
    name: "Tom Jones",
    family: {
      mother: "Norah Jones",
      father: "Richard Jones",
      brother: "Howard Jones"
    },
    age: 25
  }
];

// 배열을 순회하기 위해 for-of 문 사용
// n 변수에 people의 name 할당, f 변수에 people의 family 객체 안에 있는 father의 값 할당
for ( let {name: n, family: { father: f} } of people ) {
    console.log(`Name: ${n}, Father: ${f}`)
}

// Name: Mike Smith, Father: Harry Smith
// Name: Tom Jones, Father: Richard Jones
```

- 함수 매개변수로 전달된 객체에서 필드 분해 (user 객체로부터 id, displayName 및 firstName을 분해 후 출력)

```js
function userId( {id} ) {
    return id;
}

function whois( { displayName, fullName: { firstName: name } } ) {
    console.log(displayName + " is " + name);
}

let user = {
  id: 42,
  displayName: "jdoe",
  fullName: {
      firstName: "John",
      lastName: "Doe"
  }
};

// 아래 함수를 실행하면 함수의 인자는 { id } = user
userId(user) // 42

// 아래 함수를 실행하면 함수의 인자는 { displayName, fullName: { firstName: name } = user
whois(user) // jdoe is John
```

- 계산된 속성 이름과 구조 분해

```js
let key = "job";
let { [key]: fullTimeJob } = { job: "dev" };
// === let { job: fullTimeJob } = { job: "dev" };

console.log(fullTimeJob); // "dev"
```

- 객체 구조 분해에서 Rest

```js
// rest에 나머지 할당. rest = { c: 30, d: 40 }
let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}
a; // 10
b; // 20
rest; // { c: 30, d: 40 }
```

- 실습 예제 : getChanged 함수 실행시 user 객체의 company.name 변경한 값 출력 (spread syntax 사용). 원본 객체 user는 변경 X.

```js
let user = {
    name: 'terry',
    company: {
        name: 'unemployed',
        skill: ['Development', 'Writing'],
        role: {
            name: 'Blockchain Engineer'
        }
    },
    age: 30
}

let companyTurover = (companyName) => {
    return {
        // 객체 user를 spread syntax를 사용해 풀어서 복사해준다.
        ...user,
        // 객체 user의 모든 내용이 들어간 상태.

        // company의 값 변경
        company: {
            // company에 ...user.company의 모든 값을 풀어준다.
            ...user.company,
            // name을 변수로 받은 companyName으로 바꿔준다.
            name : companyName
        }
    }
}

let changeRole = (role) => {
    return {
        ...user,
        company : {
            ...user.company,
            role: { name: role }
        }
    }
}

companyTurover('google')
// {
//  name: 'terry',
//  company: {
//      name: 'google',
//      skill: ['Development', 'Writing'],
//      role: {
//          name: 'Blockchain Engineer'
//      }
//  },
//  age: 30
// }

changeRole('CTO')
// {
//  name: 'terry',
//  company: {
//      name: 'google',
//      skill: ['Development', 'Writing'],
//      role: {
//          name: 'CTO'
//      }
//  },
//  age: 30
// }
```