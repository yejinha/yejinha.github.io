---
layout: post
title: 스프링을 구성하는 모듈 3- web module
date:  2019-08-18 21:36:00 +0800
img: three.jpg # Add image post (optional)
categories: [spring]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

# 모든걸 다 해주는 기가 막힌 프레임워크가 있다? 
![스프링 모듈 구조](http://yaejinha.github.io/assets/img/spring-overview.png)

Web module 에 대해서 공부해 봐야겠다 ^^ 

3. Web module 

스프링 문서에서는 무슨 웹 지향적인 프로그램을 도와준다는데 웹 지향적,,,? 무슨 소리지.   

사실 웹 의 개념 자체를 잘 모르겠다. 웹 이 뭐지??? 그냥 인터넷 창 열고 하는게 웹 인가요?   

**웹**  
> 간단히 인터넷에 연결된 사람끼리 정보를 주고 받을 수 있는 서비스
> 인터넷과 혼용되고 있지만 웹은 인터넷의 한 서비스일 뿐이다.
> 웹은 인터넷 상에서 텍스트나 그림, 소리, 영상 등과 같은 멀티미디어 정보를 하이퍼텍스트 방식으로 연결하여 제공합니다.
> 하이퍼텍스트(hypertext)란 문서 내부에 또 다른 문서로 연결되는 참조를 집어 넣음으로써 웹 상에 존재하는 여러 문서끼리 서로 참조할 수 있는 
> 기술을 의미합니다. 이때 문서 내부에서 또 다른 문서로 연결되는 참조를 하이퍼링크(hyperlink)라고 부릅니다.  [출처](http://tcpschool.com/webbasic/www)  


&nbsp;&nbsp;&nbsp;&nbsp;결론적으로는 머 웹서비스 위한 동작들은 해주는 모듈이군!  
스프링에서의 웹 모듈은 서블릿 리스너와 application context 통한 IoC 의 초기화를 담당한다고 한다.  

웹 서블릿 모듈은 MVC 모델의 구현에 사용되는데 진짜 애매하게 알고 있어서 이거 정리부터한다.   
점점 정성이 사라지는 글,,,,   


**서블릿**
이것도 역시 [갓블로그](https://mangkyu.tistory.com/14) 를 찾아서 봤다,,, 블로그 없으면 나는 블로그 못 써,,,,  

> 서블릿이란 자바를 사용해 웹을 만들기 위한 기술로, 클라이언트에서 요청이 오면 그에 대한 결과를 전송해주는 역할을 한다.  

이 서블릿이 작동하기 위해서는 

> 1) 사용자(클라이언트)가 URL을 클릭하면 HTTP Request를 Servlet Conatiner로 전송합니다.  
> 2) HTTP Request를 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 두 객체를 생성합니다.  
> 3) web.xml은 사용자가 요청한 URL을 분석하여 어느 서블릿에 대해 요청을 한 것인지 찾습니다.   
> 4) 해당 서블릿에서 service메소드를 호출한 후 클리아언트의 POST, GET여부에 따라 doGet() 또는 doPost()를 호출합니다.   
> 5) doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보냅니다.   
> 6) 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킵니다.   

의 과정을 거쳐야한다고 하는데, 이걸 다 소스단에서 구성해주기 너무 귀찮다 ㅠㅠㅠ  
MVC 패턴 관점에서 보면 서블릿은 컨트롤러 역할을 하는데, 이를 간단하게 구현 가능하도록 해주는게 웹 서블릿 모듈의 일이라고 이해했다.  

**MVC**   
&nbsp;&nbsp;&nbsp;&nbsp;빡이 친다. 너무 싫다. MVC 짜증나. 물론 처음 배울땐 걍 뚝딱 이해해서 걍 대충 눈치로 보고 베끼기해서 뚝딱 대충 끼워넣고 잘 되는척 했는데,  
이제 그렇게 살 수 없잖아요,, 더 이상의 날로 먹기 no,,,,   

아주 간단하게 MVC 에 대해서 말 해보자면, 
Model: 데이터의 틀이 되어주는 것 (VO 같은거!)  
View: 화면단에 뿌려주는거 (jsp 같은거 )     
Controller: 데이터 처리하는 로직 부분으로 서블릿의 역할로 view와 model 간의 징검다리가 되어준다.   

뭐 다 좋은말이긴 한데, 그래서 어떻게 동작하냐는거요,,,?  

Spring 에서는 DispatcherServlet  이 요청이 들어오면 해당 servlet 이 뭔지 찾아서 전달해주는 역할을 한다.  
1) 요청이 들어오면 요청 분석해 해당 서블릿에 요청 전달    
2) 서블릿(controller) 는 요청에 따라서 데이터 처리한다.  (HttpRequest 영역)  
3) 처리한 데이터를 model 의 형식으로 view 에 넘긴다.   (HttpResponse 영역)  
4) view 에서는 데이터를 잘 뿌려준다.  

이걸 해주기 위해서 스프링해줘야 할 일.  

원래 서블랫 매핑을 기술해 줬던 web.xml 에 dispatcherservlet 등록 ,  url 필터 등록 같은걸 해줘야한다.   
그리고 dispatcher 서블릿이 잘 알아볼 수 있도록 어노테이션으로 표기를 해주고, servlet-context.xml 에 componet-scan 범위를 넣어서  
어떤 컨트롤러들에 매핑시켜줘야하는지 해줘야한다.  

===================================================================================================================

그 이외에도 portlet 같은게 있는데 찾아보니까 자주쓰는 웹 컴포넌트들을 정의해놓고 재사용하는 거 같은데  
써본적이 없어서 잘 와닿지가 않음.  몰라  

