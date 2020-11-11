---  
layout: post  
title: CSS - Page Layout
subtitle: Grid System, Boot Strap
categories: dev css  
tags: css grid
comments: true  
--- 

- Table
	- [Grid System](#Grid-System) 
	- [Boot Strap](#Boot-Strap)

### Grid System
페이지 전체를 나눠서 설계

- container
grid가 적용되는 전체 영역.

- column
grid의 한칸을 column (보통 12column 사용) column이 width 값을 설정.

- guter
column 양쪽의 여백.

#### Boot Strap
반응형 grid system 프레임워크

[부트스트랩 프레임워크](https://getbootstrap.com/)

**container의 자식은 반드시 row, row의 자식은 반드시 col로 시작.**

```html
<html>
<head>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">

</head>
</html>

<body>
    <div class="container">
        <div clss="row">
            <!-- 12칸 중 한칸 차지 -->
            <div class="col-1">
                <!-- 요소 -->
                <p>col-1</p>
            </div>

            <!-- 12칸 중 11칸 차지 -->
            <div class="col-11">
                <!-- 요소 -->
                <p>col-11</p>
            </div>
            <!--뷰포트 사이즈에 따라 차지하는 column수가 달라짐-->
             <div class="col-12 col-sm-6 md-4 col-lg-3 col-xl-2">
                <!-- 요소 -->

                
                <p>col-11</p>
            </div>
        </div>
    </div>
```

[반응형 부트스트랩 레이아웃](https://getbootstrap.com/docs/4.5/layout/overview/)

![](/assets/img/dev/2020-11-11-21-08-43.png)
