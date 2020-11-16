---  
layout: post
title: JS - forEach
subtitle:
categories: dev
tags: javascript
comments: true  
--- 
[Array.prototype.forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

The 'forEach()' method executes a provided function once for each array element.

```javascript
const array1 = ['a', 'b', 'c'];

array1.forEach(element => console.log(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

- syntax

```javascript
arr.forEach(callback(currentValue[, index[, array]]) {
  // execute something
}[, thisArg]);
```