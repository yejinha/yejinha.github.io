---
layout: post
title:  spring framework - @Controller @RestController 차이점 
date:  2022-02-28 18:05
img:  # Add image post (optional)
categories: [spring]
tags: [IT,  java, spring] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

개인 프로젝트를 만들다가 정리하는 spring annotation 중 @Controller 

# 0. 어노테이션 사용하는 이유 
스프릥 프레임워크의 특징 중 하나인 IoC 를 하기 위해선 프레임워크가 알아서 인스턴스를 생성하고 적재적소에 사용할 수 있어야한다.  
BeanFactory, ApplicationContext 인터페이스가 이런 일 하는데, 이 인터페이스들이 어떤게 bean 으로 만들어야할 것들인지 알기 위해 사용한다. 
이전에는 xml 사용한 주입 등이 있었는데 이 구조는 [이 블로그](https://jjingho.tistory.com/5) 참고.   

# 1. @Controller 어노테이션 
전통적으로 Spring MVC 에서 사용된 어노테이션.  @Component 라는 스프링 빈 등록 어노테이션을 구체화 한 용도로 해당 클래스가 컨트롤러 역할을 함을 알린다. 
Spring MVC는 서블릿 위에 구축된 프레임워크로 요청이 들어오면 DispatcherServlet에 매핑해 맞는 핸들러를 찾아 호출한다.  
(* 핸들러란, @Controller 클래스와 함께 사용되는 request mapping 메소드들 )

주 목적이 view 를 리턴하는게 목적이며, Json 형태의 리턴을 하기 위해서는 핸들러에 @ResponseBody 어노테이션과 함께 사용한다.  

# 2. @RestController 어노테이션 
@Controller + @ResponseBody 역할이다. 
Spring 4 부터 제공하며  핸들러인 @RequestMapping 메서드가 기본적으로 @ResponseBody 의미를 가정해 view 대신 데이터를 리턴한다.  