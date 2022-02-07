---
layout: post
title: flask 가 뭔데..? flask blueprint
date:  2021-03-16 19:00
img: study_ari.png # Add image post (optional)
categories: [python]
tags: [IT, 파이썬, python, flask, web, flask blueprint] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

저번 스터디에서는 간략하게나마  django 를 써봤는데,  이번에는 flask 를 써보게 됐다.   
까먹기전에 써놓기.  

blueprint 라는 개념을 배웠는데,  아직 완전히 이해한 거 같지는 않지만 어렴풋이 느끼기에는  django 랑 flask 의 큰 차이 같다.   
예전 포스팅에서 해봤듯이 django 에서는  컴포넌트를 붙일때 따로 폴더 경로에 몰고 url 을 지정해주는 파일이 있었다.  (urldispatcher 역할을 하는 파일. )  
은근 헷갈리기도 하고 초보인 내 입장에서는 여기서 연결 잘못해서 헤매기도 했다.  

근데 flask 에서는  blueprint라는 컴포넌트를 사용해서 url 매칭을  자유롭게 할 수 있게 도와준다.  

메인 서버를 띄울 곳에서만  @. route 를 사용하고  나머지 컴포넌트들에서는 각 컴포넌트를 이름을 가진 블루프린트 객체로 지정한뒤에 메인서버에서 부르기만 하면 된다.  

이게 무슨 말인가 싶겠지만, 예제 소스를 보면 조금 이해가 간다.  

폴더 구조가 아래와 같이 되어있고, 서버는 server.py 에서만 띄우고자 한다. 

sub  
  ㄴ blutest.py  
sub2   
  ㄴ bluetest2.py  
server.py  

bluetest.py  소스 

~~~python
from flask import Blueprint

blue_test = Blueprint('myTEST', __name__) 

@blue_test.route('/hello')
def test():
    return 'TEST 입니당'
~~~


bluetest2.py 소스 
~~~python
from flask import Blueprint

blue_test2 = Blueprint('myTEST2', __name__) 

@blue_test2.route('/hello')
def test():
    return '두번째 테스트지롱'
~~~

서버 소스 

```python
from flask import Flask
from sub import bluetest
from sub2 import bluetest2

app = Flask(__name__)
app.register_blueprint(bluetest.blue_test, url_prefix='/test')
app.register_blueprint(bluetest2.blue_test2, url_prefix='/test2')  # 서로 다른 경로 
if __name__ == '__main__':
    app.run(host='0.0.0.0', port='8088')

``` 


이런식으로 쉽게 경로를  부여할 수 있다.  
초보자인 내 입장에서는 이게 더 쉬워보인다. (물론 내가 100퍼 이해한 거 같진 않지만 ㅎㅎ)  

기능 별로 폴더 따고 하나씩 경로 지정해 쓰면  편할 거 같다.  

