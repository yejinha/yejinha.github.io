---
layout: post
title:  spring JPA 사용해 세팅시 주의할 점 ! (spring.jpa.hibernate.ddl-auto)
date:  2022-01-27 18:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  web, JPA] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

플젝하다가 배운 거 정리하기 !!   
이번 플젝에서는 JPA 쓰기로 했는데,  회원가입 구현하다가 설정 관련해 주의해야할 부분을 배우게 됐다.  
역시 직접 해보면서 깨닫는게 직빵이다.  

# 1. 배경과 문제점 
spring boot + JPA 로 간단한 회원가입 구현하고 있었다.  
요청에서 넘어온 정보를 dto 에 매핑 시키고 그냥 save 만 하면 되는 간단한 기능이었는데 이상하게 자꾸 로컬에서 새로 기동할 때마다 이전 데이터들이 날아갔다.. ?   

# 2. 원인 
application.properties 에 넣는 JPA  설정 중에 spring.jpa.hibernate.ddl-auto 가 있다.  
ddl-auto 라는 이름에서 유추할 수 있듯이 테이블 create, drop 을 자동으로 해주는 설정이다.  
JPA 사용하여 db 초기화 하는 세팅임. 

spring.jpa.hibernate.ddl-auto = create 라고 하면 실행시마다 기존 테이블을 드랍하고 새로 만든다.   

즉 내 로컬에서 spring.jpa.hibernate.ddl-auto = create 였기 때문에 발생한 문제였다.  

아니 근데 내 로컬에서 발생했으니까 ㅎㅎ아하~ 하고 말지 만약 내가 일하면서 운영계에 잘못해서 프로퍼티 세팅을 저렇게 올렸으면... ? 아찔하네요,,,,    

# 3. spring.jpa.hibernate.ddl-auto 세팅 프로퍼티 정리  

**spring.jpa.hibernate.ddl-auto = create** :  hibernate 를 사용해서 db를 초기화하는 설정.  

**property** :   
+ Create  - 실행시마다 기존테이블 드랍하고 새로 만든다.   
+ Update - 변경 분만 업데이트   
+ Validate -  엔티티와 테이블이 정상 매핑되었는지만 확인  
+ None - 값 무시     
 
spring boot 에서 기본 값은  db 가 h2 같은 임베디드이면 create-drop 이 기본이고 외부 db 커넥션 해놓는다면 none 이 기본이다.   

[참고 - 스프링 공식 문서](https://docs.spring.io/spring-boot/docs/1.1.0.M1/reference/html/howto-database-initialization.html)


