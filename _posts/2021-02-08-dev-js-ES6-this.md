---  
layout: post
title: ES6 - this in classes
categories: dev
tags: js
comments: true
---

- this는 기본적으로 class 안에서 사용 할 때 class 그 자체를 가리킨다. 하지만 class와 function을 어떻게 정의하느냐에 따라 this가 가리키는 것이 달라진다.

- class ex code 1) 코드 깔끔하게 정리하기 + this

# this가 항상 class를 가리키게 하려면 arrow function을 활용해라!

index1.html

```html
<span id="count"></span>
<button id="plus">+</button>
<button id="minus">-</button>
<script src="app.js"></script>
```

index2.html

```html
<span id="count2"></span>
<button id="plus2">+</button>
<button id="minus2">-</button>
<script src="app.js"></script>
```

app.js

```js
class Counter {
    constructor({ initialNumber = 0, counterId, plusId, minusId}) {
        this.count = initialNumber;
        this.count.innerText = initialNumber;
        this.counter = document.getElementById(counterId);
        this.plusButton = document.getElementById(plusId);
        this.minusButton = document.getElementById(minusId);
        this.addEventListener();
    }
    addEventListner = () => {
        // function을 호출하면 JS는 this를 변경한다.
        this.plusButton.addEventListner("click", this.increse);
        this.minusButton.addEventListner("click", this.decrese);
    }
    // arrow function을 사용해야 this가 class를 가리킨다.
    increse = () => {
        //  이 경우 this는 class를 가리키는 것이 아닌 element를 가리킨다.
        this.count = this.count + 1;
        this.displayCount();    
    }
    // arrow function을 사용해야 this가 class를 가리킨다.
    decrese = () => {
        //  이 경우 this는 class를 가리키는 것이 아닌 element를 가리킨다.
        this.count = this.count - 1;
        this.displayCount();    
    }
    displayCount = () => {
        this.counter.innerText = this.count;
    }
}

new Counter({counterId:"count", plusId:"plus", minusId:"minus"});
new Counter2({initialNumber: 1000, counterId:"count2", plusId:"plus2", minusId:"minus2"});

// class로 설계도를 만들어서 여러 곳에 적용 가능하다.
```
