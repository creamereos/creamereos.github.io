---  
layout: post  
title: css- Web programming Work Flow
subtitle: 반응형 웹
categories: dev
tags: css html
comments: true  
--- 
 
# 섹션 별 모바일 먼저 디자인하고 그 다음 데스크탑 디자인 (데스크탑 디자인은 변경되는 부분만). 공통 요소 디자인은 따로 정리(편의성)

# 생각해야 할 것.

- Sectioning Elements 사용 (div 사용 최소화)
- class 사용 효율적으로
- grid 공부 필요
- hover 사용 시 transition 적용
- img, strong 등의 inline 태그에 margin bottom 등을 주기 위해서는 반드시 display: block 처리
- 가상 요소, nth-child 적재적소에 사용하면 효율적
- flex도 너무 남발할 필요는 없음
- 부트스트랩 관련 태그는 이미 flex가 적용되어있음. HTML 태그 클래스를 통해 flex 관련 기능 적용이 가능하다.
- 가운데 정렬하는 2가지 방법. 간단한 가운데 정렬은 margin(0px auto;). 복잡한건 flex 사용
- flex-grow / flex-shrink

```html
<div class="container">
        <div class="row justify-content-center">
            <div class="col-12">
                <section>
                    <h1>Change your career,Change your life</h1>
                    <p> Get ahead with expert-led training in
                        coding & design</p>
                </section>
            </div>
        </div>
    </div>
```


### 1. HTMl 마크업

- CSS / grid / font 적용

```html
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&display=swap" rel="stylesheet">
    <link href='grid.css' rel='stylesheet'>
    <link href='style.css' rel='stylesheet'>
```

- [form](https://creamereos.github.io/dev/2020/10/21/html/)
H태그:제목, p태그: 문단, section 태그 : 각 화면.

- [Page layout-Grid](https://creamereos.github.io/dev/2020/11/11/dev-css-grid/)
각 섹션을 각 grid(container)에 넣어줘야한다.

```html
<div class="container">
        <div class="row">
            <div class="col-12">
                <section>
                    <h1>Change your career,Change your life</h1>
                    <p> Get ahead with expert-led training in
                        coding & design</p>
                </section>
            </div>
        </div>
    </div>
```

- [Sectioning Elements](https://creamereos.github.io/dev/2020/10/26/html/)

- Class 명 적용 (디자인 별) 

### 2. CSS 

##### 2-1. 전체 적용되는 디자인

```css
* {
    box-sizing: border-box;
    margin-top:0;
}
```

##### 2-2. CSS Reset : 기본 웹 브라우저의 구린 디자인 제거
CSS Reset : https://cssreset.com/scripts/eric-meyer-reset-css/

```css

ex)
button, input, textarea {
    font-family: 'DM Sans', sans-serif;
    /* body에만 설정해주면 폰트 적용이 안됨. 때문에 따로 적용 필요 */
    font-size: 16px;
}
button:focus, input:focus, textarea:focus,
button:active, input:active, textarea:active {
    /* 파란선 없애기 */
    box-shadow: none;
    outline: none;
}

ul,ol,li {
    list-style-type: none;
    padding-left: 0;
    margin-left: 0;
}
```

##### 2-3. 모바일 : 공통되는 디자인 요소 찾기

```css
p {
    font-size: 16px;
    line-height: 1.5em;
    color: #2B292D;
    letter-spacing: 0.01em;
}
```

##### 2.4 데스크탑 : 공통되는 디자인 중 데스크탑에서 변경되는 부분만

```css
/* >> 768px (Desktop) */
@media screen and (min-width: 768px) {
    p {
        font-size: 22px;
        line-height: 1.3636em;
    }
}
```

##### 2.5 모바일 : 기본 스타일 (font, color etc) 레이아웃 (text align, flex, postion, margin,)

##### 2.6 데스크탑 : 기본 스타일 (font, color etc) 레이아웃 (text align, flex, postion, margin,)