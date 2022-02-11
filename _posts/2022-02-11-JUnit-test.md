---
layout: post
title:  JUnit 으로 단위 테스트 하기.  
date:  2022-02-11 18:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  JAVA, Juit] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

오늘의 TIL !!   Junit 에 대해서 간단히 정리하고자 한다.  
넥스트 스텝의 자바 플레이그라운드 with TDD, 클린코드 강의를 참고 하였습니다.  

# 0. JUit 이 나오게 된 이유  
메인 메소드의 용도 중 하나로  테스트 기능이 있다.   
처음 자바를 배우면 sysout 으로 열심히 찍어가면서 테스트하던 그 용도이다.  
하지만 메인 메소드에 테스트 코드를 쓰게 되면 아래와 같은 단점들이 있다.(강의 중 여러 단점이 있는데 내가 와닿은 단점들)

- 테스트 결과를 사람이 수동으로 확인해야함. 
- 메소드 이름을 통해 어떤 부분을 테스트 하려는지 드러내기 어려움. 
- 메인 메소드 하나에서 여러개의 기능을 테스트 하므로 복잡도가 증가함. 

이걸 피하고자 유닛 테스트가 필요해졌고, JUnit 이 나오게 됐다. 

*유닛테스트*  
유닛 테스트는 화이트 박스 테스팅으로 테스터는 어떻게 동작할지를 고르고 인풋을 정하고 예측한 결과대로 나오는지 확인한다.   
unit 이라는 단어에서 볼 수 있듯이 하나의 메소드나 클래스처럼 스코프를 한정시켜 그 부분을 테스트한다.  
[출처](https://www.parasoft.com/blog/junit-tutorial-setting-up-writing-and-running-java-unit-tests/)

# 1. JUnit 이란? 
Java 의 유닛 테스트 프레임워크이다. 
JUnit 5 가 가장 최신 버전이며,  platform, Jupiter, Vintage 세 가지 섹션으로 이루어져있다.  
Platform :  JVM 위에서 테스트 프레임워크 동작하는데 기초를 제공한다.   
Jupiter :  테스트 작성하는데 필요한 API 모델의 모음이다.  
Vintage : 이전 버전을 기반으로 짜놓은 코드를 돌아가기 위한 도움을 준다.  
Junit 5 는 이전 버전으로 짜여진 코드여도 컴파일은 정상으로 되지만 공식적으로 java 8부터 지원한다.   

# 2. 자주 쓰이는 어노테이션들 
## @Test 
 이 메소드가 테스트 메소드라는 걸 알려준다. 

 ## @BeforeEach , @AfterEach 
 각각의 @Test 실행 전이나 후에 꼭 실행된다. Junit4 에서의 @Before, @After 와 같은 역할이다.  

 ## @PaParameterizedTest
 테스트에 파라미터를 넣고 싶을 때 쓰인다.  
 선언 후 @ValueSource 로 파라미터 넣는다.  
 파라미터를 여러 케이스로 넣는 경우는  @CsvSource 로 구분하여 넣는다. 이때 구분자는 기본 ,  이다.  

 ~~~java
 @ParameterizedTest
    @ValueSource(ints = {1,2,3}) // six numbers
    void paramedTest (int number) {
        assertThat(numbers).contains(number);
    }
 ~~~~

아래 예시는 1,2,3 은 통과,  4,5 를 넣으면 실패하는 로직을 확인하기 위해 만든 테스트이다.  

~~~java
    @ParameterizedTest
    @DisplayName(" paramed Test ->  1,2,3 pass , 4,5 fail ")
    @CsvSource(value={"1,2,3","4,5"})
    void paremedTestWithFail(int number){
        assertThat(numbers).contains(number);
    }
~~~~

## @DisplayName
위에서 보이듯이 메소드명 말고 테스트 이름을 붙여줄 때 사용한다.  위처럼 사용한 결과이다.  
테스트 명이 @DisplayName 어노테이션에서 사용한대로 나왔음을 알 수 있다. 
![예시](/assets/img/displayname.png)


# 3. Assertions & Assumptions 
위에서 말했듯이 단위 테스트는 화이트 테스트이기 때문에 우리가 작동하리라 예상한 방향이 있다.  
이를 실제 작동한 값과 비교해 테스트 결과를 내는게 이 메소드들이다.  
변수로는 우리가 예상한 값과 실제 값이 들어간다.  

~~~java
 @Test
    void split(){
        String[] actual = "1,2".split(",");
        assertThat(actual).isEqualTo(new String[]{"1","2"})
                .containsExactly("1","2");
    }

~~~

배열에서 어떤 값이 있는지 포함되어있는지 알고 싶을 땐, contains 사용. 
오직 예상한 값만 있는지 확인하고 싶을때는  containsExactly 쓴다.   
위의 메소드 실행하면 pass 나온다.  "1,2" 인 스트링을 , 기준으로 자르면 "1","2" 만 있기에. 

~~~java
@Test
    void split(){
        String[] actual = "1,2,3".split(",");
        assertThat(actual).isEqualTo(new String[]{"1","2","3"})
                .containsExactly("1","2");
    }

~~~~
하지만 이렇게 하면 오류난다.  실제는 "1","2","3" 인데 우리가 기대한 값은 "1","2" 이기 때문에 !! 

### assertThatThrownBy & assertThatExceptionOfType
exception 던지는거 확인할 때 쓰는 메소드!! 
아래 예시는 IndexOutOfException 나는지 테스트 하는거 

~~~java
@Test
@DisplayName(“charat outOfIndex test”)
void charAtTest(){
    int index = 3;
    /assertThatThrownBy/(()->{
        char actual = “abc”.charAt(index);
    }).isInstanceOf(IndexOutOfBoundsException.class).hasMessageContaining(“String index out of range: %d”,index);
}

OR 

assertThatExceptionOfType/(IndexOutOfBoundsException.class).isThrownBy(()->{
    char actual = “abc”.charAt(index);
});

~~~


---

이 정도로 간단히 쓰고 스프링 공부하거나 작은 내 토이프로젝트에 실제로 적용해보면서 공부해야겠다 !!
그럼이만~~~~~