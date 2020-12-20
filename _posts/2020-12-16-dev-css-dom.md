---  
layout: post
title: DOM - Element Node Inline Styles
categories: dev
tags: css dom
comments: true
---

### Style Attribute (a.k.a. Element Inline CSS Properties) Overview

```html
<div style="background-color:red;border:1px solid black;height:100px; width:100px;"></div>
```

```js
const divStyle = document.querySelector('div').style;

console.log(divStyle);
// CSSStyleDeclaration {0="background-color", 1=..., 2=... ...}
```

### Getting, Setting, and Removing Individual Inline CSS Properties

```html
<div></div>
```

```js
const divStyle = document.querySelector('div').style;

//SET
divStyle.backgroundColor = 'red'; 
divStyle.border = '1px solid black'; 
divStyle.width = '100px'; 
divStyle.height = '100px';

//or

divStyle.setProperty('background-color','red');
divStyle.setProperty('border','1px solid black'); 
divStyle.setProperty('width','100px'); 
divStyle.setProperty('height','100px');

//GET
console.log(divStyle.backgroundColor); 
console.log(divStyle.border); 
console.log(divStyle.width); 
console.log(divStyle.height);

//or

console.log(divStyle.getPropertyValue('background-color')); 
console.log(divStyle.getPropertyValue('border')); 
console.log(divStyle.getPropertyValue('width')); 
console.log(divStyle.getPropertyValue('height'));

//REMOVE
divStyle.backgroundColor = ''; 
divStyle.border = ''; 
divStyle.width = ''; 
divStyle.height = '';

//or

divStyle.removeProperty('background-color'); 
divStyle.removeProperty('border'); 
divStyle.removeProperty('width'); 
divStyle.removeProperty('height');
```

### Getting, Setting, and Removing All Inline CSS Properties

- cssText

```html
<div></div>
```

```js
const div = document.querySelector('div'); 
const divStyle = div.style;

//set using cssText
divStyle.cssText = 'background-color:red;border:1px solid black;height:100px; width:100px;';

//get using cssText
console.log(divStyle.cssText); 

//remove
divStyle.cssText = '';

//or

//set using setAttribute
div.setAttribute('style','background-color:red;border:1px solid black; height:100px;width:100px;');

//get using getAttribute
console.log(div.getAttribute('style')); 

//remove
div.removeAttribute('style');
```

### Using getComputedStyle() to Get an Element’s Computed Styles

```html
<div style="background-color:green;border:1px solid purple;"></div>
```

```css
div{
    background-color:red; 
    border:1px solid black; 
    height:100px; 
    width:100px;
}
```

```js
const div = document.querySelector('div');
//logs rgb(0, 128, 0) or green, this is an inline element style
console.log(window.getComputedStyle(div).backgroundColor);
/* logs 1px solid rgb(128, 0, 128) or 1px solid purple, this is an inline element style */
console.log(window.getComputedStyle(div).border);
//logs 100px, note this is not an inline element style
console.log(window.getComputedStyle(div).height); 
//logs 100px, note this is not an inline element style
console.log(window.getComputedStyle(div).width);
```

###  Using the class and id Attributes to Apply and Remove CSS Properties on an Element

```css
<style>
.foo{ background-color:red; padding:10px;
} 

#bar{
  border:10px solid #000;
  margin:10px;
}
```

```html
<div></div>
```

```js
const div = document.querySelector('div');

//set
div.setAttribute('id','bar');
div.classList.add('foo');

// remove 
div.removeAttribute('id'); 
div.classList.remove('foo');
```

