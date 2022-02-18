---
layout: post
title:  java class 생성자 말고 정적 팩토리 메소드 사용하기 (이펙티브 자바)
date:  2022-02-18 15:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  java] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

이펙티브 자바를 읽고 있는데,  여기서 말하는 정적 팩토리 메소드 사용하기를 java exercism 문제 풀다가 활용했다.   
반갑기도 하고 신기하기도 해서 정리하는 생성자 이야기 !!  

# 1. 생성자란?   
인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드이다.  (인스턴스란 클래스로부터 만들어진 객체이다.)   
사용자가 정의하지 않는다면 클래스명과 같은 이름으로 아무 매개변수 없는 기본 생성자가 만들어져 내부 동작한다.  
사용자가 원하는 로직대로 초기화 하고 싶다면 클래스명과 같은 이름으로 생성자 만들면된다.   


# 2. 정적 팩토리 메서드   
생성자 말고 클래스의 인스턴스 반환하는 단순한 정적 메서드.  

## 장점 
- 이름을 가질 수 있다. (클래스 명과 동일할 필요 없음.)   
- 호출될 때마다 인스턴스 만들지 않아도 된다.  (싱글턴으로 활용할 수 있음)
- 반환 타입의 하위 타입 반환 가능하다.
- static method를 작성하는 시점에 반환할 객체의 클래스가 존재하지 않아도 된다. (실행시 어떤 클래스 사용할지 정해져도 된다.)

## 단점 
- 상속을 위해서는 정적 메소드만 있으면 public, protected 필요하기에 정적 팩토리 메서드만을 가진 클래스 상속할 수 없다.  
- 프로그래머가 찾기 어렵다. -> 메서드 명명 규칙을 따라서 인스턴스화 하기 쉽도록 해야한다.  
(ex. ~~~.of() 이런식으로 해야함. )
예시 - from(매개변수 1개) , of(매개변수 여러개), getInstance(), newInstance(늘 새로운 인스턴스임)  

# 3. 활용 예시  
Exercism - java 문제 중에 Elons toy car 가 있다. class 생성 연습하는 문젠데 여기서 정적 팩토리 메서드로 사용하였다.   

*문제 조건*  buy() 로 차 구매해서 drive 호출시마다 거리는 20 미터 증가, 배터리는 1퍼센트씩 깎이도록 하여라.
거리가 2000 미터 이상이면 더 이상 달리지 못하도록 하기.   

여기서 그냥 생성자 써도 되지만 정적 팩토리 메서드를 사용하였다.  
스켈레톤 소스에서 그렇게 하도록 유도되어있는데, 이는  정적 팩토리의  장점인 이름을 가질 수 있다는 점을 이용해 직관적으로 buy 라는 행위의 의미를 전달하기 위함으로 인거 같다.  
나는 배운걸 바로 써볼 수 있어서 즐거웠다. ㅎㅎ  나이스 타이밍 ㅎㅎ   
다른 문제에도 적용해봐야징 ㅎㅎ  

그리고 역시나 단점으로 되어있는 프로그래머가 알아차리기 어렵다도 겪었다...  
exercism 에서 제공하는 테스트에서조차, 스켈레톤을 정적 팩토리 메서드 쓰도록 유도했음에도 불구하고 인스턴스화를 기본 생성자를 불러서 테스트 코드를 짰더라 !!   
내가 테스트코드대로 돌리려면 굳이 기본 생성자를 만들어서 거기서 변수 초기화 하고 정적 팩토리 메서드에서는 생성자 호출해 인스턴스 반환해주는 일만 하면 된다.  

그렇게 짤 수 도 있지만 굳이 그래야하나 싶기는 하다.  
그래서 짠 내 코드 !! 



~~~java
public class ElonsToyCar {

    private  int distance;
    private  int battery;

    // 싱글톤으로 할 필요 없어서 정적 팩토리 메서드 호출시마다 새 인스턴스 생성해 반환하도록 하였다. 
    public static ElonsToyCar buy() {
        ElonsToyCar car = new ElonsToyCar();
        car.battery=100;
        car.distance =0;
        return car;
    }

    public String distanceDisplay() {
       return String.format("Driven %d meters",distance);
    }

    public String batteryDisplay() {
        if(battery==0){
            return "Battery empty";
        }
        return String.format("Battery at %d%%",battery);
    }

    public void drive() {
       if(distance < 2000){
           distance +=20;
           --battery;
       }
    }
}



~~~


