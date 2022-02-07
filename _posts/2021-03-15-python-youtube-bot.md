---
layout: post
title: 파이썬+셀레니움으로 멜론 플레이리스트 유투브로 옮기는 봇 만들기 
date:  2021-03-15 12:30
img: dongdong_music.jpg # Add image post (optional)
categories: [python]
tags: [IT, 파이썬, selenium, beatifulsoup, 셀레니움, 자동화] # add tag
sitemap :
changefreq : daily
priority : 1.0
---
  
우연히 김혜리의 필름클럽에 나온 노래를 모아 놓은 멜론 플레이 리스트를 발견했다.   
유투브 뮤직 유저인데, 쫌쫌따리 옮겨오는건 귀찮잖아요,,,   
크롤링만 할 수 있다면 금방 짤 수 있을거 같아서 시작했는데 생각보다 난관이 많았다..   

* 셀레니움 이란 ? 
무료 자동화 테스트 툴로,  html 개체 잡아서 원격 제어하는 용도로 사용한다. 

## to do 
1. 멜론의  플레이리스트 긁어오기    
2. 파싱해서 곡 제목, 가수로 정리하기
3. 유투브 로그인하기
4. 플레이 리스트에 하나씩 추가하기 

일단 풀 코드는 
https://github.com/yejinha/mini_proj/tree/main/melon2youtube
에 있다.   

* 준비물 : 크롬이나 사파리 웹드라이버 (나는 크롬 웹드라이버 썼음.)  

# 1. 멜론의  플레이리스트 긁어오기  
우선 가져오고자하는 멜론 플레이리스트를 긁어와야해서 웹 드라이버로 창 하나 열어준다.   


~~~python
    from selenium import webdriver
    driver = webdriver.Chrome('./chromedriver') # 경로가 아니라 파일명임. 확장자 없음
    driver.get(playlist_url) # 플레이리스트 화면 열기 (원하는 플레이리스트 url 넣으면 된다.)
    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser') 
~~~

일단 죽 긁어오고 파싱을 하는데 , 이때 플레이리스트 화면에서 f12로 화면 돔 구조 살펴보고 그에 맞게 짜야한다.  
멜론, 지니 등 다 범용으로 쓸 수 있게 하고 싶었으나 플랫폼마다 화면 구조가 다를테니 약간의 노가다 성이 있음...   
심지어 일반 사용자가 공유한 플레이리스트랑 dj 로 등록된 사용자가 올린 플레이리스트의 화면도 돔 구조가 달랐다.  
이때 알았어야 하는데 차라리 그냥 쫌쫌따리 치는게 더 빠르단 걸 ^^.... 



# 2. 파싱해서 곡 제목, 가수로 정리하기

f12 로 구조 파악하고 곡 제목/ 가수 부분에 셀렉터를 가져온다.   
클릭하고 copy > copy selector 클릭해서 사용.   

![예시](/assets/img/2021-03-15-python-bot/copy_selector.png)  


```python
  title = soup.select(
            '#songList > div.section_playlist > #pageList > #frm > div.tb_list > table > tbody > tr > td.t_left > div.pd_none > div.ellipsis > a.btn_icon_detail > span'
        )
        
   singer = soup.select(
             '#songList > div.section_playlist > #pageList > #frm > div.tb_list > table > tbody > tr > td.t_left > div.wrapArtistName > #artistName'
        )
```

selector 사용해서 파싱해낼 부분만 지정하면 쉽게 나온다. 


# 3. 유투브 로그인하기 & 플레이리스트 추가
솔직히 여기가 제일 오래 걸렸다.  
기존에 사용하던  계정으로 하려니까 계속 자동화 툴이어서 보안상 막혀버리는거..   
헤더에 별 난리를 쳐도 계속 걸림..  다른 구글로그인하는 사이트 (ex. 인프런) 으로 경유하려는 것도 막힘.  
여기서 솔직히 포기하고 싶었지만, 쏟은 시간이 아까웠다..  

근데 무슨 유투브 댓글에 그냥 새 계정 파서 하면 된다는거...?   
진짜 해보니까 잘 됐다.. 코드는 고친게 없는데 새 계정 파니까 되네..?   
어떤 기준으로 잡는건지.  어쨌든 이제  경유도 안되니까 테스트용 계정 하나 파서 사용하는 것 추천   

로그인 하는 기능 자체는 그냥 입력창 개체 선택 -> 입력 -> 확인버튼 개체 선택 -> 클릭 이 순이다. 

```python
    driver.get('https://www.youtube.com/');   	# 유튜브 접속
    time.sleep(15)					#  로그인 동작 시작 
    lg_menu = WebDriverWait(driver,timeout=100).until(EC.presence_of_element_located((By.XPATH,"/html/body/ytd-app/div/div/ytd-masthead/div[3]/div[3]/div[2]/ytd-button-renderer/a/paper-button/yt-formatted-string")))				# 실행합니다.
    lg_menu.click()
    idd = WebDriverWait(driver,timeout=100).until(EC.presence_of_element_located((By.NAME,'identifier') ))  
    idd.send_keys(id)
```

여기서 time.sleep 과  WebDriverWait 을 둘 다 썼다.   
원래는  time .sleep 만 가지고 다음 창이 뜨기까지 기다리도록 제어하려고 했는데 자꾸 이런 에러가 떴다.  

![에러](/assets/img/2021-03-15-python-bot/wait_error.png)  

**stale element reference : element is not attached to the page document.**

찾아보니까 동적인 제어를 하는 경우, 화면창에는 버튼이 뜬거 같아도 완전히 로드가 안되는 경우가 있나보더라.  
그래서 사용하는게 WebDriverWait -> 이걸로 element 가 뜰때까지 기다리도록 한다.  

더 자세한 사항은 [여기](https://selenium-python.readthedocs.io/api.html#selenium.webdriver.support.wait.WebDriverWait.until) 에서 확인할 수 있다. (공식 문서)  

간략히 설명하자면 타임아웃 시간을 정해놓고 특정 개체 (여기서는  액션에 필요한 버튼) 이 로드될때까지 기다리도록 명시적으로 표현할 수 있다.  

개체를 표기하는 방법은 위의 코드에서도 보이듯이 xpath, name, class 등이 가능하다.  

플레이리스트 추가하는 건  이런 개체 선택 -> 추가 버튼 클릭의 연속이었다.  

어쨌든 완성하고 나니까 뿌듯하다...  짜고 나니까 간단한데 왜 이렇게 오래 걸렸나 몰라.   

내가 이 개고생을 하게 된 [플레이리스트](https://youtube.com/playlist?list=PLDD8dLRUTmdDaZtzFWb-msmPn7NGliD3H) 이거 백번 들을거야 ㅠ...  
  
