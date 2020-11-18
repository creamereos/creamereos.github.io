---  
layout: post
title: JS - module, library, API
subtitle:
categories: dev
tags: javascript
comments: true
---
모듈화 : 각각의 기능을 가진 코드를 여러개의 파일로 분리 후 재활용성을 높이고 유지보수를 쉽게 유지.

### 모듈화의 장점
- 자주 사용되는 코드를 별도의 파일로 만들어서 필요할 때마다 재활용 가능

- 코드를 개선하면 이를 사용하고 있는 모든 애플리케이션의 동작이 개선

- 코드 수정 시에 필요한 로직을 빠르게 찾기 가능

- 필요한 로직만을 로드해서 메모리의 낭비를 줄일 수 있다.

- 한번 다운로드된 모듈은 웹브라우저에 의해서 저장되기 때문에 동일한 로직을 로드 할 때 시간과 네트워크 트래픽을 절약 할 수 있다. (브라우저에서만 해당)

### library
라이브러리는 모듈과 비슷하지만 모듈이 프로그램을 구성하는 작은 부품으로서의 로직을 의미한다면 라이브러리는 자주 사용되는 로직을 재사용하기 편리하도록 잘 정리한 일련의 코드들의 집합을 의미.좋은 라이브러리를 선택하고 잘 사용하는 것은 프로그래밍의 핵심이라고 할 수 있다. 

- [jQuery](http://jquery.com/)

- [jQuery 메뉴얼](http://api.jquery.com/)

### API
- API(Application Programming Interface) 프로그램이 동작하는 환경을 제어하기 위해서 환경에서 제공되는 조작 장치이다. 이 조작 장치는 프로그래밍 언어를 통해서 조작할 수 있다. 

- reference : 명령어의 사전을 의미

- tutorial : 언어의 문법을 설명

### 자바스크립트 API 문서 (우선 학습)
- ECMAScript (표준문서)
- 자바스크립트 사전 (생활코딩)
- 자바스크립트 레퍼런스 (MDN)
- jscript 레퍼런스 (MSDN)

### 호스트 환경의 API 문서 (추후 학습)
- 웹브라우저 API
- Node.js API
- Google Apps Script API
- React

출처 : https://opentutorials.org/course/743/6533