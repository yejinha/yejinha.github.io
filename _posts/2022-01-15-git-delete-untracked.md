---
layout: post
title: git 에서 untracked 파일 지우는 법 ! 
date:  2022-01-15 16:05
img: returnsea.jpeg # Add image post (optional)
categories: [study]
tags: [IT,  web, git] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

확실히 git 에 대한 개념을 잡고 나니  매일 얼레벌레 모든 파일을 다 올리던 과거에 비해서 나아졌다.   
변경 사항이 많아도 올려야 할 파일들만 add 해서 쓰는데, 그러다 보니 git status 해보면 untracked file 이 많다.   

이때 파일들 지우는 법 !   

```
git clean -f  
```

만약 폴더까지 지우고 싶으면
~~~
git clean -fd 
~~~~
하면 폴더까지 깔끔히 지워짐 !!   

간단하지만 유용합니당  