---
layout: post
title: L4 와 L7 차이점
date:  2019-12-01 14:24:00 +0900
img: network.jpg # Add image post (optional)
categories: [study]
tags: [공부, 네트워크, l4,l7] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

네트워크 관련된 문제를 마주할때마다 늘 나오는 단어들이 있다.  

L7, L4 ! 솔직히 어렴풋이는 알지만 너무 헷갈려. 이해했다가도 돌아서면 까먹는다.  
그래서  정리를 한번 해봐야지.  🤠  

# **:baby:** L4, L7 이 뭔데?   

&nbsp;&nbsp;&nbsp;&nbsp;L 은  loadbalancer 를 뜻한다.   
서버의 부하를 분산시키기위해서 여러대의 서버를 두고 그 서버들한테 어떻게 연결시켜줄지 관리해주는 역할을 말한다.  그렇다면 뒤에 붙은 숫자는 뭘까?  
일단은 이걸 이해하기 전에, OSI 7계층을 먼저 봐야한다.  
저 뒤의 숫자 4, 7 이런건 7계층 중 어느 단계인가를 나타낸다.  

![OSI7계층](http://yaejinha.github.io//assets/img/osi.png)  

## **:relaxed:** L4, L7 은 부하분산해주는 시점이 다른 거구나  

## L4 : trasport layer 단에서 조정 > IP +port 를 보고 조정 (TCP/UDP 프로토콜)
    여기서 분산처리 위해 VIP (virtual IP) 사용한다. 일단 실제 서버로의 요청을 VIP 로 보내면 안에서 내부 로직으로 분산 처리해준다.   

## L7 : application layer 단에서 조정 > 최상단이기에 ip+port+페이로드까지 전부 볼 수 있다.  (HTTP 단까지)

&nbsp;&nbsp;&nbsp;&nbsp;L7 은 L4 보다 상단 영역이기에 L4 의 역할도 수행할 수 있다. 그리고  L7 은 페이로드에서 URL 까지 볼 수 있기에 조금 더 상세한 분산처리가능하다.   

로드밸런서의 알고리즘 종류는 자세히 정리된 다른 [블로그](https://medium.com/@pakss328/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05) 참조 ! ! 









