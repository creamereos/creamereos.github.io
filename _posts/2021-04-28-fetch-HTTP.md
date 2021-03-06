---  
layout: post  
title: HTTP / AJAX 21.04.28
subtitle: 
categories: dev
tags: js
comments: true  
--- 

프로토콜 : 클라이언트와 서버 간의 통신 규약(약속) 및 방법 
(ex : 웹 애플리케이션 아키텍처에서는 클라이언트와 서버가 HTTP라는 프로토콜을 이용해서 통신)


### 주요 프로토콜 종류

[전송 계층]
TCP : HTTP, FTP 통신의 근간이 되는 **양방향** 인터넷 프로토콜. 서버가 항상 켜져있어야함. (가장 많이 사용. HTTP 1.0이 사용) 전화랑 비슷하다고 생각
UDP : 단방향으로 작동하여 훨씬 단순하고 빠르지만 신뢰성이 낮음. (HTTP 2.0이 사용)

전송 계층은 비연결성과 무상태성을 갖고있지않다.

[응용 계층]
HTTP : 웹에서 HTML, JSON 등의 정보를 주고 받는 프로토콜. 비연결성, 무상태성 (API 요청 <=> 상태코드(ex: 200) + data) 문자랑 비슷하다고 생각
HTTPS : HTTP에서 보안이 강화된 프로토콜
FTP : 파일 전송 프로토콜
SMTP : 메일 전송 프로토콜
SSH : CLI 환경의 원격 컴퓨터에 접속하기 위한 프로토콜 ( ex : AWS )
RDP : 윈도우 계열의 원격 컴퓨터에 접속하기 위한 프로토콜 
webSocket : 실시간 통신, Push 등을 지원하는 프로토콜


### HTTP 메시지

서버와 클라이언트 간에 데이터가 교환되는 방식. HTTP 메시지는 ASCll로 인코딩된 텍스트 정보이며 여러줄로 되어있다. HTTP 메시지 타입은 요청(Request)과 응답(Response) 2가지가 있다.

### HTTP 상태 코드

100번대 : 정보 확인
**200번대 : 통신 성공** 
300번대 : 리다이렉트
**400번대 : 클라이언트 오류**
**500번대 : 서버 오류**

200 OK : 요청이 성공했을 때.
400 Bad Request : 문법이 잘못되어 서버가 요청을 이해 할 수 없을 때
403 Forbidden : 접근 금지 (권한 없이 접근을 시도한 경우)
404 Not Found : 서버가 요청 받은 리소스를 찾을 수 없을 때. (브라우저에서 알려지지 않은 URL을 의미)
502 Bad Gateway : 게이트웨이 오류
503 Service Unavailable : 서비스 이용 불가 (일시적으로 클라우드 서버 측에서 과부하가 걸리거나 다운 되는 경우)

# API (Application Programming Interface)

서버가 클라이언트에게 리소스를 잘 활용할 수 있도록 제공하는 프로그래밍 가능한 인터페이스(interface : 의사소통이 가능하도록 만들어진 접점). 백엔드 개발자와 프론트 엔드 개발자가 정한 명시적인 규칙.  (비유하자면, 메뉴판과 같은 역할)

(ex : 인터넷 상의 데이터 요청시 HTTP 프로토콜을 사용하며, 주소(URL, URI)를 통해 접근 가능)

```
// 아메리카노 2잔에 헤이즐넛 시럽 요청
/coffee/americano?quanity=2&syrup=hazelnet


// 파라미터 
?quanity=2&syrup=hazelnet
// 파라미터를 사용하기 위해서는 물음표(?) 기호와 & 기호 사용 필요
```

### HTTP API 디자인을 잘하는 방법 : 실사용 사례

HTTP API 디자인에는 모범 사례가 있습니다.

URL 디자인은 단순화 하고, 메소드(GET, POST, PUT, DELETE)  활용 

### [HTTP 요청 메서드]

GET : 조회(Read) : 특정 리소스의 표시를 요청. 오직 데이터를 받기만 한다.
HEAD : GET과 동일한 응답을 요구하지만, 응답 본문을 포함하지 않는다.
POST : 추가(Create) : 특정 리소스에 엔티티를 제출 할 때 사용. 서버의 상태 변화나 부작용 일으킬 수 도 있다.
PUT : 리소스의 전체 업데이트
PATCH : 리소스의 부분만을 수정할때 사용
DELETE : 삭제 (Delete) : 특정 리소스 삭제

# Ajax (Asynchronous JavaScript and XML : 비동기식 자바스크립트와 XML)

자바스크립트를 사용한 비동기 통신, 클라이언트와 서버간에 데이터를 주고 받는 기술.

Ajax 이전에는 동기식으로 브라우저에서 페이지 요청하고, 서버에서 요청한 페이지를 찾은 후 필요한 작업을 한 후 결과물을 브라우저로 보내준다. 그러면 브라우저에서 이 내용을 표시한다.  페이지 전환 시 새로운 페이지를 렌더하므로 깜빡이며 시간이 오래 걸렸다.

그러나 Ajax 방식을 사용할 때는 중간에 한 가지 과정이 더 있다. 브라우저가 서버에 페이지 전체를 요청하는 대신, 필요한 내용만 요청한다. 그 결과가 오면 화면 전체에 뭔가를 그리는 대신 화면 위에 있는 내용을 곧바로 DOM을 사용해 조작한다. 이를 dynamic web page라고 하며 서버와 자유롭게 통신(  XMLHttpRequest / JQeury  / fetch API ) 할 수 있고 페이지 깜빡임 없이 seamless하게 작동한다. 단순한 웹 페이지가 아닌 웹 앱이라고 한다.

##### json 직렬화 & 병렬화

JSON.stringfy() : 서버로 JSON을 보낼 때
JSON.parse() : 서버에서 받은 JSON을 해석할 때

Fetch API
response.json() : 서버에서 받은 응답의 body 속 JSON을 해석할 때

##### 싱글턴 패턴 : 전역변수를 선언하지 말고 하나의 객체에서 모든것을 다 하는 패턴


