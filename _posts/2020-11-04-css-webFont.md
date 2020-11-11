---  
layout: post
title: CSS - web font
subtitle: 
categories: dev
tags: css webfont
comments: true  
--- 

# web font
CSS로 폰트를 제공해도 사용자가 폰트가 없으면 폰트 적용이 안되므로 web font 사용.

### web font 제공 방법

- font.google.com
1. HTML head에 (복붙)
2. CSS 적용 (복붙)

~~~
[CSS]
body {
  font-family: 'font Name'
}
~~~

- 내 컴퓨터에 있는 폰트에 적용하기.
1. assets 안에 font 파일을 넣어준다.

~~~
[CSS]

@font-face {
  font-family: 'MyWebFont';
  src: url('webfont.eot'); /* IE9 Compat Modes */
  src: url('webfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
       url('webfont.woff2') format('woff2'), /* Super Modern Browsers */
       url('webfont.woff') format('woff'), /* Pretty Modern Browsers */
       url('webfont.ttf')  format('truetype'), /* Safari, Android, iOS */
       url('webfont.svg#svgFontName') format('svg'); /* Legacy iOS */
}

https://css-tricks.com/snippets/css/using-font-face/
~~~

폰트 적용 방법

~~~
[HTML]
<link rel = 'stylesheet' href="./fonts.css">

또는

[CSS]  
@import('./fonts.css')
~~~
