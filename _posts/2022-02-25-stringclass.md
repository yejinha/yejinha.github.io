---
layout: post
title:  java  string builder, string buffer 그리고 string 이야기
date:  2022-02-25 15:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  java] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

오늘의 TIL   

String 에 대해서 조금 정리하고자 한다.  

# 1. String 클래스  
## imuutable 한 클래스 이다.  (변경 불가능한 클래스)  
인스턴스 생성시 인스턴스 변수에 문자형 배열 참조변수를 정의한다.  
즉 String  정의 시마다 문자열 배열이 생성되고 그 주소를 참조하는 구조이다.  
문자열은 읽어 올 수만 있고, 변경할 수 없다.    

## 문자열을 더해야하는 경우에 + 할 때마다 새로운 문자열 담긴 인스턴스 생성되기에 메모리 공간을 많이 차지하게 된다. 
## 이런 경우는 Stringbuffer 나 Stringbuilder 사용하는 것이 좋다.

문자열을 만들시에는 리터럴(내용)을 정의하거나 String 클래스의 생성자를 이용하는 방법 두가지 있다.  

같은 리터럴은 한번만 정의된다. (어차피 변경 불가능하니 참조하면 되니까)  

생성자로 만들시에는 문자열 배열 인스턴스가 각각 생성된다.  

# 2. == 과 .equals 차이 
즉 우리가 String 변수 정의한 건 문자열 배열 위치 정보를 담는다.   
== 으로 String 을 비교하면 주소값 비교이기에 오류가 생길 수 있다.    
.equals 해야 값 비교 가능하다.   

~~~java
   @Test
    void stringEqualsTest(){
        String s ="abc";
        String s2 ="abc";
        String s3 =new String("abc");
        String s4 = new String("abc");
        assertThat(s==s2).isTrue();
        assertThat(s3==s4).isFalse(); // 생성자로 만들어서 인스턴스 각 생성-> 주소 다름 
        assertThat(s3.equals(s4)).isTrue();// 리터럴은 똑같음 
    }

~~~

# 3. 문자열 결합하는 방법 
1) 문자열 사이에 구분자 넣어서 결헙   
java.util.Stringjoiner 클래스 사용. 
~~~java
  @Test
    void stringJoinTest(){
        StringJoiner sj = new StringJoiner("-","[","]"); // 구분자 - , prefix [, suffix ]
        String[] arr = new String[]{"one","two","three"};
        String actual = "[one-two-three]";
        for(String s: arr){
            sj.add(s);
        }
        assertThat(actual).isEqualTo(sj.toString());
    }

~~~

# 4. StringBuffer, StringBuilder
new 연산으로 클래스를 한 번만 만들기에 Mutable하다.  
미리 배열 공간 만들어놓고 바로 값을 복사한다. 배열 공간 선언하는 횟수가 현저히 줄어든다.  
공간 부족할 경우만 공간 늘리고 값을 복사한다.  

## 차이 
StringBuffer 는  동기화되어 느리지만 멀티쓰레드 환경에서 thread-safe 하다.  
StringBuilder 는 비동기화여서 빠르다.  

## 비교 
 equals 는 오버라이딩되어 있지않다. (내부 값 비교하고 싶으면 toString 후 equals)

 ## StringBuider 구현    
[유투버 엔지니어대한민국님](https://www.youtube.com/watch?v=gc7bo5_bxdA) 영상 을 보고 따라 만들어봤다.   
역시 코드로 보니 이해가 편해  굿

~~~java
public class BuilderTest {
        char[] values;  //값 저장할 문자열 배열
        int index;  //값 어디까지 차있는지 체크에 사용
        int size; //배열 사이즈

        public BuilderTest(int givenSize){
            values =new char[givenSize];
            index =0;
            size =givenSize;
        }

        public void append(String s){
            if (s == null){
                s = "null";
            }
            capicity(s.length()); //배열 크기 검사 
            for(int i =0; i < s.length() ; i++){
                    values[index] = s.charAt(i); //배열에 값 넣기
                    ++index;
            }
        }

        // 배열 공간 확인 후 부족하면 두배로 늘려줌
        public void capicity(int stringSize){
            if(index+stringSize > size){
                size = (size + stringSize)*2 ;
                char[] temp = new char[size];
                for(int i=0; i<values.length; i++){
                    temp[i] = values[i];  // 기존 값 복사
                }
                values = temp; // 늘린 배열로 교체
            }
        }

        public  String toString(){
           return new String(values, 0, index);
        }
    }
~~~

내가 셀프로 만든것과 StringBuilder 클래스 결과를 비교하는 테스트 

~~~java
//unit test
   @Test
    void builderTest(){
        BuilderTest builder = new BuilderTest(1);
        builder.append("hi");
        builder.append("hello");
        builder.append("good");

        StringBuilder sb =new StringBuilder();
        sb.append("hi");
        sb.append("hello");
        sb.append("good");
        
        assertThat(builder.toString()).isEqualTo(sb.toString());
    }
~~~~


