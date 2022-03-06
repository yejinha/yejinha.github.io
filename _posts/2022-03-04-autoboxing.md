---
layout: post
title:  이펙티브자바 -불필요한 객체 생성 줄이기 & autoboxing 
date:  2022-03-04 17:00
img: websecurity.png # Add image post (optional)
categories: [study]
tags: [IT,  web, java, autoboxing] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

오늘의 TIL  

이펙티브 자바를 읽는 중인데, 어렵지만 조금씩 생각해볼 점들은 기록하려고 한다.  

# 아이템 6) 불필요한 객체 생성을 피해라.  

OOP의 장점인 재사용성을 생각하면 당연한 말이지만, 코드를 짤 때 은근히 간과할 수 있는 부분이다.   

어떤 자바 제공 유틸 클래스를 사용할 때, 단순히 로직만 생각해서 메소드를 마구 호출하곤 했다. 메서드 내부에서 어떤 객체를 생성하고 동작하는지를 유심히 살펴보지 못했다.  

특히나 책에 실린 예제인 String.matches 보고 반성하게 됐다.  

> String.matches 메서드 내부에서는 정규식용 Pattern 인스턴스를 생성하는데, 이는 한번 사용하고 바로 가비지 컬렉션 대상이 된다.  이를 개선하려면 클래스 초기화 과정에서 Pattern 인스턴스를 필요한 정규식으로 초기화 하고 메서드 호출시 재사용하는게 낫다. 


# autoboxing 
이렇게 나도 모르는 사이에 인스턴스를 생성하는 예시 중 하나가 오토박싱이다.  

## autoboxing 이란? 
기본 데이터 타입을 wraaper 클래스를 통해 객체로 변환하는 것  
정의부터가 객체로 만들어 사용하려고 하는것이기 때문에 그 사용 이유에 맞게 써야한다. 
남발해서는 안됨.  
~~~java
Integer a = 100; (실제 선언)
-> Interger a = new Integer(100); (오토박싱이 동작하는 방식)
~~~

## autoboxing 사용하는 이유  
[참고-Why do we use autoboxing and unboxing in Java?](https://stackoverflow.com/questions/27647407/why-do-we-use-autoboxing-and-unboxing-in-java)  
### Generics 사용하기 위함. 
-  primitive data 타입은 길이가 정해져있지 않는 반면에 Object, String 같은  타입은 크기가 정해져있다. 즉 상호호환되어 사용 가능하다. (String 을 Object 로 쓴다던지.)  

- Generics 사용할 땐 JVM 의 혼동을 줄이고자 모두 Object 로 생성한다. 따라서 Object로 호환될 수 없는 타입은 들어갈 수 없다.  
이를 type erasure 라고 부른다. [참고](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)  

~~~java
List<T> -> List<Integer> : 가능  List<int> : 불가능 
~~~


