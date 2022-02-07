---
layout: post
title: github 블로그 수정 사항 변경 방법 
date:  2019-08-10 12:36:00 +0800
img: git.jpg # Add image post (optional)
tags: [미세먼지팁] # add tag
categories: [기타]
sitemap :
changefreq : daily
priority : 1.0
---

git 다루는게 어색하기에 아주 간단한 글 수정, Config 변경에도 애를 먹었다. 

너무 기본적이긴 하지만 나같은 초보자를 위해 정리해둔다. 

# 레파지토리 복사  #

&nbsp;&nbsp;&nbsp;&nbsp;일단 repository 하나 파서 블로그를 띄우고 나면, 파일 수정이 용이하기 위해서 로컬에 복사하는 과정이 필요하다.
repository > clone or download > download zip 으로 복사 OR git bash 로 git clone 한다! 


# 파일 수정  # 
다양한 마크다운 에디터들을 이용해서 수정할 수 있지만, 나는 귀찮아서 VS code 로 수정했다.


# 파일 커밋 하기  #
파일을 수정했다면 커밋 과정을 통해서 수정된 파일을 반영해야한다. 

1. git add > 수정한 파일을 stage 에 올리기 위해 사용한다. 간단하게 **git add --all** 로 전체 올려버린다. 

     ignore 할게 없어서 그냥 그렇게 했다. git add 파일명 으로  해도 된다. 

2. git commit > 커밋을 통해 변경사항을 내 로컬에 저장한다. 아직 서버 repository 에는 반영된게 아니다.

     **git commit -m "커밋메세지"** (git commit -am "커밋메세지" 로 1의 과정 생략 가능하다. )

3. git push > 서버 repository 에 반영하기 위한 과정이다. **git push origin master **

    (git push -u origin master -u는 서버 레파지토리  업뎃 후 push >> 블로그는 그냥 혼자 쓰니까 굳이 -u 하지 않았다.)

# 반영된 모습 확인하기  #
아주 뿌듯한 과정이다 :) 히히 

왕초보인 나에게는 이렇게 간단한 과정도 참 험난했다. 

그냥 있는 에디터 사용할걸 왜 굳이 깃헙블로그 만들고 있나 현타도 왔지만 그래도 이것또한 공부라고 생각한다 !!

앞으로 열심히 정리해야지!