---  
layout: post
title: nodeJS - sequelize
categories: dev
tags: node
comments: true
---

sequelize : MySQL 작업을 쉽게 할 수 있도록 도와주는 라이브러리.

sequelize를 쓰는 이유는 자바스크립트 구문을 알아서 SQL로 바꿔주기 때문이다. 따라서 SQL 언어를 직접 사용하지 않아도 자바스크립트만으로 MySQL을 조작할 수 있고 SQL을 몰라도 MySQL을 어느정도 다룰 수 있게된다.

# 관계 정의하기 

- hasMany : 1:N 관계 (ex: 사용자 한명이 댓글을 여러개 작성). 
- belongsTo : N:1 관계 (ex: 한 게시글에 여러명의 사용자), 다른 모델의 정보가 들어가는 테이블에 belongsTo 사용.