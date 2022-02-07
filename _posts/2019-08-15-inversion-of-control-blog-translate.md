---
layout: post
title: Inversion of Control Containers and the Dependency Injection pattern
date:  2019-08-15 21:36:00 +0800
img: text.jpg # Add image post (optional)
categories: [spring]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

&nbsp;&nbsp;&nbsp;&nbsp;스프링 문서를 읽다가 IoC, DI 개념에 대한 [추천 기사](http://martinfowler.com/articles/injection.html) 를 읽었다.   
공부할겸 번역 해보았다. ^^ 물론 의역 오역 천지지만 한글이 좋자나요?  

-----
** Inversion of Control 과 Dependency Injection 은 번역 안함 (제어역전, 의존주입 > 말이 너무 어려워!)

&nbsp;&nbsp;&nbsp;&nbsp;자바 커뮤니티에서는 다른 프로젝트들에서  컴포넌트들을 연결해 결합력있는 어플리케이션으로 만들 경량 컨테이너들이 쏟아져나오고 있다.  
이런 컨테이너들이 컴포넌트들을 연결하는 방법은 주로  "Inversion of Control" 이라는  이름의 컨셉을 따랐다. 이 기사에서는 Dependency Injection 이라는 이름하의 패턴들이 다른 서비스 로케이터들과 다르게 작동하는지에 대해서 쓰겠다.  실제 구성 원리를 아는 것이 사용할 여러 방법론 옵션 중에 고르는 것보다 중요하다.


&nbsp;&nbsp;&nbsp;&nbsp;자바 엔터프라이즈 세계에서 흥미로운 건 J2EE 기술 메인스트팀의 제안을 제시하는 오픈소스 기반의 움직임들이다. 이 중 대다수는 J2EE 메인스트림의 복잡하고 덩치큰 기술들에 대한 반발이기도 하지만, 기발한 아이디어들을 제시하는 대안들이다. 주로 다뤄지는 이슈는  어떻게 다른 요소들을 연결하느냐 : 거의 서로 독립적으로 구성된 데이터베이스 인터페이스 구조를 가진 웹 컨트롤러를 연결하는것 이다. 여러 프레임워크들은 이 문제를 해결하고자 해왔고 component 들을 연결하는데 가능성을 보였다. 이런 프레임워크들은 (picoContainer,Spring 같은 ) 경량 컨테이너라고 불린다.  

# Components 와  Services
&nbsp;&nbsp;&nbsp;&nbsp;The topic of wiring elements together drags me almost immediately into the knotty terminology problems that surround the terms service and component. You find long and contradictory articles on the definition of these things with ease. For my purposes here are my current uses of these overloaded terms.

&nbsp;&nbsp;&nbsp;&nbsp;I use component to mean a glob of software that's intended to be used, without change, by an application that is out of the control of the writers of the component. By 'without change' I mean that the using application doesn't change the source code of the components, although they may alter the component's behavior by extending it in ways allowed by the component writers.

&nbsp;&nbsp;&nbsp;&nbsp;A service is similar to a component in that it's used by foreign applications. The main difference is that I expect a component to be used locally (think jar file, assembly, dll, or a source import). A service will be used remotely through some remote interface, either synchronous or asynchronous (eg web service, messaging system, RPC, or socket.)

&nbsp;&nbsp;&nbsp;&nbsp;I mostly use service in this article, but much of the same logic can be applied to local components too. Indeed often you need some kind of local component framework to easily access a remote service. But writing "component or service" is tiring to read and write, and services are much more fashionable at the moment.


# Inversion of Control


&nbsp;&nbsp;&nbsp;&nbsp;이런 컨테이너들이 'Inversion of control' 을 사용하기 때문에 스스로가 유용하다고 말할때, 나는 혼란스럽다. Inversion of Control 은 
 프레임워크의 흔한 특징이기에, IoC 를 사용해서 경량 프레임워크가 특별하다고 하는 것은 내 차가 바퀴가 있어서 특별하다고 말하는 것 과 다름없다.  

&nbsp;&nbsp;&nbsp;&nbsp;진짜 질문은 "제어의 어떤 면을 뒤바꿨는가?" 이다. 내가 처음  IoC 를 접했을 때,  IoC 가 역전한 건 유저 인터페이스였다. 초창기 유저 인터페이스는 어플리케이션에
 의해 제어됐다. 프롬프트를 통해 "이름을 입력하세요. " , "주소를 입력하세요" 같은 명령에 따라서 입력하면,  각각에 맞는 결과를 뽑아냈다. 그래픽적인 UI 와 UI 프레임워크는 다양한 화면에 맞는 이벤트 핸들러 대신, 이런 작업을 하는 메인 루프와 프로그램을 가지고 있었다. 메인 프로그램의 제어가 프레임워크로 역전된 것이다.  

&nbsp;&nbsp;&nbsp;&nbsp;컨테이너들의 역전의 새로운 바람은  플러그인 구현을 어떻게 찾느냐에 있다. (즉 구현을 어떻게 반영시키느냐)  내 단순한 예제에서는 lister 가 바로 인스턴스 생성하여 finder 구현을 연결한다.
 이건 finder 가 플러그인화 되는 것을 막는다. 이러한 컨테이너가 사용하는 접근방식은 별도의 모듈이 lister 에 구현을 주입할 수 있도록 하는 어떤 규칙을 사용자가 따르도록 하는 것이다. ( 이부분 번역이 너무 애매한데, 요약하자면 IoC 를 특징으로 하는 컨테이너들이 제어해주는건 lister 가 어떻게 구현을 주입할 수 있는지 자기들만의 법칙을 만들고 사용자는 이를 지키도록 한다. 이를통해 사용자는 프레임워크 규칙만 따르면 자기들이 알아서 구현을 넣어주는 역할을 한다는 것. 진심 문장 너무 어렵네;; ㅠ )  

 이 결과로, 나는 이 패턴에 대해 조금 더 자세한? 상세한 이름이 있어야 한다고 생각한다. Inversion of Control 은 너무 포괄적인 단어이기에 사람들이 혼란스러워 하는 것 같다. 여러 논의끝에 우리는 Dependency Injection 이라는 단어에 정착했다.   

&nbsp;&nbsp;&nbsp;&nbsp;I의 여러 방법들에 대해 언급할텐데, 나는 어플리케이션 클래스에서 플러그인으로 의존성을 제거하는 것만이 방법이 아니라는 것을 설명할 것이다. Service locator 라는 다른 패턴으로도 할 수 있는데 이건 DI 설명을 끝내고 덧붙이겠다. 
