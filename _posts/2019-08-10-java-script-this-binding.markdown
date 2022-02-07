---
layout: post
title: Javascript this 바인딩 1 [객체 내부 메서드 호출과 함수 호출]
date:  2019-08-10 12:36:00 +0800
img: javascript.jpg # Add image post (optional)
categories: [study]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

 ``함수 호출 방식에 따라서``  this 인자는 다른 객체를 참조한다. 

&nbsp;&nbsp;&nbsp;&nbsp;JS 로 Exercism 문제를 풀고 있는데 이 개념을 정확히 안 잡고 가니까 자꾸 엉뚱한데에서 헤맸다. 이것도 정리해봐야겠다. ( 인사이드 자바스크립트 책 참고)

&nbsp;&nbsp;&nbsp;&nbsp;그래서 결국 어떻게 호출되었는지 상관없이 똑같이 행동하는 bind 가 나오게 되었다. 이거는 뭐 this 헷갈리는건 만국 공통이었다 이거구만.


``1. 객체가 내부 메서드 호출시 ``
객체 내부의 메서드 프로퍼티를 호출할때  this 는  해당 메서드를 호출한 객체로 바인딩된다. 

        var myObject ={
           name: 'foo',
           sayName: function(){
             console.log (this.name);    // 객체의 내부 메서드 sayName
                                       //   객체 myObject 의 name
             }
        };
        
        var otherObject={
           name : 'bar'
        };
        
       otherObject.sayName = myObject.sayName;
         
       myObject.sayName();
       otherObject.sayName();
       =======================================
       foo
       bar
     두 번의 sayName 메서드가  호출한 객체의 name 으로 바인딩 된 것을 확인할 수 있다. 

``2. 함수 호출시의  this ``
&nbsp;&nbsp;&nbsp;&nbsp;함수 호출시에는 this 가  전역 객체에  바인딩 된다. node.js 같은 환경 이용할 땐 전역 객체는 global,  브라우저 실행시에는 window 객체 바인딩 된다.  
변수명을 정의할 때 잘 정의해야하는 이유가 되기도 한다. (window 객체 이름이랑 중복되면 바인딩 시 에러의 원인이 되니까) 

&nbsp;&nbsp;&nbsp;&nbsp;여기까지는 이해가 쉬웠는데 내부함수를 호출할 경우 부터 개념이 헷갈리기 시작했다. 
내부 함수에 this 를 사용한다면, 내부 함수 호출도 함수 호출 패턴으로 인식되기에 전역 객체에 바인딩 된다.   

&nbsp;&nbsp;&nbsp;&nbsp;그래서 책에서는 this 값을 저장하는 that 변수를 따로 이용해서 거기에 접근하도록 하지만, bind 함수를 사용해서 해결 가능하다. 

```
    var value = 100;
    var myObject={
    	value=1,
    	func1: function(){
	    		var that=this;  //that 변수에 this 저장함으로써 접근 가능
	    		this.value+=1;
		    	console. log (this.value) ;  // 2
		    	func2:  function(){
			    	that.value+=1; //3
			    	console.log(that.value);
		    	}
		    	func2(); 
	    	}
    };
    myObject.func1(); 
    =======================================
    2  
    3
```
`` 호출 주체의 중요성을 느낄 수 있다. ``  

&nbsp;&nbsp;&nbsp;&nbsp;누가 실행했냐에 따라서 this  의 주체가 달라진다.  
단순히 매핑시켜놓은 것과는 무관하고 호출 시기가 중요하다.  

그래서 언제 누가 호출해도 같은 방식이 되도록 bind하는 방식이 나왔다.  
위의 예제를 bind 활용해서 바꾸면 다음과 같다.  

```
 var value = 100;
    var myObject={
    	value: 1,
    	func1: function(){
	    		this.value+=1;
		    	console. log (this.value) ;  // 2
		    	func2= function(){
			    	this.value+=1; //3
			    	console.log("func2   "+this.value);
		    	}.bind(this)  // func1 의 this (myObject) 를 바인딩시켜 같은 value 가리키도록
                func2();
	    	}
    };
    myObject.func1(); 
```
