---
layout: post
title: 스프링을 구성하는 모듈 1- Core Container
date:  2019-08-17 21:36:00 +0800
img: one.jpg # Add image post (optional)
categories: [spring]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---
# 스프링 프레임워크
&nbsp;&nbsp;&nbsp;&nbsp;POJO 를 사용하여 어플리케이션을 만들고,  엔터프라이즈 서비스를 비침투적으로 적용할 수 있도록 해주는 자바 플랫폼이다.

&nbsp;&nbsp;&nbsp;&nbsp;이렇게 말하니까 도대체 무슨 말인지 하나도 모르겠다.  하나씩 뜯어보자. 

&nbsp;&nbsp;&nbsp;&nbsp;즉 사용자가 어플리케이션에만 집중할 수 있게  인프라를 다뤄주는데,  어플리케이션도 POJO ( getter 와 setter 로만 이루어진 클래스 구조 > 프레임워크 의존적이지 않음) 으로 구성할 수 있도록 해서  쓸데없는 일을 안하고 자기들끼리 지지고 볶겠다는 거 아닌가? 

&nbsp;&nbsp;&nbsp;&nbsp;엔터프라이스 서비스에 비침투적으로 적용한다? 엔터프라이즈 서비스는 내가 보니까 여러 서비스를 하나로 묶어주는  구조를 엔터프라이즈 서비스 라고 하는 것 같다.

&nbsp;&nbsp;&nbsp;&nbsp;결국은 여러 서비스를 하는 커다란 엔터프라이즈 레벨 서비스 구현에서도 , 개발자는 이걸 어떻게 연결할지 고민할 필요 없이 POJO 를 사용해서 핵심 로직 구현에만 힘쓰면 된다는 말이지?  

# 모든걸 다 해주는 기가 막힌 프레임워크가 있다? 
![스프링 모듈 구조](http://yaejinha.github.io/assets/img/spring-overview.png)

이제 스프링의 구조를 좀 봐야겠다.  어떤 구조이길래 자기가 다 알아서 한다는거지? 

1. Core Container  
Core, Beans, Context,  Expression Language 모듈이 있다.  
- Core / Beans 모듈 : 프레임워크의 핵심에 해당하는 것으로 DI 할때 제 역할을 한다.BeanFactory 는 팩토리 패턴으로 구성되어 있고 싱글톤 패턴을 직접 구현할 필요가 없고, 의존성 설정과 구현 부분을 분리하는 역할을 한다.  

- Context 모듈 : JNDI 와 비슷한 방식으로 프레임워크 안에서 동작한다.  
&nbsp;&nbsp;&nbsp;&nbsp;엥? 이게 무슨 소리냐...? JNDI 도 뭔지 잘 모르는데 밑도 끝도 없이 무슨 JNDI 와 비슷한 방식입니까? 
JDNI 에 대해서는 [이 포스트]() 참고 !!  
즉 다양한 리소스들을 가져다 쓸 때 참조하기 위한 모듈이라는 것이다. beans 모듈 특성들을  가지고 있지만 리소스 로딩 이나 EJB, JMX 같은 Java EE 의 기능도 가지고 있다. ApplicationContext 인터페이스 를 사용한다.     

** **BeanFactory 와 ApplicationContext 차이**  
기본적으로 ApplicationContext 는 BeanFactory 인터페이스 의 기능을 모두 가지고 있고 플러스 알파 기능이 있는 것이다.  주된 차이점은 다음과 같다.  
>  BeanFactory 는 getBean 메소드 호출시에 bean을 초기화하는데, applicationfactory 는 스프링 컨테이너 구동시에 초기화한다.  
>  ApplicationContext 는 어노테이션 기반 설정 처리 가능하다. (@Autowired 같은거)  
>  ApplicationContext 는  여러 설정파일 적용 가능하다.  
>  ApplicationContex 는 i18N 처리 가능하다.  (다국어 처리)

- Expression Language 모듈 :  객체그래프 ( 객체 간의 참조 관계를 나타냄)을 표현하거나 쿼리를 표현하는 언어를 다루는 모듈이다. 속성값 설정이나 오퍼레이터, 설정상의 이름으로 컨테이너가  정보 찾도록 도와주는 역할을 한다.  

와 아직 모듈 하나 했네 ^^ 어렵다 어려버ㅠㅇ ㅠ

글이 길어져서 모듈별로 나눠서 올려야겠다 !!  