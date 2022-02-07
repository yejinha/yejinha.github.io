---
layout: post
title:  HTML 에 대하여
date:  2022-01-23 16:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  web, git] # add tag
sitemap :
changefreq : daily
priority : 1.0
---
온보딩 커리큘럼에 올린거 블로그에도 올린다.    
![온보딩 커리큘럼](https://github.com/Knowre-Dev/WebDevCurriculum)


### *1. HTML 표준의 역사는 어떻게 될까요?*   

 html 은 HyperText Markup Language 의 약어로  1991년 처음 발표 됐다. 
 텍스트, 이미지 등 웹 페이지에 표시되는 데이터들을 태그 형식으로 구조적 형태로 나타낸다. 
 1997 년 HTML3.2 가 W3C 권고안으로 표준으로 발표됨. w3c 가 의사결정이 느리자 Apple, Mozilla, Opera는 공동으로 WHATWG(Web Hypertext Application Technology Working Group)라는 그룹을 만들어 w3c와 함께 HTML5 를 제정했다. 

### *2.HTML 표준을 지키는 것은 왜 중요할까요?*   
  웹은 누가 어떤 플랫폼으로 (어떤 브라우저로) 사용할지 특정할 수 없기에 표준을 지키지 않으면 누가 접근하더라도 동일한 내용으로 출력되어야하는 원칙이 위반되고 누구나 원할하게 웹 페이지를 접근할 수 있어야한다는 웹 접근성이 훼손되기에 지켜야한다. 

### *3.XHTML 2.0은 왜 세상에 나오지 못하게 되었을까요?*   
XML과 HTML을 합성하여 더 확장된 표현을 위해 XHTML 이 나왔다.  하지만, 하위 호환성을 지원하지 않아서 이전의 태그들로 작성된 것들이 사용되지 않을 수 있다는 문제점이 있었고, 사용하기 어렵다는 이유로 사용자들이 점점 줄어들면서 사양으로 접어들었다. 

### *4.HTML5 표준은 어떤 과정을 통해 정해질까요?*   
  W3C에서 정해져 공표된다 .

### *5.브라우저의 역사는 어떻게 될까요?*  

웹 브라우저란 웹 상에 있는 페이지들의 HTML 언어를 해석해서 내용을 화면에 정리하여 보여주는 프로그램이다.   
웹 브라우저란 월드 와이드 웹(www)에서 정보를 검색,표현하고 탐색하기 위한 소트프웨어이다.   
브라우저는 인터넷에서 특정 정보로 이동할수 있는 주소입력창(인터페이스)를 포함, 서버와 HTTP로 정보를 받을 수있는 네트워크 모듈도 포함한다.    
1989년 영국에서 처음 만들어졌으며, 1990년대 넷스케이프와 IE 양강구조로 발전됐다.    
마이크로 소프트 운영체제에 이 소프트웨어를 기본 탑재하면서 시장 점유율이 급격히 올라갔다.    
그러나 MS 제품이 아닌 곳에서는 브라우저 지원을 중단하고  대체 브라우저들이 많이 생겨나면서  시장 점유율이 낮아졌다.    

### *6. Internet Explorer가 브라우저 시장을 독점하면서 어떤 문제가 일어났고, 이 문제는 어떻게 해결되었을까요?*  
독점시장이기에 기술의 발전이 일어나지 않았다.  마이크로 소프트에서는 웹브라우저 개발팀을 해체하기까지했었다.  그러나 다른 브라우저의 점유율이 올라오니 위기감을 느낀 마이크로소프트는 다시 IE 의 새 버전을 내놓았다. 

### *7.현재 시점에 브라우저별 점유율은 어떻게 될까요? 이 브라우저별 점유율을 알아보는 것은 왜 중요할까요?*   
  크롬 - 65 %, 사파리 20 %, 엣지 4 % 파이어폭스 3 % , 기타 등등이다. (2021 기준)
  (https://en.wikipedia.org/wiki/Usage_share_of_web_browsers)
  
  개발을 할때 사용자 호환성이 가장 좋게 하기 위해서 어느 브라우저에 최적화를 해야할 지 판단하기 위함이다. 

 ### *8. 브라우저 엔진(렌더링 엔진)이란 무엇일까요? 어떤 브라우저들이 어떤 엔진을 쓸까요?* 
  사용자가 요청한 콘텐츠를 화면에 표시해주는 것이다. HTML, CSS 등을 파싱하여 그린다. 
  서버에게서 전달받은 HTML 을 파싱하여 DOM 객체로 만들고, 외부 CSS 등을 파싱하여 스타일을 구축한다.  이를 합쳐 렌더 트리를 생성하고 화면상에 어디에 놓을지 정하고 그린다. 
 
  크롬: Blink      
  사파리: Webkit   
  파이어폭스:Gecko  
  엣지 : EdgeHTML (MS 자체개발 렌더링 엔진)

 ### *9.모바일 시대 이후, 최근에 출시된 브라우저들은 어떤 특징을 가지고 있을까요?* 
모바일에서도 잘 돌아가야하기에 가볍고 작은 화면에서 효율적으로 웹 콘텐츠를 보여주는데 최적화되어있다. 

### *10.HTML 문서는 어떤 구조로 이루어져 있나요?*  
HEAD: HTML문서의 정보가 담겨 있다. 타이틀이나 HTML 버전, 메타 데이터 등등   
BODY: 웹 페이지에 그리고자 하는 콘텐츠가 담겨 있다. 

~~~html 
<!DOCTYPE html>
  
<html>
    <head>
        <title>
              
        </title>
    </head>
      
    <body>
          
    </body>
</html>
~~~
!DOCTYPE html 태그 : HTML 버전을 말하며 HTML5 를 뜻한다.    
html 태그: html 루트 로 코드를 감싸주어야한다.     

  ### *`<head>`에 자주 들어가는 엘리먼트들은 어떤 것이 있고, 어떤 역할을 할까요?*   

style 태그 :  css 같은 스타일을 입히는데 사용된다.  태그를 특성하고 그 태그의 스타일을 선언한다.

~~~html 
 <style>
        body{
        background-color: #616A6B;
        }
        h1{
        font-family: commanders;
        background-color: yellow;
        }
        h2{
        font-family: algerian; 
        background-color: cyan;
        }
        #first{
        font-family: Castellar; 
        background-color: green;
        color: blue;
        }
        .second{
        text-align: right;
        background-color: white;
        font-size: 30px;
        color: red;
        }
    </style>
~~~

title 태그 : html 문서의 제목을 나타낸다.   

base 태그: URI 나 URL  을 선언한다. 페이지의 모든 링크의 베이스가 되는 URL 을 선언한다.  
~~~html
  <base
      href="https://media.geeksforgeeks.org/wp-content/uploads/"
      target="_blank"
    />
~~~
이런 식임. 

script 태그 :  클라이언트 사이드 스크립트 를 넣는다.  
   스크립트 자체를 써넣을 수 있고  외부 스크립트 파일을 첨부하는데에 사용할 수 도 있다.   
   주로 컨텐츠의 동적 변경을 위해 자바스크립트를 이 곳에 사용한다. 

meta 태그: 문서의 정보에 대해서 기술한다.    

attribute 에는     
name: 프로퍼티 이름 정의할 때 사용   
http-equiv: http 응답 헤더 사용할 때.   
content:  프로퍼티 값 채울 때 
가 있다.    
~~~html
<meta name="viewport" 
          content="width=device-width,
                   initial-scale=1,
                   maximum-scale=1">

~~~~

link 태그 : html 문서와 외부 리소스 연결할 때 사용한다.  주로 외부 css 등을 링크할 때 사용한다. 

(https://www.geeksforgeeks.org/html-link-tag/)

### *11. 시맨틱 태그는 무엇일까요?*
  이름만으로도 브라우저나 개발자가 어떤 역할을 하는 태그인지 알 수 있는 태그들을 뜻한다.   
  
  article ,aside, footer, header, nav, section 등이 있다.    
   section : 어떤 공통된 테마를 가진 컨텐츠의 묶음이다.  
   article : 독립적이고 개별적인 컨텐츠 단위   
   header  : 컨텐츠 소개와 링크들   
   footer : 저작권 정보 등 푸터에 들어가야할 정보들   
   nav   :  내비게이션 링크들의 묶음을 감싼다.   
  aside :  주변 정보들과 관계가 좀 떨어지는  정보들을 묶는다. (사이드바 같은)
  
  ![시맨틱태그](/assets/img/semantictag.png)

### *12. 시맨틱 엘리먼트를 사용하면 어떤 점이 좋을까요?*

시각장애가 있는 사용자가 스크린 리더를 사용하여 페이지를 탐색할 때도 도움이 되고 검색엔진이 태그 기반으로 페이지 내 키워드 우선 순위 판단하기에 검색 엔진 최적화에도 중요하다!! 
그리고 div 연속보다 코드 가독성이 좋다.  


### *13.  `<section>`과 `<div>`, `<header>`, `<footer>`, `<article>` 엘리먼트의 차이점은 무엇인가요?* 

section>태그는 논리적으로 관계있는 컨텐츠들을 감싸는 태그이다. 이 안에 div, header, footer, article 들어갈 수 있다. 물론 article안에 섹션이 들어갈 수 있기는 하지만,  section은 논리적으로 연관있는 콘텐츠의 묶음이고 article은 개별 정보이기에  section으로 article을 감싸는게 더 자연스럽다.    

### *14. 블록 레벨 엘리먼트와 인라인 엘리먼트는 어떤 차이가 있을까요?*

  블록 레벨 엘리먼트는 항상 새 라인에서 시작하고, 페이지의 가로를 통째로 잡아먹는다. 
  h1~h6 이런 헤딩 태그들이나  ol.ul 의 리스트들, div가 속한다. 

  인라인 엘리먼트는 페이지 가로를 꽉 채우지 않고 오픈 태그와 클로즈 태그 사이에서 정의한 만큼의 공간만 차지한다. a em img span이런 태그들 !      


https://codeburst.io/block-level-and-inline-elements-the-difference-between-div-and-span-2f8502c1f95b