---
layout: post
title: clojure 로 hello world 찍기 
date:  2021-08-17 19:00
img: dongdong.JPG # Add image post (optional)
categories: [python]
tags: [IT,  clojure] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

어차피 쉬엄쉬엄 사는 하반기 재밌는거나 많이 해보자.
clojure 를 쓰는 회사도 있다고 들어서  호기심이 생겼다. 

그래서 그게 뭔데?  
함수형 프로그래밍? 나도 해보련다~~~

**clojure 란?**
리스프 프로그래밍 언어의 한 종류로 JVM 상에서 돌아가는 장점을 가진 함수형 프로그래밍 언어이다. 

간단 소개긴한데 모르는 개념이 너무 많다.   
일단 기본적으로 난 리스프를 안 써봤고, 함수형 프로그래밍도 잘 모른다.  
일단 부딪혀보기 

일단 오늘은 clojure 로  hello world 찍는 법 ! 

**1. clojure 설치**  
mac OS 인 경우 쉽게 설치 가능하다. 
~~~
brew install clojure/tools/clojure
~~~

[참고](https://clojure.org/guides/getting_started)

2. Hello world 찍기 

~~~clojure
(ns clojure.examples.hello  // 네임스페이스 지정 
  (:gen-class))
(defn hello-world []  
  (println "Hello World"))  // hello world 찍기
(hello-world)  
~~~


일단 이렇게  간단히 프린트만 하는 걸 짜고 실행해봤다.  

실행은 뭐 복잡한거 깔 필요없이 커맨드 창에서  해당 .clj 파일 있는 경로에서 
clj 파일이름.clj  하면  실행된다.  

이걸로 간단한 웹 백엔드를 짜보려고 한다.  많이 어려우려나?  

일단  **가보자고** 

