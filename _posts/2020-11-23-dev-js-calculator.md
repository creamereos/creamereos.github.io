---  
layout: post
title: 계산기 만들기
categories: dev
tags: HTML CSS js
comments: true
---

1. CSS GRID

- grid
```css
* {
    box-sizing: border-box;
    margin: 0px;
}

.container {
    width: 100%;
    height: 100%;
    padding : 20px;
    max-width: 300px;
}

.row {
    display: flex;
    flex-wrap: wrap;
    margin-right: -5px;
    margin-left: -5px;
}

.col-1 {
    width: 25%;
    padding: 0 5px;
}
.col-2 {
    width: 50%;
    padding: 0 5px;
}
.col-3 {
    width: 75%;
    padding: 0 5px;
}

.col-4 {
    width: 100%;
    padding: 0 5px;
}
```


2. HTML Mark up

```HTML

```

3. JavaScript

- Caluclate

```javascript

```

4. CSS Style

- style 
```css
* {
    box-sizing: border-box;
    margin: 0px;
}

.container {
    background-color: black;
    margin: 50px auto;
    border-radius: 30px;
    border: solid 3px #3a424e;
}

input {
    border: none;
    background-color: black;
}

input:focus {
    border: none;
    outline: none;
}

input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

button {
    margin-bottom: 10px;
    width: 100%;
    height: 55px;
    /* px 말고 %로 동그랗게 주는 법 */
    border-radius: 50%;
    border: none;
    outline: none;
    font-size: 30px;
    text-align: center;
}

button:hover {
    cursor: pointer;
    background-color: #3a424e;
    transition: 250ms ease-in-out;
}

.button-color1:hover {
    cursor: pointer;
    background-color: #e0e0e0;
    transition: 250ms ease-in-out;
}

.button-color2:hover {
    cursor: pointer;
    background-color: #ffffff;
    color: #ff9500;
    transition: 250ms ease-in-out;
}

.button-color3:hover {
    cursor: pointer;
    background-color: #3a424e;
    transition: 250ms ease-in-out;
}

.two-button {
    border-radius: 50px;
    text-align: left;
    padding-left: 20px;
}

.button-color1 {
    background-color: #b5b5b5;
    color: black;
}

.button-color2 {
    background-color: #ff9500;
    color: white;
}

.button-color3 {
    background-color: #252E3B;
    color: white;
}

#formula {
    width: 100%;
    height: 30px;
    text-align: right;
    color: gray;
    font-size: 25px;
}

#result {
    width: 100%;
    text-align: right;
    color: white;
    font-size: 50px;
}

@media screen and (max-width: 300px) {
    .container {
        width: 100%;
        height: auto;
    }
}
```