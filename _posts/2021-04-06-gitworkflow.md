---  
layout: post  
title: Git Workflow 21.04.05
subtitle: 
categories: dev
tags: js
comments: true  
--- 

# Git 환경 설정

- Git 최초 설정 (이름, 이메일)

Git init을 한 후 사용자 이름과 이메일 주소를 설정 (Git 커밋 기록에 해당 사용자 이름과 메일 주소가 기록)

--global로 설정하는 것은, 맨처음 단 한번만 해줘된다. 

```
$ git config --global user.name "Kim Coding"
$ git config --global user.email kimcoding@codestates.com
```

**git clone**으로 repo를 local에 가져온 경우 최초 설정이 필요 없다. 

- 에디터 설정

Git에서 커밋 메시지를 기록할 때 (특히 merge commit 확인 메시지가 나올 때) 에디터(vim, nano 등)가 필요하다.

Git을 설치하고 나면 Git의 사용 환경을 적절하게 설정해 주어야 한다. 환경 설정은 한 컴퓨터에서 한 번만 하면 된다. 설정한 내용은 Git을 업그레이드해도 유지된다. 언제든지 다시 바꿀 수 있는 명령어도 있다.

Git 최초 설정 상세 사항 : https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%B5%9C%EC%B4%88-%EC%84%A4%EC%A0%95

-설정 확인

```
$ git config --list
```
