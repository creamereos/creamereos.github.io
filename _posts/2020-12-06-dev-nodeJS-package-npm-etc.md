---  
layout: post
title: nodeJS - 기타 npm 명령어
categories: dev
tags: node
comments: true
---

- npm outdated : 업데이트 할 수 있는 패키지가 있는지 확인

```
$ npm outdated 
```

```json
Package          Current  Wanted  Latest  Location
cookie-parser    MISSING   1.4.5   1.4.5  npmtest
express          MISSING  4.17.1  4.17.1  npmtest
express-session  MISSING  1.17.1  1.17.1  npmtest
morgan           MISSING  1.10.0  1.10.0  npmtest
rimraf           MISSING   3.0.2   3.0.2  npmtest
```

Current와 Wanted가 다르다면 업데이트가 필요하다.

- 패키지 업데이트 : 가능한 모든 패키지가 Wanted에 적힌 버전으로 업데이트 된다. Latest는 해당 패키지의 최신 버전이지만 package.json에 적힌 버전 범위와 다르다면 설치되지 않는다.

```
$ npm update 패키지명 패키지명 ...
```

- 패키지 제거하기 : 해당 패키지가 node_modules 폴더와 package.json에서 사라진다.

```
$ npm uninstall 패키지명 패키지명 ...

또는

$ npm rm 패키지명 패키지명 ...
```

- 패키지 검색하기 : 윈도우나 맥에서는 브라우저를 통해 [npm 공식 사이트](https://npmjs.com){: target="_blank"}에서 검색하면 편리하다. 

콘솔로도 검색 가능하다. (이 때 package.json에 있는 keywords가 사용된다.)

```
$ npm search 검색어
```

- 패키지의 세부 정보 파악하기 : package.json의 내용과 의존관계, 설치 가능한 버전 정보 표시

```
$ npm info 패키지명
```

- npm 로그인 : [npm 공식 사이트](https://npmjs.com){: target="_blank"}에서 가입한 계정으로 로그인 하면 된다. **추후 패키지 배포시 로그인이 필요**하다.

```
$ npm adduser
Username: 사용자 이름 입력
Password: 비밀번호 입력
Email: 이메일 입력
Logged in as 사용자 이름 on https://resigtry.npmjs.org
```

- npm에 로그인한 사용자 파악 (로그인 상태가 아니라면 에러 발생)

```
$ npm whoami
```

- npm 로그아웃 : npm adduser로 로그인한 계정을 로그아웃 할 때 사용

```
$ npm logout
```

- npm 버전 업데이트 : package.json의 버전을 올린다.

```
$ npm version [원하는 버전의 숫자]

또는

major, minor, patch 라는 문자열을 넣어 해당 부분의 숫자를 1 올릴 수 있다.

$ npm version 5.3.2, npm version minor
= 5.4.2
```

- 해당 패키지를 설치 할 때 경고 메시지 띄우기 (자신의 패키지에만 이 명령어를 적용 가능)

```
npm deprecate [패키지명] [버전] [메시지]
```

- 자신이 만든 패키지 배포

```
npm publish 
```

- 자신이 배포한 패키지 제거 (다른 사람이 사용하고 있는 패키지를 제거하는 경우를 막기 위해 24시간 이내에만 가능.)

```
npm unpublish 
```

- package.json 대신 package-lock.json에 패키지 설치 (더 엄격하게 버전을 통제하여 패키지를 설치하고 싶을 때 사용)

```
npm ci
```

이 외의 명령어는 [npm 공식문서](https://docs.npmjs.com){: target="_blank"}의 CLI Commands에서 확인 가능