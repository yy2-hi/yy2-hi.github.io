---
layout: single
title: "Project 3 - Web Parsing"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## Beautiful Soup for web data

### HTML 기초
- html 태그 : 웹 페이지 표현
- head 태그 : 눈에 보이진 않지만 문서에 필요한 헤더 정보 보관
- body 태그 : 눈에 보이는 정보 보관

### Beautiful soup : 태그로 이루어진 문서를 해석하는 기능을 가진 모듈
- html.parser : Beautiful Soup의 html을 읽는 엔진 중 하나로 적당히 빠른 속도를 가지고 있다.

- lxml : xml을 사용한다면 제공받는 파서로 외부 C라이브러리에 의존하지만 속도가 빠르다.
- prettify() : html을 태그 기준으로 들여쓰기

```python
# import
from bs4 import BeautifulSoup
```
```python
page = open("../data/03. zerobase.html", "r").read()
soup = BeautifulSoup(page, "html.parser") 
print(soup.prettify())

=>

Output exceeds the size limit. Open the full output data in a text editor
<!DOCTYPE html>
<html>
 <head>
  <title>
   Very Simple HTML Code by ZeroBase
  </title>
 </head>
 <body>
  <div>
   <p class="inner-text first-item" id="first">
    Happy ZeroBase.
    <a href="https://pinkwink.kr/" id="pw-link">
     PinkWink
    </a>
   </p>
   <p class="inner-text second-item">
    Happy Data Science.
    <a href="https://www.python.org" id="py-link" target="_blank">
     Python
    </a>
   </p>
  </div>
  <p class="outer-text first-item" id="second">
   <b>
    Data Science is funny.
...
   </i>
  </p>
 </body>
</html>
```
```py
# head 태그 확인
soup.head

=>

<head>
<title>Very Simple HTML Code by ZeroBase</title>
</head>
```
```py
# body 태그 확인
soup.body

=>

<body>
<div>
<p class="inner-text first-item" id="first">
                Happy ZeroBase.
                <a href="https://pinkwink.kr/" id="pw-link">PinkWink</a>
</p>
<p class="inner-text second-item">
                Happy Data Science.
                <a href="https://www.python.org" id="py-link" target="_blank">Python</a>
</p>
</div>
<p class="outer-text first-item" id="second">
<b>Data Science is funny.</b>
</p>
<p class="outer-text">
<i>All I need is Love.</i>
</p>
</body>
```
```py
# p 태그 확인
# 처음 발견한 p 태그만 출력
# find()
soup.p
# soup.find("p")

=>

<p class="inner-text first-item" id="first">
                Happy ZeroBase.
                <a href="https://pinkwink.kr/" id="pw-link">PinkWink</a>
</p>
```
```py
# 파이썬 예약어
# class, id, def, list, str, int, tuple, ...
# -> class_, id_, ...
soup.find("p", class_="inner-text second-item")

=>

<p class="inner-text second-item">
                Happy Data Science.
                <a href="https://www.python.org" id="py-link" target="_blank">Python</a>
</p>
```
```py
soup.find("p", {"class":"outer-text first-item"}).text

=>

'\nData Science is funny.\n'
----------------------------------------------------------------
soup.find("p", {"class":"outer-text first-item"}).text.strip()

=>

'Data Science is funny.'
```
```py
# 다중 조건
soup.find("p", {"class":"inner-text first-item", "id":"first"})

=>

<p class="inner-text first-item" id="first">
                Happy ZeroBase.
                <a href="https://pinkwink.kr/" id="pw-link">PinkWink</a>
</p>
```
```py
# find_all(): 여러 개의 태그를 반환
# list 형태로 반환

soup.find_all("p")

=>

[<p class="inner-text first-item" id="first">
                 Happy ZeroBase.
                 <a href="https://pinkwink.kr/" id="pw-link">PinkWink</a>
 </p>,
 <p class="inner-text second-item">
                 Happy Data Science.
                 <a href="https://www.python.org" id="py-link" target="_blank">Python</a>
 </p>,
 <p class="outer-text first-item" id="second">
 <b>Data Science is funny.</b>
 </p>,
 <p class="outer-text">
 <i>All I need is Love.</i>
 </p>]
```
```py
# 특정 태그 확인
soup.find_all(id="pw-link")[0].text

=>

'PinkWink'
```
```py
soup.find_all('p', class_="inner-text second-item")

=>

[<p class="inner-text second-item">
                 Happy Data Science.
                 <a href="https://www.python.org" id="py-link" target="_blank">Python</a>
 </p>]
 ```
 ```py
 len(soup.find_all("p"))
 
 =>
 
 4
 ```
 ```py
 print(soup.find_all("p")[0].text)
print(soup.find_all("p")[1].string)
print(soup.find_all("p")[1].get_text())

=>

                Happy ZeroBase.
                PinkWink

None

                Happy Data Science.
                Python

 ```
 ```py
 # p 태그 리스트에서 텍스트 속성만 출력

for each_tag in soup.find_all("p"):
    print("=" * 50)
    print(each_tag.text)
    
=>

==================================================

                Happy ZeroBase.
                PinkWink

==================================================

                Happy Data Science.
                Python

==================================================

Data Science is funny.

==================================================

All I need is Love.
 ```
 ```py
 # a 태그에서 href 속성값에 있는 값 추출
links = soup.find_all("a")
links[0].get("href"), links[1]["href"]

=>

('https://pinkwink.kr/', 'https://www.python.org')
```
```py
for each in links:
    href = each.get("href") # each["href"]
    text = each.get_text()
    print(text + "=>" + href)
    
=>

PinkWink=>https://pinkwink.kr/
Python=>https://www.python.org
```

## BeautifulSoup 예제 1-1 네이버 금융
```py
# import
from urllib.request import urlopen
from bs4 import BeautifulSoup
```
```py
url = "https://finance.naver.com/marketindex/"
page = urlopen(url)
# response = urlopen(url)
# response
soup = BeautifulSoup(page, "html.parser")
print(soup.prettify())

=>

<script language="javascript" src="/template/head_js.naver?referer=info.finance.naver.com&amp;menu=marketindex&amp;submenu=market">
</script>
<script src="https://ssl.pstatic.net/imgstock/static.pc/20230109162011/js/info/jindo.min.ns.1.5.3.euckr.js" type="text/javascript">
</script>
<script src="https://ssl.pstatic.net/imgstock/static.pc/20230109162011/js/jindo.1.5.3.element-text-patch.js" type="text/javascript">
</script>
<div id="container" style="padding-bottom:0px;">
 <div class="market_include">
  <div class="market_data">
   <div class="market1">
    <div class="title">
     <h2 class="h_market1">
      <span>
       ШЏРќ АэНУ ШЏРВ
      </span>
     </h2>
    </div>
    <!-- data -->
    <div class="data">
     <ul class="data_lst" id="exchangeList">
      <li class="on">
       <a class="head usd" href="/marketindex/exchangeDetail.naver?marketindexCd=FX_USDKRW" onclick="clickcr(this, 'fr1.usdt', '', '', event);">
        <h3 class="h_lst">
         <span class="blind">
          ЙЬБЙ USD
...
	window.addEventListener('mousedown', gnbLayerClose);
}
</script>
```
```py
# 1
soup.find_all("span", "value"), len(soup.find_all("span", "value"))
# 2
soup.find_all("span", class_="value"), len(soup.find_all("span", class_="value"))
# 3
soup.find_all("span", {"class":"value"}), len(soup.find_all("span", {"class":"value"}))

=>

([<span class="value">1,228.00</span>,
  <span class="value">945.16</span>,
  <span class="value">1,339.50</span>,
  <span class="value">181.85</span>,
  <span class="value">129.8400</span>,
  <span class="value">1.0867</span>,
  <span class="value">1.2396</span>,
  <span class="value">101.7200</span>,
  <span class="value">79.68</span>,
  <span class="value">1574.35</span>,
  <span class="value">1929.4</span>,
  <span class="value">76030.75</span>],
 12)
```
```py
#1
soup.find_all("span", {"class":"value"})[0].text
#2
soup.find_all("span", {"class":"value"})[0].string
#3
soup.find_all("span", {"class":"value"})[0].get_text()

=>

'1,228.00'
```

## BeautifulSoup 예제 1-2 네이버 금융
- !pip install requests
- find, find_all
- select, select_one
- find, select_one : 단일 선택
- select, find_all : 다중 선택

```py
import requests
# from urllib.request.Request
from bs4 import BeautifulSoup
import pandas as pd
```
```py
url = "https://finance.naver.com/marketindex/"
response = requests.get(url)
# requests.get(), requests.post()
# response.text
soup = BeautifulSoup(response.text, "html.parser")
print(soup.prettify())

=>

<script language="javascript" src="/template/head_js.naver?referer=info.finance.naver.com&amp;menu=marketindex&amp;submenu=market">
</script>
<script src="https://ssl.pstatic.net/imgstock/static.pc/20230109162011/js/info/jindo.min.ns.1.5.3.euckr.js" type="text/javascript">
</script>
<script src="https://ssl.pstatic.net/imgstock/static.pc/20230109162011/js/jindo.1.5.3.element-text-patch.js" type="text/javascript">
</script>
<div id="container" style="padding-bottom:0px;">
 <div class="market_include">
  <div class="market_data">
   <div class="market1">
    <div class="title">
     <h2 class="h_market1">
      <span>
       환전 고시 환율
      </span>
     </h2>
    </div>
    <!-- data -->
    <div class="data">
     <ul class="data_lst" id="exchangeList">
      <li class="on">
       <a class="head usd" href="/marketindex/exchangeDetail.naver?marketindexCd=FX_USDKRW" onclick="clickcr(this, 'fr1.usdt', '', '', event);">
        <h3 class="h_lst">
         <span class="blind">
          미국 USD
...
	window.addEventListener('mousedown', gnbLayerClose);
}
</script>
```
```py
# soup.find_all("li", "on")
# id => #
# class => .
exchangeList = soup.select("#exchangeList > li")
len(exchangeList), exchangeList

=>

(4,
 [<li class="on">
  <a class="head usd" href="/marketindex/exchangeDetail.naver?marketindexCd=FX_USDKRW" onclick="clickcr(this, 'fr1.usdt', '', '', event);">
  <h3 class="h_lst"><span class="blind">미국 USD</span></h3>
  <div class="head_info point_dn">
  <span class="value">1,228.50</span>
  <span class="txt_krw"><span class="blind">원</span></span>
  <span class="change"> 6.50</span>
  <span class="blind">하락</span>
  </div>
  </a>
  <a class="graph_img" href="/marketindex/exchangeDetail.naver?marketindexCd=FX_USDKRW" onclick="clickcr(this, 'fr1.usdc', '', '', event);">
  <img alt="" height="153" src="https://ssl.pstatic.net/imgfinance/chart/marketindex/FX_USDKRW.png" width="295"/>
  </a>
  <div class="graph_info">
  <span class="time">2023.01.30 16:33</span>
  <span class="source">하나은행 기준</span>
  <span class="count">고시회차<span class="num">315</span>회</span>
  </div>
  </li>,
  <li class="">
  <a class="head jpy" href="/marketindex/exchangeDetail.naver?marketindexCd=FX_JPYKRW" onclick="clickcr(this, 'fr1.jpyt', '', '', event);">
  <h3 class="h_lst"><span class="blind">일본 JPY(100엔)</span></h3>
  <div class="head_info point_dn">
  <span class="value">948.10</span>
...
  <span class="time">2023.01.30 16:33</span>
  <span class="source">하나은행 기준</span>
  <span class="count">고시회차<span class="num">315</span>회</span>
  </div>
  </li>])
```
```py
title = exchangeList[0].select_one(".h_lst").text
exchange = exchangeList[0].select_one(".value").text
change = exchangeList[0].select_one(".change").text
updown = exchangeList[0].select_one("div.head_info.point_dn > .blind").text

baseUrl = "https://finance.naver.com"
link = baseUrl + exchangeList[0].select_one("a").get("href")

title, exchange, change, updown, link

=>

('미국 USD',
 '1,228.50',
 ' 6.50',
 '하락',
 'https://finance.naver.com/marketindex/exchangeDetail.naver?marketindexCd=FX_USDKRW')
```
```py
# 4개 데이터 수집

exchange_datas = []
baseUrl = "https://finance.naver.com"

for item in exchangeList:
    data = {
        "title": item.select_one(".h_lst").text,
        "exchange": item.select_one(".value").text,
        "change": item.select_one(".change").text,
        "updown": exchangeList[0].select_one("div.head_info.point_dn > .blind").text,
        "link": baseUrl + item.select_one("a").get("href")
    }
    exchange_datas.append(data)
df = pd.DataFrame(exchange_datas)
df.to_excel("./naverfinance.xlsx", encoding="utf-8")
```

## BeautifulSoup 예제 2 - 위키백과 문서 정보 가져오기
```py
import urllib
from urllib.request import urlopen, Request

html ="https://ko.wikipedia.org/wiki/{search_words}"
# https://ko.wikipedia.org/wiki/여명의_눈동자
req = Request(html.format(search_words=urllib.parse.quote("여명의_눈동자"))) # 글자를 url로 인코딩
response = urlopen(req)
soup = BeautifulSoup(response, "html.parser")
print(soup.prettify())

=>

<!DOCTYPE html>
<html class="client-nojs" dir="ltr" lang="ko">
 <head>
  <meta charset="utf-8"/>
  <title>
   여명의 눈동자 - 위키백과, 우리 모두의 백과사전
  </title>
  <script>
   document.documentElement.className="client-js";RLCONF=
...
   (RLQ=window.RLQ||[]).push(function(){mw.config.set({"wgBackendResponseTime":103,"wgHostname":"mw2325"});});
  </script>
 </body>
</html>
```
```py
n = 0
for each in soup.find_all("ul"):
    print('=>' + str(n) + "--------------------------------------")
    print(each.get_text())
    n += 1
    
=>

=>0--------------------------------------
계정 만들기
=>1--------------------------------------
계정 만들기
로그인

=>2--------------------------------------
토론기여
=>3--------------------------------------
대문최근 바뀜요즘 화제임의의 문서로기부
=>4--------------------------------------
사랑방사용자 모임관리 요청
=>5--------------------------------------
도움말정책과 지침질문방
=>6--------------------------------------
여기를 가리키는 문서가리키는 글의 최근 바뀜파일 올리기특수 문서 목록고유 링크문서 정보이 문서 인용하기위키데이터 항목
=>7--------------------------------------
책 만들기PDF로 다운로드인쇄용 판
=>8--------------------------------------



처음 위치

...
```
```py
soup.find_all("ul")[32].text.strip().replace("\xa0", "").replace("\n", "")

=>

'채시라: 윤여옥 역 (아역: 김민정)박상원: 장하림(하리모토 나츠오) 역 (아역: 김태진)최재성: 최대치(사카이) 역 (아역: 장덕수)'
```