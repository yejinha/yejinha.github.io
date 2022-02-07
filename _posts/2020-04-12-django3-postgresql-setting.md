---
layout: post
title: 얼레벌레 django 따라하기 2 - postgresql db 세팅해보기
date:  2020-04-12 17:00
img: lukeknowsnothing.jpg # Add image post (optional)
categories: [python]
tags: [IT, 파이썬, python, django, web, postgresql] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

기본 sqlite 설정 말고, postgresql 연결해봤다.   

## to do 
1. default db 를 postgresql 로 해주기    
2. db 여러개 사용하기 - database router 사용    



**실습 환경은 mac OS   

# 1. postgresql 설치   
우선 로컬에 postgresql 설치해서 db 생성한다.  

~~~console
brew install postgresql  
~~~

^^ 너무 간단하죵?   

# 2. 유저 설정해주기  
기본적으로는 로컬 사용자가 유저로 등록되지만, db에서 테이블 생성하고 변경하고 기타 등등하는 유저 등록해준다.  


```console
psql postgres  # 로컬 사용자 게정으로 일단 들어감 

CREATE ROLE 생성할 유저이름 WITH LOGIN PASSWORD '사용할 비번';  #   유저 생성  
ALTER ROLE 생성한 유저이름 CREATEDB  # db 생성 권한 부여   

\q  # postgresql 종료

psql postgres -U 생성한 유저 이름  # 생상한 유저로 들어가 확인  

\du # 유저리스트 확인   

                                   List of roles
 Role name |                         Attributes                         | Member of 
-----------+------------------------------------------------------------+-----------
 hayejin   | Superuser, Create role, Create DB, Replication, Bypass RLS | {}  ## 로컬 사용자
 yj        | Create DB                                                  | {}  ## 추가한 사용자


```


# 3. db 생성해주기 
사용할 db 생성해준다. 

```console
psql postgres       

CREATE DATABASE sampleDB ENCODING 'utf-8';  # db 생성하면서 인코딩 타입 정해준다. 
ALTER database sampleDB owner to yj;  #사용할 사용자에게 권한주기 

grant all on database sampleDB to yj with grant option;
```

이제 이 db 를 쟝고의 모델이랑 연결해주기만 하면 된다.  

---

# Django model 과 postgresql 연결하기  

## 기본 지식  
모델은 실제 데이터베이스와 객체를 연결하는 역할을 한다.  
즉 모델을 생성하였다면 이러저러한 내용물을 가지는 객체의 설계도를 선언한 것과 같다.  
실질적으로 데이터베이스에 이 구성대로 테이블을 만들고 값을 조작하기 위해서는 어플리케이션을 통해서 동기화해야한다.  
이 과정이 마이그레이션이다.   
**모델을 생성함 -->  마이그레이션 통해 설정대로 데이터 베이스에 구현하라고 마이그레이션 시켜야함**
이 과정을 거쳐야지 실제 데이터베이스에 우리가  사용하고자하는 내용대로 테이블이 생성된 것이다.  
ORM 을 사용하여 그 이후에 CRUD 는 함수 코딩을 통해 할 수 있다.  
이 필수 과정을 하기 위해서는 **이 모델을 어느 데이터베이스에 테이블로 생성할지를 정의**해야한다. 

## psycopg2 설치하기. 

psycopg2 는 파이썬에서 사용하는 postgresql 어댑터 라이브러리다.    
pip install psycopg2 로 설치해준다 ^^  


# 1. 기본 설정 postgresql 로 하기 


## postgresql 데이터베이스 설정 명시 
이제 postgresql 사용하겠다고 설정을 명시해준다. 프로젝트의 설정파일인 settings.py 에 적어준다.  

~~~python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2', ## 어댑터 명시
        'NAME': 'sampledb',  #사용할 db 이름
        'USER':'yj', #테이블 생성 권한 있는 유저
        'PASSWORD':'비밀번호', 
        'HOST':'127.0.0.1', # 로컬에 설치하여 사용하니까 로컬호스트
        'PORT':'5432', # postgresql 기본 포트 
    },
}
~~~

## 서버 올려서 확인  
관리자 페이지 통해서 간단히 확인해볼 수 있다.  
기본은 별로 어렵지 않다. 그냥 어댑터만 잘 깔면 괜찮음 ㅎㅎ   


# 2. db 여러개 사용하기 - database router 사용

모종의 이유로 모델마다 사용하는 db 를 분리해주고 싶을 때가 있을 것이다.   
그 경우 어떻게 하는지 해봤다. 여기서는 database router  사용한다.  
공식 레퍼는 [여기](https://docs.djangoproject.com/en/3.0/topics/db/multi-db/) 

## 여러 db 설정해주기 - settings.py   
나는 디폴트는 기본 설정인 sqlite 사용하고,  postgreE 모델만 postgresql로 들어가도록 했다.  

~~~python
DATABASES = {
      'default': {
       'ENGINE': 'django.db.backends.sqlite3',
       'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    },
    'postgre': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'sampledb',
        'USER':'yj',
        'PASSWORD':'비번',
        'HOST':'127.0.0.1',
        'PORT':'5432',
    },
} 
~~~

## database router 설정해주기
 어떤 모델은 어느 db 에 저장하라고 라우팅 설정을 해줘야한다. 
 어차피 setting.py 에 경로 명시하니까 아무데나 라우터 파일 만든다.  
 라우터 파일에는 여러 메소드가 있는데 가장 필요한 기본 메소드만 했다.  

~~~python

from django.conf import settings

class PostgresqlRouter: 
 ## 모델의 앱 이름이 postgreE 면 postgre 란 이름의 db 로 가라고 라우팅 해줘야함 
    def db_for_read(self,model,**hints):  # select 
        if model._meta.app_label == 'postgreE': 
            return 'postgre'
        return None

    def db_for_write(self,model,**hints): # insert update delete etc
        if model._meta.app_label == 'postgreE':
            return 'postgre'
        return None

    def allow_relation(self,obj1,obj2,**hints): #다른 db 조작할때  사용할 건지. 나는 복잡하게 설정하기 싫어서 그냥 true 로 다 허용 
        return True

    def allow_migrate(self,db,app_label,model_name=None,**hints): #마이그레이션 할때 관심있는 모델의 변화만 할건지 정해줄 수 있다. 예를 들어 나랑 관계없는 모델 변화 마이그레이션이면 패스할 수 있다.   
    # 나는 역시 복잡한거 싫어서 일단 전체 마이그레이션 한다.  
        return True
~~~


## 모델 메타 데이터에 앱 이름 명시 (선택사항)  

여기서 주목해야할 부분은 model._meta.app_label 부분이다.  모델명으로 하려면 model._meta.model_name 으로도 명시해 줄 수 있다.   
이건 더 세세하게 쪼개는거 겠지.  나는 그냥 지금은 한 앱 내의 모든 모델은 이쪽 db 로 처리하세요 하는 뜻으로 앱 이름으로 해줬다.    
이를 위해서는 모델 메타데이터에 앱 이름 명시해줘야한다.   



~~~python

class Log2(models.Model):  #로그 데이터 저장할 모델 
    #어쩌고 저쩌고 모델  내용 

    class Meta:
        app_label = 'postgreE'  ##앱 이름 정해준다.  
~~~



간단한 작업이었지만 database router 라는 개념을 알게됐다.  신가하당 ㅎㅎㅎ  
사아즈가 큰 서비스를 만들때는 유용할 것 같다.  오늘의 공부 끝  !   