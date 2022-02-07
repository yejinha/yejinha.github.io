---
layout: post
title: java 의 return문 위치
date:  2022-01-05 19:05
img: returnsea.jpeg # Add image post (optional)
categories: [study]
tags: [IT,  web, JAVA, return문, return문위치] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

너무 기본적인 것도 중간 중간 기록해 놓으려고 한다.  
자바를 사용하면서 함수 바깥으로 값을 보낼때 (반환할때) return문은 정말 질릴 정도로 사용했다고 생각했는데 오랜만에 간단한 문제를 풀다가 컴파일이 안되는 문제가 생겼다.  

# return문  
- 함수 안에서 사용한 값을 함수 바깥으로 보낼때 사용.   
- 리턴하고자 하는 데이터의 자료형을 메소드 이름 옆에 명시해 주어야한다.   
- 값을 리턴할 뿐 아니라 함수를 종료시키는 역할도 한다.   
- *반대로 리턴할 자료형을 메소드 이름 옆에 적어주었다면, 반드시 리턴문이 있어야한다 !* 


문제가 생겼던건 마지막 줄이었다.  
나는 간단한 문제를 풀면서 로직내 if 문들안에만 return 문을 적어놓은 것이다.   

~~~java
public static boolean example (int i){
    if (i > 0) {
        return true;
    }else {
        return false; 
    }
}
~~~

이렇게 해둔 것이다.  이 경우 if 문안에 조건이 안 걸리는경우가 있을 수 있기에 컴파일이 되지 않는다.   

이를 방지하기 위해선 두 가지 방법이 있다. 

1) return null ; 같은걸  맨 아래 삽입하여 컴파일 되도록 함.
2) 조건문 밖에 boolean result 같은 변수 만들고 그 변수값을 분기에 따라 조정후 리턴.   

나는 2 가 귀찮다고 저렇게 무지하게 쓴 것이당..   
제대로 하자 제대로,,  그래도 이렇게 기본 한번 잡고 간당 !! 