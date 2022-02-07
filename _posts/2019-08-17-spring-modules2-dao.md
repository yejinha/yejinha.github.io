---
layout: post
title: 스프링을 구성하는 모듈 2- Data Access/Integration
date:  2019-08-17 21:36:00 +0800
img: two.jpg # Add image post (optional)
categories: [spring]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

# 모든걸 다 해주는 기가 막힌 프레임워크가 있다? 
![스프링 모듈 구조](http://yaejinha.github.io/assets/img/spring-overview.png)

이제 두번째 모듈 Data Access/Integration  차례다.   

2. Data Access/Integration  
이름에서 알 수 있듯이 데이터 관련된 처리를 해주는 모듈이다.  
JDBC나 트랜잭션 같은 DB 연결할때 자주 봤던 친구가 있는가하면,  ORM, OXM, JMC ??  얘네는 도대체 누구지..? 누구세요;;;;  
모르는 것들 정리하는김에 어렴풋이 알고 있는 JDBC 나 트랜잭션도 잡고 가야겠다 !  

**JDBC**  

> JDBC란, 자바 언어로 다양한 종류의 관계형 데이터베이스에 접속하고 SQL문을 수행하여 처리하고자 할 때 사용되는 표준 SQL 인터페이스  
> API입니다. JDBC는 자바의 표준 에디션에서 지원하는 기술로서, 접속하려는 DBMS 서버에 따라서 JDBC 드라이버가 필요합니다. 
> https://opentutorials.org/module/3569/21222

&nbsp;&nbsp;&nbsp;&nbsp;저 블로그에 너무 잘 정리가 되어있다. JDBC 인터페이스를 작동시키기 위해서  인터페이스 구현체인 클래스를 JDBC 드라이버 라고 한다.  
작동을 위해 JDBC 드라이버를 메모리에 올리고, DB 연결을 하고 ,  쿼리 구문을 쓰고, 결과값을 얻었다면 다시 연결을 해제하는 과정이 필요하다.  

&nbsp;&nbsp;&nbsp;&nbsp;이 지겨운 과정을 대신해주는 역할을 하는게 바로 이 모듈이다.  프레임워크가 알아서 해줄테니 사용자는 설정값 입력, 쿼리 작성정도만 하면 원하는때 알아서 연결해서 데이터 가져오고 알아서 연결해제까지 해준다.  

스프링 프레임워크에서는 이 기능을 간편하게 하기 위해서 **Mybatis** 를 사용한다. 

그렇다면 <u>mybatis 와 JDBC 의 차이점</u>은 뭘까?  
Mybatis 는 JDBC 를 편하게 다루기 위한 라이브러리 이다. JDBC 의 단점은 다음과 같다.  
- 같은 연결 코드를 여러번 중복해서 사용해야한다.   
- 같은 파일 내에 자바 코드와  sql문이 혼용되어 있다.  
- preparedstatement 같이 변수 들어가는 쿼리문을 사용자가 직접 준비하고 대입하는 코드 작성해야한다.  
-  결과물을 java object 로 넣는 코드를 사용자가 짜야 한다.  

이런 불편함을 없애주는 라이브러리가 Mybatis 이다.  사용법도 간단하다.  사용법 예시를 찾아왔다.  
1) sql 쿼리를 StudentMapper.xml 파일에  작성한다.  (일반적으로 쿼리 작성하는 xml 파일 이름 ~mapper.xml)   
~~~
  INSERT INTO STUDENTS(STUD_ID,NAME,EMAIL,DOB)  
  VALUES(#{studId},#{name},#{email},#{dob})
~~~
2) xml 파일과 동명의 인터페이스 생성한다.  
~~~
public interface StudentMapper
{
  Student findStudentById(Integer id);
  void insertStudent(Student student);
}
~~~  
3) 자바 코드에서 불러와서 사용한다.  
~~~  
SqlSession session = getSqlSessionFactory().openSession();
StudentMapper mapper =  
session.getMapper(StudentMapper.class);
// Select Student by Id
Student student = mapper.selectStudentById(1);
//To insert a Student record
mapper.insertStudent(student);
~~~


**트랜잭션** 

아 짜증난다. 너무 길어진다 ㅠㅠㅠ ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ  
![길어 ㅠ ](http//yaejinha.github.io/assets/img/long.jpg)  

트랜잭션 관련해서도 또 정리 잘해논 [블로그](http://egloos.zum.com/springmvc/v/495798) 찾았다 ㅎㅎ  지식보다 정보 찾는게 늘어가네 ㅎㅎ   

> 2개 이상의 쿼리를 하나의 커넥션으로 묶어 DB에 전송하고 이 과정에서 어떠한 에러가 발생할 경우 자동으로 모든 과정을 원래 상태로 돌려놓는다. > 그야 말로 All Or Nothing 전략이다. 
> by 블로그 주인분 

결국은 이 Data Access/Integration 모듈에서 트랜잭션 역할도 해준다는 건데, 그럼 도대체 어떻게 해주는거지?  

스프링에서의 트랜잭션 관련된 내용은 또 이 [블로그](https://syaku.tistory.com/269)  에 잘 나와있다. 나는야 지식 소매상~~~ 조금씩 떼다 써요~~~  

**ORM**
관계형 데이터베이스 객체를 자바 객체로 매핑시켜주는 것으로  sql 쿼리를 직접 설정해야하는  mybatis 같은 mapper 기반  라이브러리와는 다르게 객체간의 관계로 sql을 자동으로 설정해준다.  [블로그](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)

**OXM** 
Objext Xml Mapping 이라는 뜻으로 xml 을 자바 객체로 매핑시켜주는 역할을 한다.  음 사실 잘 이해가 안된다. 예시로 확 와닿는게 없어서 그런가.   머 marshalling 하고 serialization 한다 어쩌고 하는데 잘 모르겠다. 
[블로그](https://zuyo.tistory.com/580)  에서 찾은 marshalling serialization 차이부터 익혀야겠다.   회사 소스에서 post 로 파라미터 쏘기 전에 항상 serialization 하던데 이런 거 였구나.   

>마샬링은 추가적인 메타 데이터(코드 베이스)를 가질 수 있다는 점에서 직렬화와 구분된다.  
>직렬화는 객체와 관계된 메소드를 포함하지 않고, 오브젝트에 있는 멤버 데이터만(코드 없이) 바이트 스트림에 써진다.  
>마샬링에서는 오브젝트에 있는 멤버데이터 + 코드베이스가 전해진다.  
>따라서 직렬화는 마샬링의 일부라고 할 수 있다.  