---  
layout: post
title: nodeJS - built-in module (url / querystring)
categories: dev
tags: nodeJS
comments: true
---

- url : 인터넷 주소를 쉽게 조작하도록 도와주는 모듈.

url.js

```js
const url = require('url');

const { URL } = url;
const myURL = new URL('https://creamereos.github.io/');

console.log(myURL);

console.log(url.format(myURL));
// url.fomat(객체) WHATWG 방식 url과 기존 노드의 url을 모두 사용 가능. 분해 되었던 url 객체를 다시 원래 상태로 조립

const parsedUrl = url.parse('https://creamereos.github.io/');

console.log(parseURL);
// url.parse('url 주소') : 주소를 분해. (WHATWG과 비교하면 username, password 대신 auth 속성이 있고, searchParams 대신 query가 있다.)

console.log(url.format(parseURL));
// url.fomat(객체) WHATWG 방식 url과 기존 노드의 url을 모두 사용 가능. 분해 되었던 url 객체를 다시 원래 상태로 조립
```

[WHATWG url vs node url] (취향에 따라 사용 가능) 
- 노드 url 형식을 꼭 사용해야하는 경우 : host 부분 없이 pathname 부분만 오는 주소인 경우 WHATWG 방식이 처리 할 수 없다. (ex: /book/booklist.apsx)

- WHATWG 방식은 search 부분을 **serachParams**라는 특수한 객체로 반환하므로 유용하다.

searchParams.js

```js
// url 모듈 불러오기
const { URL } = require('url');

// URL 생성자(new URL)을 통해 myURL이라는 주소 객체 생성
const myURL = new URL('https://creamereos.github.io/');

// myURL 안에는 searchParams라는 객체가 있다. 이 객체는 search 부분을 조작하는 다양한 method를 지원한다.
console.log(myURL.searchParams);

// getAll(키) : (키)에 해당하는 모든 값들을 가져온다. 
console.log(myURL.searchParams.getAll('category'));

// get(키) : 키에 해당하는 첫번째 값만 가져온다.
console.log(myURL.searchParams.get('limit'));

// has(키): 해당 키가 있는지 없는지 검사
console.log(myURL.searchParams.has('page'));

// keys(): searchParams의 모든 키를 반복기(iterator) 객체로 가져온다.
console.log(myURL.searchParams.keys());

// values() : searchParams의 모든 값을 반복기(iterator) 객체로 가져온다.
console.log(myURL.searchParams.values());

// append(키, 값) : 해당 키를 추가한다. 같은 키의 값이 있다면 유지하고 하나 더 추가한다. 
myURL.searchParams.append('filter', 'es3');
console.log(myURL.searchParams.getAll('filter'));

myURL.searchParams.append('filter', 'es5');
console.log(myURL.searchParams.getAll('filter'));

// set(키, 값) : 같은 키의 값을 모두 지우고 새로 추가한다.
myURL.searchParams.set('filter', 'es6');
console.log(myURL.searchParams.getAll('filter'));

myURL.searchParams.set('delete', 'es6');
console.log(myURL.searchParams.getAll('filter'));

// delete(키) : 해당 키를 제거한다.
myURL.searchParams.delete('filter');
console.log(myURL.searchParams.getAll('filter'));

// toString(): 조작한 searchParams 객체를 다시 문자열로 만든다. 이 문자열을 search에 대입하면 주소 객체에 반영된다.
console.log(myURL.searchParams.toString());
myURL.search = myURL.searchParams.toString();
```

- querystring : WHATWG 방식의 url 대신 **기존 노드의 url을 사용할 때**, **search 부분을 사용하기 쉽게 객체로 만드는 모듈**

querystring.js

```js
// 실제 프로젝트에서도 모듈 여러개를 파일 하나에 불러올 수 있다.
const url = require('url');
const querystring = require('querystring');

const parsedUrl = url.parse('https://creamereos.github.io/dev/2020/12/01/dev-nodeJS-process/');

// querystring.parse(쿼리) : url의 query 부분을 자바스크립트 객체로 분해한다.
const query = querystring.parse(parsedUrl.query);
console.log(query);

// querystring.stringify(쿼리) : url의 query 부분을 자바스크립트 객체를 문자열로 재조립.
console.log(querystring.stringify(query));
```