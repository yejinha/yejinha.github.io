---
layout: post
title: MAC 에서 포트 죽이기 ! 
date:  2020-02-29 19:00
img: gorapaduck.png # Add image post (optional)
categories: [기타]
tags: [IT, MAC, MACBOOK, 맥북 , 포트, pid] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

내 인생 최초로 맥북을 사서 사용하려니까 이것저것 외워야 할 단축키며 명령어가 너무 많다 ^^   
나 요즘 완전 인간 고라파덕 ^^....  


너무 간단하지만 몰라서 찾아봤던것중에 하나는 포트 죽이기다.  
스프링 부트 실습하는데 자꾸 포트 사용중이라고 하니까..ㅎㅎ..   

커맨드창에서 
1) lsof -i : 포트번호  
2) kill -9 pid 번호 하면 된다.   

![포트죽이기](/assets/img/portkill.png)  

