---  
layout: post
title: 자바스크립트 이벤트 처리 방식 addEventListener
categories: dev
tags: js
comments: true
---

1. HTML 요소에 직접 이벤트 처리 속성 설정.

```HTML
<button type="button" onclick="buttonClick()">
<!-- buttonClick()이라는 함수를 별도로 JS에 만들어 줘야한다. -->
```

가독성이 떨어지고, 이벤트를 단 하나밖에 설정 못하므로 효율성이 떨어진다. (동일한 이벤트 적용시 여러번 입력해야함)

2. addEventListener
addEventListener는 이벤트를 등록하는 가장 권장되는 방식으로 여러개의 이벤트 핸들러를 등록할 수 있다.

```javascript
const A = document.getElementById('target');
// document에서 id가 target인 요소를 변수 A에 담아준다.

A.addEventListener('click', function(함수명){
    // A를 click시 함수를 실행한다.
    alert('Hello world, '+함수명.target.value);
    // 함수 작동 시 실행
});
```