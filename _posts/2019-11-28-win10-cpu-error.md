---
layout: post
title: WIN10 환경에서 갑자기 cpu 100 칠때 해결법 
date:  2019-11-28 16:36:00 +0800
img: pompom.jpg # Add image post (optional)
categories: [기타]
tags: [공부, win10, cpu점유율] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

사람살려,,,, 
출근해서 평화롭게 일하려고 하는데, 갑자기 컴퓨터가 버벅거리기 시작했다.  

바로 작업 관리자로 메모리 확인해보니까, cpu 100..???   
cpu 제일 많이 잡아먹고 있는 프로세스 보니까 가장 많이 잡아먹는건 system이었다..   
그걸 멈출 순 없잖아요 **:thinking:**

시도해본 해결 방안들 🐱‍👓

1. windows 색인 중지  
CPU 부하 줄이는 방법 블로그 글들마다 제일 처음으로 소개하는 방법이길래 따라했다.  
**실행 - services.msc >  windows search  기능 종료**  
나름 줄어들긴했는데 그래도 여전히 cpu 70~80 ㅠㅠ  

2. windows 구성 요소 저장소 파일 손상 확인  
찾아보니 win10 에서 드라이버 업데이트 과정에서 이런 상황 겪는 사용자들이 많았다.  
시스템 파일에 문제있는지 탐색하고 복구 진행하는 과정으로 해결.  
**1) cmd 창에  Dism /online /cleanup-image /restorehealth 입력**    
**2) 검사 완료 후  sfc /scannow 입력 !!**  
확실히 뭔가 문제가 있었는지, 복구 완료 메시지가 떴고, 재부팅하니 cpu 5 정도로 회복이 됐다 👏🎉👏🎉✨  

아니, ms 무슨 일배포를 8만건 한다는디 이렇게 불안정해도 되는건가 싶긴한데..   
그래도 행복하다면 ok 입니다.  👌

![happythenOk](http://yaejinha.github.io//assets/img/happyOK.jpg)