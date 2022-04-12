---
layout: post
title:  Bit operation
date:  2022-04-12 13:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  java, algorithm] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

알고리즘 문제 풀다가 공부한 bit operation 정리 

---

# 1. Bit operation 
비트 단위의 연산에 대해서 공부하기 위해서는 우선 비트가 뭔지 알아야한다.  

## Bit 란?  
컴퓨터에서  0 혹은 1의 binary digit을 저장하는 가장 작은 단위의 데이터이다. 너무 작은 단위이기에 하나의 bit 씩 사용되기 보단 byte처럼 bit 의 묶음이 주로 사용된다.  
가장 많이 사용되는 1 byte는 8 bit 이다.  

## java 의 데이터 자료형의 bit 수는? 
- byte : 8 bits, 2^7-1(양수+0), 2^7 (음수)
- short : 2 bytes = 8*2 bits = 16bits
- int : 4byte = 4*8 bits = 32 bits 이다.   
32 bits 로 표현 가능한 수는 2^32 이지만, 맨 앞의 비트는 부호를 표기하도록 하여 아래와 같은 범위의 수 해결할 수 있다.    
2^31-1 (양수 + 0) ~ 2^31 (음수) 
- long : 8 byte = 8*8 bits =64 bits 이다.  

결국 우리가 32 같은 10진수를 저장한다고 쳐도 컴퓨터한테는 우리가 선언한 데이터 자료형에 따라서 긴 bit 방에 2진수를 넣는것과 같다.  

그렇기 때문에 숫자를 통해서 bit 연산을 할 수 있다.

## Bit Operation 종류

### AND 연산 (기호 : &)
X & Y : X 와 Y 가 모두 1이어야 1, 아니면 0  

### OR 연산 (기호 : |)
X | Y : X , Y 둘 중에 1개만 1이 있으면 1, 둘다 0인 경우만 0 

### XOR 연산 (기호 : ^)
X^Y : 둘이 다르면 1, 같으면 0 

### NOT 연산 (기호 : ~)
~X : 모든 비트 반대로, 1 이면 0 , 0 이면 1  

### shift 연산 (기호 : 왼쪽으로 shift 시 <<  오른쪽으로 shift 시>>)
ex: 1<<3 -> 1000  
왼쪽 shift는 왼쪽으로 밀고 뒤에는 다 0으로 채우기에 큰 문제가 없지만,   
오른쪽 shift의 경우는 부호 파트인 첫번째 비트를 어떻게 할건지가 중요하다.   
" >>> " : 부호를 무시하고 그냥 밀기   
" >> " : 부호를 유지하고 밀기 이다.  



