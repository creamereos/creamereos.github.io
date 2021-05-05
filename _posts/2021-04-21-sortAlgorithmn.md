---  
layout: post  
title: Sort Algorithm 21.04.21
subtitle: 
categories: dev
tags: js algorithm
comments: true  
--- 

Sort Algorithm : 랜덤 데이터를 기준에 맞춰 정렬하는 알고리즘

# [Sort Algorithm 종류]

### Bubble sort

- 데이터를 2개 씩 비교하여 큰 쪽이 오른쪽으로 가도록 정렬

- 1번 반복하면 가장 큰 값이 가장 오른쪽으로 간다.

- 즉, n 번째 정렬 회차가 끝나면 n 번째 자리의 데이터가 확정된다.

- Big O : 정렬이 하나도 안되어있는 경우 : O(n^2) / 이미 정렬되어있는 경우 O(n)

- 장점 : 추가 메모리 공간이 필요하지 않고 데이터가 저장된 공강 내에서 정렬(in place 알고리즘)하기 때문에 메모리 절약. 구현 역시 쉽다.

