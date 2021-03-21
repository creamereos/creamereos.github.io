---  
layout: post  
title: Array Method 21.03.16
subtitle: filter / map / reduce / forEach / find / sort / some /every
categories: dev
tags: js
comments: true  
--- 

##### filter() : function의 테스트를 통과(boolean)하는 모든 요소를 모아 새로운 배열로 반환
filter()는 **array 내 각 element**에 대해 한 번 **제공된 callback 함수를 호출**해, **callback이 true로 강제하는 값을 반환**하는 모든 값이 있는 **새로운 배열을 생성**합니다. callback은 할당된 값이 있는 배열의 인덱스에 대해서만 호출됩니다; 삭제됐거나 값이 할당된 적이 없는 인덱스에 대해서는 호출되지 않습니다. callback 테스트를 통과하지 못한 배열 요소는 그냥 건너뛰며 새로운 배열에 포함되지 않습니다.

filter()는 호출되는 배열을 변화시키지(mutate) 않습니다.

- syntax

```js
배열.filter(콜백함수(배열의 각 요소[, 인덱스[, 배열]])[, thisArg])
array.filter(callback(element[, index[, array]])[, thisArg])
// [] 안에 있는 것들은 선택 사항
```

- ex code : 조건 필터링.

```js
let arr = [1, 10, 100, 1000, 10000]

let biggerThanTen = function(arrEl) {
    return arrEl > 10;
}

arr.filter(biggerThanTen);
// [100, 1000, 10000]
```

vs

##### find() : 주어진 판별 함수를 만족하는 첫 번째 element의 값을 return

##### map() : array 내의 모든 element 각각에 대하여 주어진 function을 호출한 결과를 모아 새로운 배열을 반환
map은 호출한 배열의 값을 변형하지 않습니다. 단, callback 함수에 의해서 변형될 수는 있습니다.

- syntax

```js
배열.map(콜백함수(배열의 각 요소 순차적으로))
arr.map(callback(currentValue[, index[, array]])[, thisArg])
```

- ex code 0 : 기본

```js
let arr = [1, 4, 9];

let root = function(arrEl){
    return arrEl * 2;
}

arr.map(root);
// [2, 8, 18]
```

vs

##### forEach() : 주어진 함수를 배열 요소 각각에 대해 실행 (map과 다르게 하나의 배열로 만들어 return하지 않고 실행만)

##### reduce() : 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환

- syntax

```js
배열.reduce(콜백함수, [초기값])
// 초기값이 없는 경우 배열의 0번째 index가 초기값.
arr.reduce(callback[, initialValue])
```

- ex code :

```js
let arr = [1, 2, 3, 4, 5];
let reducer = (acc, cur) => acc + cur;

// 1 + 2 + 3 + 4
arr.reduce(reducer));
// 10

// 5 + 1 + 2 + 3 + 4
arr.reduce(reducer, 5));
// 15
```

콜백의 최초 호출 때 accumulator와 currentValue는 다음 두 가지 값 중 하나를 가질 수 있습니다. 

- reduce() 함수 호출에서 initialValue를 제공한 경우 : accumulator는 initialValue와 같고 currentValue는 배열의 첫 번째 값과 같습니다. + 배열의 인덱스 0에서 시작.

- reduce() 함수 호출에서 initialValue를 제공하지 않은 경우 : accumulator는 배열의 첫 번째 값과 같고 currentValue는 두 번째와 같습니다. 인덱스 1부터 시작


##### sort() : array의 element를 적절한 위치에 정렬한 후 정렬한 array를 반환

- syntax

```js
배열.sort([비교함수])
```

- ex code 0 : 기본

```js
let months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();

months;
// ["Dec", "Feb", "Jan", "March"]

let array1 = [1, 30, 4, 21, 100000];
array1.sort();
array1;
// [1, 100000, 21, 30, 4]
// 유니코드 코드 기준으로 정렬되기 때문에 숫자 크기 순으로 정렬이 안된다. 별도의 비교 함수가 필요하다.
```

- ex code 1 : 비교함수

```js
function 비교함수명(a, b) {
  if (a < b) {
    return -1; // false
  }
  if (a > b) {
    return 1; // true
  }
  // a와 brk 같다면
  return 0;
}

array1.sort(비교함수명);
// [1, 4, 21, 30, 100000]
```

##### some() : 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트. boolean 값으로 return

```js
function isBiggerThan10(element, index, array) {
  return element > 10;
}
[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
[12, 5, 8, 1, 4].some(isBiggerThan10); // true
```

##### every() : 메서드는 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트. boolean 값으로 return


```js
function isBigEnough(element, index, array) {
  return element >= 10;
}
[12, 5, 8, 130, 44].every(isBigEnough);   // false
[12, 54, 18, 130, 44].every(isBigEnough); // true
```

##### [심화 학습]

* MapReduce 학습하기 (MapReduce Model)
* 자바스크립트에서 커링(currying)과 클로져(closure)의 차이 이해하기 (js closure vs curry)
* 선언형 프로그래밍(declarative programming)과 절차형 프로그래밍(imperative programming)의 차이를 배열 메소드를 통해 이해하기 (js imperative vs declarative)
* 함수의 조합(function composition)에 대해 학습하기 (javascript function composition)

