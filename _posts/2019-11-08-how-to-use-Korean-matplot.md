---
layout: post
title: Matplot 에서 한글 폰트 적용하는 법
date:  2019-11-08 16:36:00 +0800
img: graph.jpg # Add image post (optional)
categories: [python]
tags: [공부] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

matplot  에서 한글 폰트를 적용하지 않으면, 축의 한글이 깨진다. ㅠ 

이를 해결하는 방법  **:thinking:**

matplotlib 의rc paramater 설정으로 전체 그래프에 적용한다. 

1) matplotlib 캐싱 폰트 리스트들에  내가  바꾸고자 하는 한글파일이 캐싱되어있는지 확인한다. 

~~~python
import matplotlib as mpl
import matplotlib.pyplot as plt
print('matplot 캐시 폴더',mpl.get_cachedir())  //matplot 캐시 폴더
>> matplot 캐시 리스트 C:\Users\Admin\.matplotlib
~~~ 

2) 캐싱되어있지 않다면 폰트 설치 후 jupyter 재시작하고 폰트리스트 json 캐싱 확인
![캐싱리스트](http://yaejinha.github.io//assets/img/cashing.JPG)

3) rc parameter 에 font-family 지정    
~~~python
plt.rcParams["font.family"] = 'NanumGothicOTF' //fontlist.json 에 캐싱된 한글 폰트 
mpl.rcParams['axes.unicode_minus'] = False //마이너스 축 안깨지게 하기 
~~~ 

