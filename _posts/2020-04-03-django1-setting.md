---
layout: post
title: 얼레벌레 django 따라하기 1 - 기본 세팅과 hello world
date:  2020-04-03 17:00
img: lukeknowsnothing.jpg # Add image post (optional)
categories: [python]
tags: [IT, 파이썬, python, django, web] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

파이썬 웹 개발 연습차 django 로 기본 실습을  해보고자 한다 ~!!  

**TO DO LIST** 
- 파이썬 설치 + django 라이브러리 기본 세팅
- hello world ^^ (get) 
- db 연결
- db 저장 (admin페이지)
- 요청 파라미터 값을 db 에 저장 (get) 
- db 저장값 가져와서 출력 
- post 요청해보기

\* 실습환경은 mac os   

이번 포스트에서는 기본세팅 +  hello world 

# 1. 파이썬 + 가상환경 컨트롤 라이브러리 설치
homebrew 이용해서 커맨드 창으로 설치했다. 
- homebrew 설치
~~~cmd
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
~~~
- python3 설치
~~~
brew install python3
~~~
여기까지 설치하면 필요한 기본 설치는 끝났다.  

파이썬에서는  가상 환경이라는게 있다.   
여러 버전의 python 을 통해 서로 다른 프로젝트들 할때, 프로젝트 별로 환경을 따로 따서, 설정이 꼬이는걸 방지한다는데 사실 이해를 잘 못했다.  
그래도 또 남이 하는건 다 해보고 싶으니까 pyenv, virtualenv 설치했다. 

- pyenv (파이썬 버전관리)
~~~
brew install pyenv
~~~

- pyenv-virtualenv (가상환경- 프로젝트별로 따서 사용할 것)
~~~
brew install pyenv-virtualenv
~~~

- pyenv 로 파이썬 설치 및 전역 사용 버전 정해주기
~~~
pyenv install  3.6.1
pyenv global 3.6.1  
~~~

- 프로젝트 경로에 가상환경 생성해주기   
이제 실제로 프로젝트 관련 소스가 있는 경로에 가상환경을 생성해주고 활성화 한다.    
프로젝트 소스 존재할 폴더 하나 만들고 그 폴더 경로해 가서 명령어 입력한다.  
~~~
python3 -m venv 내가사용할가상환경이름 
source 내가사용할가상환경이름/bin/activate  ##내 경우는 myenv 로 함
~~~ 

# 2. django 설치 및 프로젝트 생성

- django 설치 
~~~
 pip install django
~~~

- 프로젝트 생성
~~~
django-admin startproject 프로젝트이름 .
~~~

여기까지 하면 기본 프로젝트는 생성이다.  
프로젝트를 구성하는 기본 세팅 파일들이 있고, 어플리케이션을 만들고 프로젝트에 붙여서 실제 뭔가 돌아가는걸 보는거다.   

즉 익숙한 스프링 프로젝트로 비유하자면 일단 프로젝트 하나 생성해서 pom.xml 같은 설정파일에 라이브러리 로딩한 정도 느낌..?  

기본 설정 파일 역할은 [여기](https://jackerlab.com/django-make-application-1/) 에서 보고 배웠다. 


# 3. 어플리케이션 만들기 

- 어플리케이션 생성   
~~~
python manage.py startapp 어플리케이션이름
~~~

이렇게하면 프레임워크 기본 구조대로 생성된다. django 는 model, template, view 구조이다.  
**model**: db 관련된 사항으로 데이터 구조 역할이다.   
**template** : 화면단과 관련됨 (mvc 구조에서 view 느낌)  
**view** :서버 로직 

추가로 urls.py 파일이 있는데, 이건 dispatcher 역할을 한다. 
url 과  해당 url 요청 들어오면 실행해야할 메소드 짝지어준다.     

- hello world 출력할 인덱스 페이지 url추가   
어떤 url 입력 > Hello world 출력하도록 할 인덱스 페이지 url 추가하기  

어플리케이션 폴더의 url.py 
~~~python
urlpatterns = [
    path('', views.index, name='index'),  ##http://127.0.0.1/어플리케이션이름  으로 접근하면, views.py 에 정의된 index 메소드 실행 
]
~~~

프로젝트 폴더의 url.py  
~~~python
urlpatterns = [
    path('hello/', include('hello.urls')),  # 이 부분 추가 - hello 어플리케이션의 Url 패턴 추가해줌 hello 이부분에 본인이 정한 어플리케이션 이름 들어간다. 
    path('admin/', admin.site.urls),  # 관리자 페이지 url 로 기본 포함되어있음 
]
~~~


- hello world 출력할 서버 로직 view 에 추가 
실제로 인덱스 페이지 url 로 요청 들어오면 응답할 서버 로직 작성 

~~~python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):  # 리퀘스트 들어오면 응답으로 hello world  보냄
    return HttpResponse("Hello, world.")
~~~

- 서버 띄워서 확인하기   

기본 구성은 다 됐다. 이제 서버를 띄우고, url 요청을 넣어서 원하는 값 반환되는지 확인한다. 

```python 
python manage.py runserver #서버 실행
```

기본 세팅을 변경하기 않았다면 8000번 포트로 접근한다.  
http://127.0.0.1:8000/hello/

![쟝고헬로우월드](/assets/img/djangohelloworld.png)  

짠 너무 잘됩니당 