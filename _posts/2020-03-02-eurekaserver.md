---
layout: post
title: 실습 with spring boot  Netflix OSS 3- Eureka 
date:  2020-03-01 20:23
img: moomin.jpg # Add image post (optional)
categories: [msa]
tags: [IT, Spring cloud, NetflixOSS, Eureka, Hystrix] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

풀소스는 [여기](https://github.com/yaejinha/msa-SelfStudy) 에서 확인할 수 있다. 

이번 실습은 Eureka 설정이다.   
Eureka를 간단히 말하자면 연결된 서버들의 생사 체크도 해서 레지스트리로 관리하고  서버가 죽었으면 서비스에서 제외하는 load balancing 역할을 하는 거다.   
여기서 각 클라이언트들은 또 서버가 되서 다른 연결된 서버들을 클라이언트 삼을수도 있다. [링크](https://www.baeldung.com/spring-cloud-netflix-eureka)  

나는 여기서 유레카 서버 하나 만들고 클라이언트 역할 하나만들어서 등록이랑 해제만 해보겠다. 

## Eureka Server 

1) 일단 클라이언트들 생사체크를 하고 분산 처리를 할  서버 역할을 할 spring boot project 만든다.  여기에는 eureka-server 디펜던시를 넣어준다.  
```xml
<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
``` 

2) 서버 설정을 넣어준다.  
```properties
eureka.client.registerWithEureka = false   // 스스로도 클라이언트처럼 등록하여 사용할 수도 있는데, 여기서는 서버로 사용할 거니까 false 로 자기자신은 등록 안하도록 한다. 
eureka.client.fetchRegistry = false  // 클라이언트들이 레지스트리를 캐시로 사용할 수있도록 할지 선택
server.port = 8761  //유레카 서버 기본 포트 

```

3)@EnableEurekaServer  어노테이션으로 Eureka server 작동시킨다.  


## Eureka Client 

클라이언트는 유레카 서버에 서비스 등록하고 싶은 어플리케이션을 말한다.  

1.  eureka client 디펜던시 넣기.  
```xml
 <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
 </dependency>
 ```

 2. 설정 넣기. 

```properties
 eureka.client.serviceUrl.defaultZone  = http://localhost:8761/eureka  // 체크될 유레카 서버 서비스 url 넣는다. 여러개 일수 있음
 spring.application.name = eurekaclient  // 유레카 서버에 등록될 이름 
```

3. @EnableDiscoveryClient 어노테이션으로 등록 


여기까지 해서 두개 다 띄워보면 유레카 모니터링 화면에서 확인할 수 있다.  

![유레카 모니터링](/assets/img/eureka.png)  

그럼 등록까진 확인이 된다. 그냥 포트를 죽여서 서비스 해제 까지 보려고 했는데 해제가 잘 안된다.  
이건 내일 해봐야지. 좀 더 공식 문서도 보고 설정값도 좀 봐야겠다.   


---

Eureka client 레지스트리 등록 해제 실습  

어제 해보니까 클라이언트의 포트를 죽여서 아예 서비스를 내려도 eureka dashboad에서 상태가 UP 이었다. 
이걸 해결하기 위해서 아래와 같은 방법들을 썼다.  

1) spring boot actuator 의 healthcheck 이용하여 eureka 서버 등록 해제 

[여기] (https://cloud.spring.io/spring-cloud-netflix/multi/multi__service_discovery_eureka_clients.html) 에 보면 자세히 나와있다.  

> Unless specified otherwise, the Discovery Client does not propagate the current health check status of the application, per the Spring Boot Actuator. <span style="color: #B343CD">**Consequently, after successful registration, Eureka always announces that the application is in 'UP' state.** </span>This behavior can be altered by enabling Eureka health checks, which results in propagating application status to Eureka.

그래서 해답은 클라이언트 설정에 **eureka.client.healthcheck.enabled: true** 를 추가하는 것이다. 나는 클라이언트에 @EnableDiscoveryClient 이걸 넣으면 자동으로 체크해주는 알았더니, 현재 어플리케이션 클라이언트의 상태를 서버에 리프레시 안해줘서 정상 등록된 이후에는 서버에서 볼땐 클라이언트의 상태가 UP 에서 변하지 않았다.  
근데도 잘 이해가 안가서 조금 더 찾아봤다. 애초에 쓰는 이유가 자동으로 hearbeat 날려서 죽은 놈은 뺀다는거 아니었어...?  
[이 페이지](https://jmnarloch.wordpress.com/2015/09/02/spring-cloud-fixing-eureka-application-status/) 를 참고해보면 Spring boot 의 actuator 의 healthcheck를 유레카 서버에 반영하겠다는 뜻으로 하는 설정이다.  

>Additionally it turns out that <span style="color: #B343CD">**the default statuses defined by Actuator and Eureka matches each other perfectly**</span>, so everything that needs to be done is to determine current aggregated application status, map it to Eureka’s and return it.


2) 클라이언트 등록 해제 판단 기준 아주 짧게 하기  
 이것 저것 설정을 찾아보니, 클라이언트 등록 해제하는 기준때문일 수도 있겠다는 생각을 했다. "나 살아있어요~~" 하고  heartbeat을 보내는데, 일정 기간 동안 이걸 안보내면 서버 레지스트리에서 삭제한다.   
- 서버로 heartbeat 보내는 주기 : eureka.instance.lease-renewal-interval-in-seconds = 30  (기본 30초)
- 서버에서 일정 시간 이상 연락 없으면 클라이언트 죽었다고 판단하는 시간 : eureka.instance.lease-expiration-duration-in-seconds =90 (기본 90초)

그래서 eureka.instance.lease-renewal-interval-in-seconds =1  
eureka.instance.lease-expiration-duration-in-seconds =2 로 확 줄였다. 

일단은 혼자해보는거니까 확 줄였는데 [여기](https://stackoverflow.com/questions/46344005/how-does-eureka-lease-renewal-work) 나 여러군데 글을 읽으니 이 값을 조정하는건 기본 설정값을 디폴트로하고 짜여진 클라이언트 관리 주기 같은 것 때문에 비추라고 하네.   


일단은 여기까지 살펴봤고 실습은 했다. 아무래도 설정값도 더 공부해보고 여러 클라이언트 만들어보고 돌려봐야겠다. 








