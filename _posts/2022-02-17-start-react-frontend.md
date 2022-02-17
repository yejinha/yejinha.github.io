---
layout: post
title:  react 시작하기 그리고 싱글페이지 어플리케이션
date:  2022-02-17 15:05
img:  # Add image post (optional)
categories: [react]
tags: [IT,  react] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

팀 프로젝트에서 프론트엔드를 리액트로 구성하고자 해서 로컬에서 띄워보는 연습하고 있다.  
참고한 내용은 https://sundries-in-myidea.tistory.com/71 입니다.  

# 1. 리액트란?  
자바스크립트의 라이브러리 중 하나로 프론트엔드 라이브러리이다. 
이는 싱글페이지 어플리케이션을 개발하는데 유용하여 인기인데,,, 그래서 싱글페이지 어플리케이션이 뭐요...?     

# 2. 싱글페이지 어플리케이션 (SPA)  
정적인 리소스는 한번에 읽어와 렌더링 해놓고, 사용자 요청에 따라서 필요한 부분만 동적으로 다시 부분 렌더링 하는 방식의 웹 어플리케이션을 말한다.   

기존 방식에선 동적인 리소스 필요할 때마다 전체 페이지가 리로드 되었는데, 이를 줄여 트래픽 효율적이다.  
하지만 처음 구동시 정적 리소스를 모두 다운받기에 구동이 느리다는 단점이 있다.  

[참고-싱글 페이지 애플리케이션(Single Page Application, SPA) 개념](https://velog.io/@wiostz98kr/TIL48-l-%EC%8B%B1%EA%B8%80-%ED%8E%98%EC%9D%B4%EC%A7%80-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98Single-Page-Application-SPA-%EA%B0%9C%EB%85%90-z1zkrzr9)

[참고2- [React] 프론트 엔드와 백 엔드 분리 시 동작 원리 (vs 풀 스택)](https://it-eldorado.tistory.com/85)

# 3. 설치 및 실행
 -  node, npm  설치
 -  프로젝트 폴더 만들고 싶은 위치에서 npx create-react-app 프로젝트명  (ex:  npx create-react-app reactex)  *이때 프로젝트명 대문자 없이 소문자로 기입*  
 -  프로젝트 폴더에서 npm start  로 서버 기동  -> 기본 포트 3000 으로 서버 기동됨.   

# 4. 백엔드(API) 와의 통신 !   
다른 서버로 포트 다르게 뜨다보니,  그냥 localhost:포트번호 해서 호출하면 CORS 오류가 난다.  
*CORS 오류란?*  다른 origin 에서 오는 자원을 차단하는 것으로,  여기서는 본인의 origin 은 localhost:3000인데, 포트가 다른 백엔드 서버를 호출해 거기서 온 응답을 사용하려고 하니 발생한다.   
![예시](/assets/img/CORS.png)

따라서 package.json에 프록시 처리를 해주어야한다.  
~~~json 
"proxy": "http://localhost:8080",  
~~~

이런식으로 !! 

fetch 해서 자세히 쓰는 동작은 내가 리액트를 잘 몰라서 아직 다 이해한게 아니라 여기에 붙이진 않겠습니당 !!  

