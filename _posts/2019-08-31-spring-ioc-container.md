---
layout: post
title: Spring IoC container
date:  2019-08-31 21:36:00 +0800
img: wth.jpg # Add image post (optional)
categories: [spring]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

**인생 쉽지 않다**

오늘은 Spring Framework - IoC container 에 대해서 적어보겠다.   

사실 그냥 문서 읽고 요약에 가깝겠지만 정리하는 김에 적는다. ㅠㅠㅠ   

--------------------------

&nbsp;&nbsp;&nbsp;&nbsp;spring IoC container 의 기본은 org.springframework.beans, org.springframework.context 패키지 이다.  여기서 beanfactory 인터페이스를 기본으로 하여 application context 인터페이스가 구현된다. beanfactory 인터페이스는 프레임워크 설정과 구동의 기본 기능을 담당하고, application context 는 다른 레이어의 기능들을 이용하기 쉽도록 한 superset 과도 같다.    

**IoC 컨테이너 간략 소개**


&nbsp;&nbsp;&nbsp;&nbsp;IoC 컨테이너는 객체들을 인스턴스화 하고, 설정하고, 조립하는 일들을 하는데 이에 대한 정보들은 xml, java 어노테이션, 코드들로 이루어진 설정메타데이터를 보고 한다. 따라서 Application context 가 생성되고 초기화되면 어플리케이션/시스템에 대한 설정이 완료된 것이다.  

&nbsp;&nbsp;&nbsp;&nbsp;설정 메타데이터들은 원래 xml 형식으로 되어있었는데, 스프링 3.0 이상에서는 빈들의 설정을 @어노테이션으로 해결한다. xml 베이스의 설정 메타 데이터는 `<bean>` 태그로 객체들 정의 나타낸다. 각 `<bean>` 태그에는 id 로 구분을 하고 class 속성으로 impl 가 어떤 내용인지 표현해준다.  

&nbsp;&nbsp;&nbsp;&nbsp;IoC 컨테이너를 구동하기 위해서는 실제 설정 메타데이터가 어디 있는지를 알 수 있도록 명시해주어야 한다. 여러 메타데이터를 한번에 사용하기 위하여 `<import>` 태그를 사용하여 한 메타 데이터 설정파일에 다른 메타 데이터들을 얹어 놓기도 한다. 컨테이너의 가장 큰 역할은 시스템을 구성하는 객체 (beans) 를 조립해주는 것이기에 getBean 이라는 메소드를 통해 명시적으로 구동 시킬 수 있지만, 이상적인 작동방식은 getBean 없이 스스로 알아서 주입시키는 것이다.   

**bean이란...?**  
&nbsp;&nbsp;&nbsp;&nbsp;bean들은 컨테이너에 메타 데이터를 제공해서 만들어진다. 컨테이너에서 참고하는 이런 정보들은 impl된 클래스, 행동 범위나 lifecycle 같은 설정, 다른 객체와의 의존성 관계 등등을 가져야 한다.  bean을 인스턴스화 (실제 객체로 생성하는 방법)은 그냥 bean 설정을 쓰면 생성자로 알아서 생성해주는 방법도 있지만, static인 경우나 factory 패턴의 메소드를 이용하는 방법도 있다. 

질문 >  bean 인스턴스화 하는 순서가 정확히 어떻게 되는거지?
       static은 왜 autowired 되지 않는 0걸까. : 인스턴스 생성 전에  static 은 만들어지니까 설정 메타 데이터 읽고 bean 만드는 과정은 그 후에 만들어져서 안되는듯 static0 이 문제 ㅠㅠ : 시스템 구동시에 constant 영역에 할당됨 그 후에 구동하면서 컨테이너가 bean 만들고 어쩌고 하는거라서 static 안됨 

**Dependencies** 

&nbsp;&nbsp;&nbsp;&nbsp;DI 는 보통의 객체간의 의존관계 방식을 넣어주는 걸 말한다. 순서는 다음과 같다.  
1) 객체들은 의존관계를 지정한다.  
2) 컨테이너가 객체 bean 이 실제 생성될때, 의존 관계에 있는 것들을 넣어서 조립해준다.  

&nbsp;&nbsp;&nbsp;&nbsp;이 과정을 컨테이너가 알아서 하기에 inversion of control 이라는 용어를 쓰는 것이다.  
bean 자체들이 인스턴스화되고, 어디에 들어가야 할지 알고 들어가게 되는 거다.   

이를 통해서 테스트 하기에도 쉽고 코드를 분리해내기에도 쉬워진다.   

&nbsp;&nbsp;&nbsp;&nbsp;DI 방법에는 생성자 DI, setter DI 가 있다.  
뭐 당연히 이름처럼 생성자를 통해서 해당 객체 내에서 필요한(의존관계가 있는) 다른 객체를 넣어주는 거나, 아니면 setter 로 넣어주는 거다.  다른 객체를 bean 형식으로 넣어주는거 말고도, 123 같은 속성 값 자체를 넣어줄때는 타입이 문제가 된다.  
그래서 type="int" 같은걸로 표현해줄 수도 있는데, 알아서 type 매칭 해주기도 한다.  

&nbsp;&nbsp;&nbsp;&nbsp;이 두가지 DI 방법 중에 어떤걸 쓸지는 자기 마음이긴한데, 뭐 doc 에서는 꼭 필수적인 의존관계는 생성자로,  옵션같은건 setter 로 하라고 하는데 spring 에서는 setter 방식을 조금 더 추천하는 모양이다.  (생성자에 arg를 주렁주렁 다는게 별로여서)  

**DI 과정**
1. ApplicationContext 가 생성되고 bean 들에 대한 정의를 가지고 있는 설정 메타 데이터를 보고 초기화된다.  
1. 각 bean들에 대한 의존 관계는 실제 bean 들이 생성될때 만들어진다.  
1. 의존 관계에 있는 속성들이 서로 다른 bean 들을 통해 세팅 된다.  (의존관계 연결된다. )  
1. 각 설정에 맞게 타입도 자동으로 변환된다.  

&nbsp;&nbsp;&nbsp;&nbsp;여기서 주의할 점은 스프링 컨테이너는 구동시에 bean 설정을 점검하지만 실제로 의존관계 나 속성들이 세팅되는건 실제로 bean 이 생성될 때 라는 거다.  싱글턴 패턴인 bean 들이나, pre-init 설정된 bean 들은 컨테이너 구동시 만들어지지만, 아닌 bean 들은 bean 이 불릴때 생성된다.  

&nbsp;&nbsp;&nbsp;&nbsp;이 특성 때문에, 실제로 속성들을 세팅할때 이미 컨테이너 가동 후에 에러를 발견할 수 있기 때문에  applicationcontext는 기본이 pre-init 이다. lazy-init 속성을 통해 bean 이 불릴때 생성되도록 해줄수는 있다.  

&nbsp;&nbsp;&nbsp;&nbsp;의존 관계 설정같은건 뭐 string 자체를 넣어서 해줄 수 도 있고, null 이나 빈 값도 가능하다.  property 세팅이나 constructor args 로 넣는 방법 쉽게 하기 위해서 c:~~ , p:~~~ 이런거 이싿는데 솔직히 여기서 처음봤다. ㅎㅎ 신기하네.  

그리고 bean 설정 파일에 depends-on 속성 통해서 명시적으로 의존 관계 표현해줄 수도 있다.  

autowire 속성을 통해서 applicationcontext 가 컨텐츠 보고 있다가 자동으로 연결해줄 수 도 있다.  주로 이름을 사용한다. 타입을 사용하는 경우도 있는데, 이때 타입이 명확히 하나로 정해지지 않고 해당 타입이 여러개 있으면 에러 발생시키기 때문이다.  

**autowire 통한 field 주입의 단점**
&nbsp;&nbsp;&nbsp;&nbsp;대충 보니까 평범하게 써왔던 그냥 의존관계 필요한 변수에 autowired 로 주입 시켜버리는걸 field 주입이라하고, 단점이 매우 많아서 피해야한다고 한다.  
이유를 조금 찾아보니, 세 가지가 있다.  
1) final 로 선언할 수 없다.  (주입된 객체가 가변하는 성질을 가져 코드상 문제를 발생시킬 수 있다.)
2) 테스트하기 불편하다. (코드 분리성이 떨어진다.)  
3) 의존 관계를 파악하기 어렵다. (순환 의존 관계에 대한 에러 파악 이런게 어렵다.)  
결론은 조금 더 똑똑하고 책임감있게 짜려면 constructor 버젼으로 해라 ^^  아 하나 배웠다!!  

&nbsp;&nbsp;&nbsp;&nbsp;이렇게 일단은 마무리 짓겠다.  좀 어렵긴한데, 확실히 doc 읽으니까 망나니였던 내 자신이 약간을 숙연해지는 효과가 있다 ^^  
인생 후르락끼 뚝딱뚜까딱~~~ 하던 내 모습 반성하게 되네요....   


**bean scope 에 관한 아주 잘 정리된 글은 여기[https://gmlwjd9405.github.io/2018/11/10/spring-beans.html] 에서!!**






















