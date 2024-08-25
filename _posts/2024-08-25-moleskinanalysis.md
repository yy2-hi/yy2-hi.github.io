---
layout: single
title: "Project 7 - Naver API에서 모은 몰스킨 데이터 분석"
categories: Data Analysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 1. 네이버 API 사용 등록
- 네이버 개발자 센터
- https://developers.naver.com/main/
- Application
    - 어플리케이션 등록
    - 어플리케이션 이름
    - 사용 API
        - 검색
    - 환경추가
        - WEB 설정
        - http://localhost
    - Client ID : **
    - Client Secret: **
    - https://developers.naver.com/apps/#/myapps/E_N2j6ER9uWLIDb2BEEc/overview
    
## 2. 네이버 검색 API 사용   
- urllib: http 프로토콜에 따라서 서버의 요청/응답을 처리하기 위한 모듈
- urllib.request: 클라이언트의 요청을 처리하는 모듈
- urllib.parse: url 주소에 대한 분석
#### 개발 가이드
- https://developers.naver.com/docs/serviceapi/search/blog/blog.md#python

### 검색: 블로그(blog)
```py
# 네이버 검색 API 예제 - 블로그 검색
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("파이썬")
url = "https://openapi.naver.com/v1/search/blog?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

"title":"<b>파이썬<\/b>학원 초보자를 위한 공부과정!",
			"link":"https:\/\/blog.naver.com\/chzhvkdll\/222931446581",
			"description":"그래서 제가 <b>파이썬<\/b>학원을 다닌 계기과 장단점, 고민에 대해 써보도록 하겠습니다 :) 모두가 같을 수는... <b>파이썬<\/b>학원 수강 신청한 건 대학 후배 중에 갑자기 전공 무관하게 그쪽으로 턴해서 공부하고 취직했단... ",
			"bloggername":"에피",
			"bloggerlink":"blog.naver.com\/chzhvkdll",
			"postdate":"20221118"
            								.
                                            .
                                            .
```
---
```py
response, response.getcode(), response.code, response.status

=>

(<http.client.HTTPResponse at 0x25e7c01c190>, 200, 200, 200)
```

---

```py
# 글자로 읽을 경우, decode utf-8 설정
print(response_body.decode("utf-8"))

=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:40 +0900",
	"total":384417,
	"start":1,
	"display":10,
	"items":[
		{
			"title":"<b>파이썬<\/b>학원 초보자를 위한 공부과정!",
			"link":"https:\/\/blog.naver.com\/chzhvkdll\/222931446581",
			"description":"그래서 제가 <b>파이썬<\/b>학원을 다닌 계기과 장단점, 고민에 대해 써보도록 하겠습니다 :) 모두가 같을 수는... <b>파이썬<\/b>학원 수강 신청한 건 대학 후배 중에 갑자기 전공 무관하게 그쪽으로 턴해서 공부하고 취직했단... ",
			"bloggername":"에피",
			"bloggerlink":"blog.naver.com\/chzhvkdll",
			"postdate":"20221118"
		},
        									.
                                            .
                                            .
```

---
### 검색: 책(book)
```py
# 네이버 검색 API 예제 - 책 검색
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("파이썬")
url = "https://openapi.naver.com/v1/search/book?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:40 +0900",
	"total":826,
	"start":1,
	"display":10,
	"items":[
		{
			"title":"혼자 공부하는 파이썬 (1:1 과외하듯 배우는 프로그래밍 자습서)",
			"link":"https:\/\/search.shopping.naver.com\/book\/catalog\/32507605957",
			"image":"https:\/\/shopping-phinf.pstatic.net\/main_3250760\/32507605957.20221019133018.jpg",
			"author":"윤인성",
			"discount":"19800",
			"publisher":"한빛미디어",
			"pubdate":"20220601",
			"isbn":"9791162245651",
            							.
                                        .
                                        .
```

---

### 검색: 영화(movie)
```py
# 네이버 검색 API 예제 - 영화 검색
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("파이썬")
url = "https://openapi.naver.com/v1/search/movie?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:40 +0900",
	"total":1,
	"start":1,
	"display":1,
	"items":[
		{
			"title":"<b>파이썬<\/b> 앤 가드",
			"link":"https:\/\/movie.naver.com\/movie\/bi\/mi\/basic.nhn?code=152070",
			"image":"https:\/\/ssl.pstatic.net\/imgmovie\/mdi\/mit110\/1520\/152070_P01_145336.jpg",
			"subtitle":"PYTHON AND GUARD",
			"pubDate":"2015",
			"director":"안톤 디아코프|",
			"actor":"",
			"userRating":"0.00"
		}
        									.
                                            .
                                            .
```
---

### 검색: 카페(cafearticle)
```py
# 네이버 검색 API 예제 - 카페 검색
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("파이썬")
url = "https://openapi.naver.com/v1/search/cafearticle?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:41 +0900",
	"total":156167,
	"start":1,
	"display":10,
	"items":[
		{
			"title":"<b>파이썬<\/b> vs C언어",
			"link":"http:\/\/cafe.naver.com\/mathall\/2564974",
			"description":"고수님들의 현명한 답변 기다립니다 <b>파이썬<\/b>을 가르친다는 학원은 저희 집에서 차로 15분 거리라서 제가... 제가 라이딩이 힘든 건 아니니 그건 감안하고 <b>파이썬<\/b> vs C언어 중 어느 학원을 선택해야 아이한테 도움이 될까요?",
			"cafename":"[상위1%카페] 대한민국 상위1% 교육정...",
			"cafeurl":"https:\/\/cafe.naver.com\/mathall"
		},
        								.
                                        .
                                        .
```
---
### 검색: 쇼핑(shop)
```py
# 네이버 검색 API 예제 - 쇼핑 검색
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("파이썬")
url = "https://openapi.naver.com/v1/search/shop?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:41 +0900",
	"total":148244,
	"start":1,
	"display":10,
	"items":[
		{
			"title":"잘모이 셀리나 리얼 <b>파이톤<\/b> 뉴 빅 토트백 ZA-4022",
			"link":"https:\/\/search.shopping.naver.com\/gate.nhn?id=35683981989",
			"image":"https:\/\/shopping-phinf.pstatic.net\/main_3568398\/35683981989.20221107081401.jpg",
			"lprice":"165530",
			"hprice":"",
			"mallName":"네이버",
			"productId":"35683981989",
			"productType":"1",
			"brand":"잘모이",
			"maker":"",
			"category1":"패션잡화",
			"category2":"여성가방",
			"category3":"토트백",
			"category4":""
		},
        								.
                                        .
                                        .
```

---

### 검색: 백과사전(encyc)
```py
# 네이버 검색 API 예제 - 백과사전 검색
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("파이썬")
url = "https://openapi.naver.com/v1/search/encyc?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:41 +0900",
	"total":522,
	"start":1,
	"display":10,
	"items":[
		{
			"title":"<b>파이썬<\/b>",
			"link":"https:\/\/terms.naver.com\/entry.naver?docId=3580815&cid=59088&categoryId=59096",
			"description":"‘<b>파이썬<\/b>’이다. 간결한 문법으로 입문자가 이해하기 쉽고, 다양한 분야에 활용할 수 있기 때문이다. 이 외에도 <b>파이썬<\/b>은 머신러닝, 그래픽, 웹 개발 등 여러 업계에서 선호하는 언어로 꾸준히... ",
			"thumbnail":"http:\/\/openapi-dbscthumb.phinf.naver.net\/4749_000_1\/20170118193349632_0CHSSS5Y6.png\/01_16.png?type=m160_160"
		},
        						.
                                .
                                .
```

---

## 3. 상품 검색
- "몰스킨"
```py
import os
import sys
import urllib.request
client_id = "E_N2j6ER9uWLIDb2BEEc"
client_secret = "OwQUT_S108"
encText = urllib.parse.quote("몰스킨")
url = "https://openapi.naver.com/v1/search/shop?query=" + encText # JSON 결과
# url = "https://openapi.naver.com/v1/search/blog.xml?query=" + encText # XML 결과
request = urllib.request.Request(url)
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
response = urllib.request.urlopen(request)
rescode = response.getcode()
if(rescode==200):
    response_body = response.read()
    print(response_body.decode('utf-8'))
else:
    print("Error Code:" + rescode)
    
=>

{
	"lastBuildDate":"Thu, 02 Feb 2023 18:25:41 +0900",
	"total":43102,
	"start":1,
	"display":10,
	"items":[
		{
			"title":"<b>몰스킨<\/b> 2023 다이어리 위클리 소프트커버",
			"link":"https:\/\/search.shopping.naver.com\/gate.nhn?id=84662525433",
			"image":"https:\/\/shopping-phinf.pstatic.net\/main_8466252\/84662525433.2.jpg",
			"lprice":"22950",
			"hprice":"",
			"mallName":"베스트펜",
			"productId":"84662525433",
			"productType":"2",
			"brand":"몰스킨",
			"maker":"",
			"category1":"생활\/건강",
			"category2":"문구\/사무용품",
			"category3":"다이어리\/플래너",
			"category4":"다이어리"
		},
        						.
                                .
                                .
```

---

### gen_search_url()
<span style="color: #FAFAD2">encText = urllib.parse.quote("몰스킨")
url = "https://openapi.naver.com/v1/search/shop?query=" + encText # JSON 결과</span>
```py
def gen_search_url(api_node, search_text, start_num, disp_num):
    base = "https://openapi.naver.com/v1/search"
    node = "/" + api_node + ".json"
    param_query = "?query=" + urllib.parse.quote(search_text)
    param_start = "&start=" + str(start_num)
    param_disp = "&display=" + str(disp_num)
    
    return base + node + param_query + param_start + param_disp
    
gen_search_url("shop", "TEST", 10, 3)

=>

'https://openapi.naver.com/v1/search/shop.json?query=TEST&start=10&display=3'
```

---

### get_result_onpage()
```py
import json
import datetime

def get_result_onpage(url):
    request = urllib.request.Request(url)
    request.add_header("X-Naver-Client-Id",client_id)
    request.add_header("X-Naver-Client-Secret",client_secret)
    response = urllib.request.urlopen(request)
    print("[%s] Url Request Success" % datetime.datetime.now())
    return json.loads(response.read().decode("utf-8"))
    
url = gen_search_url("shop", "몰스킨", 1, 5)
one_result = get_result_onpage(url)

=>

[2023-02-02 18:25:42.249704] Url Request Success
```

---

```py
one_result

=>

{'lastBuildDate': 'Thu, 02 Feb 2023 18:25:42 +0900',
 'total': 43102,
 'start': 1,
 'display': 5,
 'items': [{'title': '<b>몰스킨</b> 2023 다이어리 위클리 소프트커버',
   'link': 'https://search.shopping.naver.com/gate.nhn?id=84662525433',
   'image': 'https://shopping-phinf.pstatic.net/main_8466252/84662525433.2.jpg',
   'lprice': '22950',
   'hprice': '',
   'mallName': '베스트펜',
   'productId': '84662525433',
   'productType': '2',
   'brand': '몰스킨',
   'maker': '',
   'category1': '생활/건강',
   'category2': '문구/사무용품',
   'category3': '다이어리/플래너',
   'category4': '다이어리'},
   								.
                                .
                                .
```

---

### get_fields()
```py
import pandas as pd

def get_fields(json_data):
    title = [each["title"] for each in json_data["items"]]
    link = [each["link"] for each in json_data["items"]]
    lprice = [each["lprice"] for each in json_data["items"]]
    mall_name = [each["mallName"] for each in json_data["items"]]
    
    result_pd = pd.DataFrame({
        "title": title,
        "link": link,
        "lprice": lprice,
        "mall": mall_name,
    }, columns=["title", "lprice", "link", "mall"])
    return result_pd
    
get_fields(one_result)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>title</th>
      <th>lprice</th>
      <th>link</th>
      <th>mall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;b&gt;몰스킨&lt;/b&gt; 2023 다이어리 위클리 소프트커버</td>
      <td>22950</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>베스트펜</td>
    </tr>
    <tr>
      <th>1</th>
      <td>&lt;b&gt;몰스킨&lt;/b&gt; 2023 데일리 12개월 다이어리 L</td>
      <td>38030</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>2</th>
      <td>&lt;b&gt;몰스킨&lt;/b&gt; 노트 가죽 하드커버 감성 고급 업무용 이쁜 심플</td>
      <td>24000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>베스트펜</td>
    </tr>
    <tr>
      <th>3</th>
      <td>&lt;b&gt;몰스킨&lt;/b&gt; 2023다이어리 데일리 하드커버 라지블루 다이어리노트</td>
      <td>31000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[&lt;b&gt;몰스킨&lt;/b&gt;] 2023년 클래식 다이어리(12개월) (데일리, 위클리, 먼슬리)</td>
      <td>27000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>몰스킨공식온라인스토어</td>
    </tr>
  </tbody>
</table>

---

### delete_tag()

```py
def delete_tag(input_str):
    input_str = input_str.replace("<b>", "")
    input_str = input_str.replace("</b>", "")
    return input_str
    
import pandas as pd

def get_fields(json_data):
    title = [delete_tag(each["title"]) for each in json_data["items"]]
    link = [each["link"] for each in json_data["items"]]
    lprice = [each["lprice"] for each in json_data["items"]]
    mall_name = [each["mallName"] for each in json_data["items"]]
    
    result_pd = pd.DataFrame({
        "title": title,
        "link": link,
        "lprice": lprice,
        "mall": mall_name,
    }, columns=["title", "lprice", "link", "mall"])
    return result_pd
    
get_fields(one_result)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>title</th>
      <th>lprice</th>
      <th>link</th>
      <th>mall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>몰스킨 2023 다이어리 위클리 소프트커버</td>
      <td>22950</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>베스트펜</td>
    </tr>
    <tr>
      <th>1</th>
      <td>몰스킨 2023 데일리 12개월 다이어리 L</td>
      <td>38030</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>2</th>
      <td>몰스킨 노트 가죽 하드커버 감성 고급 업무용 이쁜 심플</td>
      <td>24000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>베스트펜</td>
    </tr>
    <tr>
      <th>3</th>
      <td>몰스킨 2023다이어리 데일리 하드커버 라지블루 다이어리노트</td>
      <td>31000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[몰스킨] 2023년 클래식 다이어리(12개월) (데일리, 위클리, 먼슬리)</td>
      <td>27000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>몰스킨공식온라인스토어</td>
    </tr>
  </tbody>
</table>

---

```py
url = gen_search_url("shop", "몰스킨", 1, 5)
json_result = get_result_onpage(url)
pd_result = get_fields(json_result)

=>

[2023-02-02 18:48:57.156929] Url Request Success
```

---

```py
pd_result
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>title</th>
      <th>lprice</th>
      <th>link</th>
      <th>mall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>몰스킨 2023 다이어리 위클리 소프트커버</td>
      <td>22950</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>베스트펜</td>
    </tr>
    <tr>
      <th>1</th>
      <td>몰스킨 2023 데일리 12개월 다이어리 L</td>
      <td>38030</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>2</th>
      <td>몰스킨 노트 가죽 하드커버 감성 고급 업무용 이쁜 심플</td>
      <td>24000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>베스트펜</td>
    </tr>
    <tr>
      <th>3</th>
      <td>몰스킨 2023다이어리 데일리 하드커버 라지블루 다이어리노트</td>
      <td>31000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[몰스킨] 2023년 클래식 다이어리(12개월) (데일리, 위클리, 먼슬리)</td>
      <td>27000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>몰스킨공식온라인스토어</td>
    </tr>
  </tbody>
</table>

---

### actMain()
```py
result_mol = []

for n in range(1, 1000, 100):
    url = gen_search_url("shop", "몰스킨", n, 100)
    json_result = get_result_onpage(url)
    pd_result = get_fields(json_result)

    result_mol.append(pd_result)

result_mol = pd.concat(result_mol)

result_mol.info()

=>

<class 'pandas.core.frame.DataFrame'>
Int64Index: 1000 entries, 0 to 99
Data columns (total 4 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   title   1000 non-null   object
 1   lprice  1000 non-null   object
 2   link    1000 non-null   object
 3   mall    1000 non-null   object
dtypes: object(4)
memory usage: 39.1+ KB
```

---
#### 인덱스 재정렬, object -> float

```py
result_mol.reset_index(drop=True, inplace=True)
result_mol["lprice"] = result_mol["lprice"].astype("float")
result_mol.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 4 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   title   1000 non-null   object 
 1   lprice  1000 non-null   float64
 2   link    1000 non-null   object 
 3   mall    1000 non-null   object 
dtypes: float64(1), object(3)
memory usage: 31.4+ KB
```

---

```py
result_mol.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>title</th>
      <th>lprice</th>
      <th>link</th>
      <th>mall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>995</th>
      <td>몰스킨 까이에 룰드 라지 사이즈 옵션1</td>
      <td>20000</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>996</th>
      <td>갤럭시 갤럭시 몰스킨 다잉 팬츠 GA1821U22P</td>
      <td>116630</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>997</th>
      <td>몰스킨 Moleskine 클래식 2023 데일리 플래너 하드 커버 라지 12 x 스칼렛</td>
      <td>29800</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
    <tr>
      <th>998</th>
      <td>몰스킨 기프트박스 트래블 - Travel Journal+Luggage Tags</td>
      <td>73700</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>몰스킨스토어</td>
    </tr>
    <tr>
      <th>999</th>
      <td>올젠 몰스킨 워싱 스트레치 자켓 편한 착장 고급스러운 연출 ZOC3KG1312</td>
      <td>230910</td>
      <td>https://search.shopping.naver.com/gate.nhn?id=...</td>
      <td>네이버</td>
    </tr>
  </tbody>
</table>

---

### to_excel()
```py
writer = pd.ExcelWriter("../data/06_molskin_diary_in_naver_shop.xlsx", engine="xlsxwriter")
result_mol.to_excel(writer, sheet_name="Sheet1")

workbook = writer.book
worksheet = writer.sheets["Sheet1"]
worksheet.set_column("A:A", 4)
worksheet.set_column("B:B", 80)
worksheet.set_column("C:C", 7)
worksheet.set_column("D:D", 10)
worksheet.set_column("E:E", 40)
worksheet.set_column("F:F", 10)

worksheet.conditional_format("C2:C1001", {"type": "3_color_scale"})
writer.save()
```
![](https://velog.velcdn.com/images/yy2hi/post/18b6cf77-5d17-439b-8ffe-bbd4e7b61eee/image.png)

---

### 시각화

```py
import set_matplotlib_hangul
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(15, 6))
sns.countplot(
    x=result_mol["mall"], 
    data=result_mol, 
    palette="RdYlGn",
    order=result_mol["mall"].value_counts().index
)
plt.xticks(rotation=90)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/125d018e-9fdf-4a9f-8aa9-6dc4b5d18adc/image.png)

