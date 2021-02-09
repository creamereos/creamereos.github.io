---  
layout: post
title: ES6 - symbol & set & map
categories: dev
tags: js
comments: true
---

- symbol : 새로운 data type (data type : string, number, boolean 등). 잘 사용하진 않는다.

```js
// symbol 안에 있는 값은 value가 아닌 단지 설명해주는 부분.
const hi = symbol("hi");
const bye = symbol("hi");

hi === bye
// false 
```

- set : 어떤 타입의 unique한 value든 저장할 수 있게 해준다. (object와 비슷하지만 특별한 규칙을 가지고 있다.)

```js
const testSet = new Set([1,2,3,3,3,4]);

console.log(testSet);
// Set(4) {1, 2, 3, 4}
// 중복된 value를 제거하고 unique한 value만 object에 담아준다.
```

- set.has() : 가지고 있는지 체크

```js
console.log(testSet.has(3));
// true
console.log(testSet.has(5));
// false
```


- set.delete() : 삭제

```js
console.log(testSet.delete(3));
// Set(3) {1, 2, 4}
```

- set.clear() : 전부 삭제

```js
console.log(testSet.clear());
console.log(testSet);
// Set(0) {0}
```

- set.add() : 추가

```js
console.log(testSet.add([1,2,3,4,5,5]));
// Set(1) {Array(6)}
// [[Entries]]
// 0: Array(6)
// value: (6) [1, 2, 3, 4, 5, 5]
```

등

- WeakSet() : 오로지 object만 저장 할 수 있음 (number, string, array 등 불가). 적용 가능한 api도 set에 비해 한정적(오로지 object로만 사용하기 때문) - 거의 사용하지 않음.

```js
const weakSet = new WeakSet();

const creamer = {
    job : dev
};

weakSet.add(creamer);
weakSet.add({age:30});
```
- Map (key value storage를 사용할 때 유용. 대부분 Set을 사용)

```js
const map = new Map();

console.log(map.set("age", 18));
// map은 object를 참조
// Map(1) {"age" => 18}

console.log(map.has("age"));
// true

console.log(map.get("age"));
// 18

// overwrite 가능
console.log(map.set("age", 30))
// Map(1) {"age" => 30} 
```

