---  
layout: post  
title: JS 21.03.08
subtitle: 
categories: dev
tags: js
comments: true  
--- 

### while / Nested if else if / Nested for

- condition 

```js
if(true일 때 if문 실행)

for(true일 때까지 반복하다 false면 종료)
    
while(true일 때까지 반복하다 false면 종료)
```

- Nested if else if

```js
// ex code1
if (조건 0){
    if(조건1){
    실행1;
    } else if(조건2){
        실행2;
    } 
    실행0
}
// 조건 0이 true고 조건 1이 true면 실행 1;
// 조건 0이 true고 조건 1이 false고 조건 2가 true면 실행 2;
// 조건 0이 true고 조건 1이 false고 조건 2가 false면 실행 0;

// ex code1의 if와 else if문은 연관이 있음.

// ex code2
if(조건0){
    if(조건1){
    실행1;
    }
    if(조건2){
        실행2;
    }
    실행0;
}

// 조건 0이 true고 조건 1이 true면 실행 1;
// 조건 0이 true고 조건 2가 true면 실행 2;

// ex code2의 if와 if문은 연관이 없음.
```

- Nested for 

```js
for(let i=0; i <= x; i++){
    for(let j=0; i<= y; i++){
        console.log(String(i) + String(j));
    }
}

// 0 0
// 0 1
// 0 2
// ...
// 0 y

// 1 0
// 1 1
// 1 2
// ...
// 1 y
// ...
// ...
// x y
```

### querySelector querySelectorAll

- document.querySelector() : document 내에서 특정 부분을 선택하여 첫번째 element를 반환. (일치하는 요소가 없으면 null)

```js
// syntax
document.querySelector(selectors);

document.querySelector('#idName');
// id가 idName인 element를 선택

document.querySelector('.className');
// class가 className인 첫번째 element를 선택
```

- document.querySelectorAll() : document 내에서 특정 부분을 선택하여 첫번째 element를 반환. (일치하는 요소가 없으면 null)

```js
// syntax
document.querySelector(selectors);

document.querySelector('.className');
// class가 className인 모든 element를 선택

document.querySelectorAll("p");
// 모든 <p> 태그 선택
```

- ex code

```js
const container = document.querySelector("#test");
// document 내에 id가 test인 element를 선택
const matches = container.querySelectorAll("div.highlighted > p");
// id가 test인 element 내에서 parents element가 div이고 class가 highlighted인 p 태그 전체 선택
```

### textContent vs innerText vs innerHtml

##### textContent가 가장 효율성과 안정성이 좋음

- element.textContent : element의 text 내용을 선택. (text의 tyep은 string)

```js
// ex code
// Text 변경 전:
<div id="hello">hello</div>
<div id="world">world</div>

document.querySelector('#hello').textContent = '헬로'
document.querySelector('.world').textContent = '월드'

// Text 변경 후:
<div id="hello">헬로</div>
<div id="world">월드</div>
```

- element.innerText : element의 text 내용을 선택. (text의 tyep은 string)

```
textContent는 <script>와 <style> 요소를 포함한 모든 요소의 콘텐츠를 가져옵니다. 
반면 innerText는 "사람이 읽을 수 있는" 요소만 처리합니다.

textContent는 노드의 모든 요소를 반환합니다. 
그에 비해 innerText는 스타일링을 고려하며, "숨겨진" 요소의 텍스트는 반환하지 않습니다.
```

- element.innerHtml : HTML을 반환합니다. 간혹 innerHTML을 사용해 요소의 텍스트를 가져오거나 쓰는 경우가 있지만, HTML로 분석할 필요가 없다는 점에서 textContent의 성능이 더 좋습니다. 이에 더해, textContent는 XSS 공격의 위험이 없습니다.

### element.addEventListener() : 
addEventListener() method는 지정한 event가 대상(Element, Document, Window)에 전달될 때마다 호출할 function을 설정합니다. 일반적인 대상은 Element, Document, Window지만, XMLHttpRequest와 같이 이벤트를 지원하는 모든 객체를 대상으로 지정할 수 있습니다.

```js
// syntax
target.addEventListener(동작, 이벤트리스너[, 옵션]);
// syntax에서 []는 option을 이야기한다. 즉 [] 안에 있는 , 옵션 부분은 생략 가능하다.

// ex code 1
buttons.addEventListener('click', function (event) { 
    실행;
}
// buttons에 addEventListener를 붙임. 
// click 시 function 실행

buttons.addEventListener('mouseup', function (event) { 
    실행;
}
// buttons에 mouse 올려 놓을 시 function 실행



// ex code 2 : option
// element.addEventListener('행동', 이벤트 함수명, 옵션)
// 이벤트 함수는 별도로 설정.
outer.addEventListener('click', onceHandler, once);
// once : function을 한번만 실행
middle.addEventListener('click', captureHandler, capture);
// capture : 이벤트의 전송여부를 나타내는 Boolean 
```

