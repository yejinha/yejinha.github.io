---
layout: post
title: SSL offload 란..?
date:  2020-03-01 20:23
img: gorapaduck.png # Add image post (optional)
categories: [study]
tags: [IT, Network, ssloffload] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

요즘은 부하분산의 중요성을 많이 느낀다.  
물론 고리쩍부터 중요했겠지만 나한텐 이제야 와닿았잖아요..? 

이번에 네트워크 작업을 하면서   ssl offload  작업을 했다. 

ssl offload 는 단순히 말하자면 외부 -> L7 -> 내부 로 두 번의 연결과정에서 모두 ssl 인증 거치는데  발생한느 부하를  줄이고자 외부 -> L7 에서만 ssl 인증을 한다.  

![ssloffload](/assets/img/ssloffload.png)
[출처-](https://avinetworks.com/glossary/ssl-offload/) 

각각의 웹서버는 ssl 인증 과정을 수행하지 않고 그 과정을 앞단에서 맡아서 해주길 바라는 거다. 





