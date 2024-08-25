---
layout: single
title: "Project 5 - 네이버 영화 평점 사이트 분석"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# Naver Movie Ranking

## 1. 사이트 분석
- https://movie.naver.com/
- 영화랭킹 탭 이동
- 영화랭킹에서 평점순(현재상영영화) 선택
![](https://velog.velcdn.com/images/yy2hi/post/76ffc6f5-55bb-40bf-ad84-ada06022aded/image.png)
- 주소의 규칙 발견 -> 날짜 정보를 변경해주면 해당 페이지 접근 가능

#### requirements
```py
# requirements

import pandas as pd
from urllib.request import urlopen
from bs4 import BeautifulSoup
```
```py
url = "https://movie.naver.com/movie/sdb/rank/rmovie.naver?sel=cur&date=20230130"
response = urlopen(url)
# response.status => 200
soup = BeautifulSoup(response, "html.parser")
print(soup.prettify())

=>

<!DOCTYPE html>
<html lang="ko">
 <head>
  <meta content="text/html; charset=utf-8" http-equiv="Content-Type"/>
  <meta content="IE=edge" http-equiv="X-UA-Compatible"/>
  <meta content="http://imgmovie.naver.com/today/naverme/naverme_profile.jpg" property="me2:image">
   <meta content="네이버영화 " property="me2:post_tag">
    <meta content="네이버영화" property="me2:category1"/>
   
...
  </div>
 </body>
</html>
```

---

#### 영화 제목 태그
```py
soup.find_all("div", "tit5") # soup.select("div.tit5")

=>

[<div class="tit5">
 <a href="/movie/bi/mi/basic.naver?code=223800" title="더 퍼스트 슬램덩크">더 퍼스트 슬램덩크</a>
 </div>,
 <div class="tit5">
 <a href="/movie/bi/mi/basic.naver?code=130850" title="주토피아">주토피아</a>
 </div>,
 <div class="tit5">
 <a href="/movie/bi/mi/basic.naver?code=10001" title="시네마 천국">시네마 천국</a>
 </div>,
...
 <a href="/movie/bi/mi/basic.naver?code=184513" title="교섭">교섭</a>
 </div>,
 <div class="tit5">
 <a href="/movie/bi/mi/basic.naver?code=212099" title="강남좀비">강남좀비</a>
 </div>]
```
```py
soup.find_all("div", "tit5")[0].a.string
# soup.select("div.tit5")[0].find("a").text
# soup.select(".tit5")[0].select_one("a").get_text()

=>

'더 퍼스트 슬램덩크'
```

---

#### 영화 평점 태그
```py
soup.find_all("td", "point") # soup.select(".point")

=>

[<td class="point">9.50</td>,
 <td class="point">9.36</td>,
...
 <td class="point">7.12</td>,
 <td class="point">6.88</td>,
 <td class="point">6.82</td>,
 <td class="point">6.14</td>,
 <td class="point">2.83</td>]
```

```py
len(soup.find_all("td", "point")), len(soup.find_all("div", "tit5"))

=>

(47, 47)
```
```py
soup.find_all("td", class_="point")[0].text # soup.select("td.point")[0].string

=>

'9.50'
```

---

#### 영화제목 리스트
```py
end = len(soup.find_all("div", "tit5"))

movie_name = []

# for n in range(0, end):
#     movie_name.append(
#         soup.find_all("div", "tit5")[n].a.string
#     )
movie_name = [soup.select(".tit5")[n].a.text for n in range(0, end)]

movie_name

=>

['더 퍼스트 슬램덩크',
 '주토피아',
 '시네마 천국',
...
 '더 메뉴',
 '유령',
 '원피스 필름 레드',
 '교섭',
 '강남좀비']
```
---

#### 영화평점 리스트
```py
end = len(soup.find_all("td", "point"))

movie_point = [soup.find_all("td", "point")[n].string for n in range(0, end)]
movie_point

=>

['9.50',
 '9.36',
 '9.33',
 '9.33',
 '9.32',
...
 '7.12',
 '6.88',
 '6.82',
 '6.14',
 '2.83']
```

---

#### 전체 데이터 수 확인
```py
len(movie_name), len(movie_point)

=>

(47, 47)
```

---

## 2. 자동화를 위한 코드
- 날짜 변경을 통해 원하는 기간만큼 데이터 추출
```py
date = pd.date_range("2022.10.23", periods=100, freq="D")
date

=>

DatetimeIndex(['2022-10-23', '2022-10-24', '2022-10-25', '2022-10-26',
               '2022-10-27', '2022-10-28', '2022-10-29', '2022-10-30',
                 							.
                                            .
                                            .
               '2023-01-23', '2023-01-24', '2023-01-25', '2023-01-26',
               '2023-01-27', '2023-01-28', '2023-01-29', '2023-01-30'],
              dtype='datetime64[ns]', freq='D')
```

```py
import time
from tqdm import tqdm

movie_date = []
movie_name = []
movie_point = []

for today in tqdm(date):
    url = "https://movie.naver.com/movie/sdb/rank/rmovie.naver?sel=cur&date={date}"
    response = urlopen(url.format(date=today.strftime("%Y%m%d")))
    soup = BeautifulSoup(response, "html.parser")

    end = len(soup.find_all("td", "point"))

    movie_date.extend([today for _ in range(end)])
    movie_name.extend([soup.select("div.tit5")[n].find("a").get_text() for n in range(end)])
    movie_point.extend([soup.find_all("td", "point")[n].string for n in range(end)])

    time.sleep(0.5)
    
=>

100%|██████████| 100/100 [02:21<00:00,  1.42s/it]
```
```py
movie = pd.DataFrame({
    "date": movie_date,
    "name": movie_name,
    "point": movie_point
})
movie.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>name</th>
      <th>point</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4591</th>
      <td>2023-01-30</td>
      <td>더 메뉴</td>
      <td>7.12</td>
    </tr>
    <tr>
      <th>4592</th>
      <td>2023-01-30</td>
      <td>유령</td>
      <td>6.88</td>
    </tr>
    <tr>
      <th>4593</th>
      <td>2023-01-30</td>
      <td>원피스 필름 레드</td>
      <td>6.82</td>
    </tr>
    <tr>
      <th>4594</th>
      <td>2023-01-30</td>
      <td>교섭</td>
      <td>6.14</td>
    </tr>
    <tr>
      <th>4595</th>
      <td>2023-01-30</td>
      <td>강남좀비</td>
      <td>2.83</td>
    </tr>
  </tbody>
</table>

---

```py
movie.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 4596 entries, 0 to 4595
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype         
---  ------  --------------  -----         
 0   date    4596 non-null   datetime64[ns]
 1   name    4596 non-null   object        
 2   point   4596 non-null   object        
dtypes: datetime64[ns](1), object(2)
memory usage: 107.8+ KB
```

---

```py
movie["point"] = movie["point"].astype(float)
movie.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 4596 entries, 0 to 4595
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype         
---  ------  --------------  -----         
 0   date    4596 non-null   datetime64[ns]
 1   name    4596 non-null   object        
 2   point   4596 non-null   float64       
dtypes: datetime64[ns](1), float64(1), object(1)
memory usage: 107.8+ KB
```
---

#### 데이터 저장
```py
movie.to_csv(
    "../data/03. naver_movie_data.csv", sep=",", encoding="utf-8"
)
```
---
## 3. 영화 평점 데이터 정리

#### requirements
```py
import numpy as np
import pandas as pd
```
```py
movie = pd.read_csv("../data/03. naver_movie_data.csv", index_col=0)
movie.tail() 
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>name</th>
      <th>point</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4591</th>
      <td>2023-01-30</td>
      <td>더 메뉴</td>
      <td>7.12</td>
    </tr>
    <tr>
      <th>4592</th>
      <td>2023-01-30</td>
      <td>유령</td>
      <td>6.88</td>
    </tr>
    <tr>
      <th>4593</th>
      <td>2023-01-30</td>
      <td>원피스 필름 레드</td>
      <td>6.82</td>
    </tr>
    <tr>
      <th>4594</th>
      <td>2023-01-30</td>
      <td>교섭</td>
      <td>6.14</td>
    </tr>
    <tr>
      <th>4595</th>
      <td>2023-01-30</td>
      <td>강남좀비</td>
      <td>2.83</td>
    </tr>
  </tbody>
</table>

- 인덱스 : 영화 이름
- 점수 합산
- 100일 간 네이버 영화 평점 합산 기준 베스트 & 워스트 10 선정

---

#### pivot table
```py
movie_unique = pd.pivot_table(data=movie, index="name", aggfunc=np.sum)
movie_unique
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>point</th>
    </tr>
    <tr>
      <th>name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>120BPM</th>
      <td>19.39</td>
    </tr>
    <tr>
      <th>3000년의 기다림</th>
      <td>126.96</td>
    </tr>
    <tr>
      <th>강남좀비</th>
      <td>14.17</td>
    </tr>
    <tr>
      <th>거북이는 의외로 빨리 헤엄친다</th>
      <td>172.62</td>
    </tr>
    <tr>
      <th>겨울왕국</th>
      <td>109.44</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>헤어질 결심</th>
      <td>867.05</td>
    </tr>
    <tr>
      <th>혼자 사는 사람들</th>
      <td>88.77</td>
    </tr>
    <tr>
      <th>홀리 모터스</th>
      <td>127.14</td>
    </tr>
    <tr>
      <th>화양연화</th>
      <td>114.40</td>
    </tr>
    <tr>
      <th>환절기</th>
      <td>106.21</td>
    </tr>
  </tbody>
</table>

---

```py
movie_best = movie_unique.sort_values(
    by="point", ascending=False # 내림차순
)
movie_best.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>point</th>
    </tr>
    <tr>
      <th>name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>극장판 주술회전 0</th>
      <td>922.82</td>
    </tr>
    <tr>
      <th>너의 이름은.</th>
      <td>881.00</td>
    </tr>
    <tr>
      <th>코다</th>
      <td>867.66</td>
    </tr>
    <tr>
      <th>헤어질 결심</th>
      <td>867.05</td>
    </tr>
    <tr>
      <th>라라랜드</th>
      <td>853.83</td>
    </tr>
  </tbody>
</table>

---

```py
tmp = movie.query("name == ['헤어질 결심']")
tmp
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>name</th>
      <th>point</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>25</th>
      <td>2022-10-23</td>
      <td>헤어질 결심</td>
      <td>8.68</td>
    </tr>
    <tr>
      <th>78</th>
      <td>2022-10-24</td>
      <td>헤어질 결심</td>
      <td>8.68</td>
    </tr>
    <tr>
      <th>126</th>
      <td>2022-10-25</td>
      <td>헤어질 결심</td>
      <td>8.68</td>
    </tr>
    <tr>
      <th>177</th>
      <td>2022-10-26</td>
      <td>헤어질 결심</td>
      <td>8.68</td>
    </tr>
    <tr>
      <th>227</th>
      <td>2022-10-27</td>
      <td>헤어질 결심</td>
      <td>8.67</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4378</th>
      <td>2023-01-26</td>
      <td>헤어질 결심</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>4429</th>
      <td>2023-01-27</td>
      <td>헤어질 결심</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>4478</th>
      <td>2023-01-28</td>
      <td>헤어질 결심</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>4526</th>
      <td>2023-01-29</td>
      <td>헤어질 결심</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>4572</th>
      <td>2023-01-30</td>
      <td>헤어질 결심</td>
      <td>8.64</td>
    </tr>
  </tbody>
</table>

---

### 시각화
#### requirements
```py
import matplotlib.pyplot as plt
from matplotlib import rc

rc("font", family="malgun gothic")
%matplotlib inline
# get_ipython().run_line_magic("matplotlib", "inline")
```
```py
plt.figure(figsize=(20, 8))
plt.plot(tmp["date"], tmp["point"]) # 선 그래프, x축 : 날짜, y축 : 평점 -> 날짜에 따른 평점 변화를 선그래프로 표현(시계열)
plt.title("날짜별 평점")
plt.xlabel("날짜")
plt.ylabel("평점")
plt.xticks(rotation="vertical")
plt.legend(labels=["평점 추이"], loc="best")
plt.grid(True)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/f4425ab4-d6ca-4c76-b8f1-4f8d51d593aa/image.png)


#### 상위 10개 영화
```py
movie_best.head(10)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>point</th>
    </tr>
    <tr>
      <th>name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>극장판 주술회전 0</th>
      <td>922.82</td>
    </tr>
    <tr>
      <th>너의 이름은.</th>
      <td>881.00</td>
    </tr>
    <tr>
      <th>코다</th>
      <td>867.66</td>
    </tr>
    <tr>
      <th>헤어질 결심</th>
      <td>867.05</td>
    </tr>
    <tr>
      <th>라라랜드</th>
      <td>853.83</td>
    </tr>
    <tr>
      <th>비긴 어게인</th>
      <td>840.72</td>
    </tr>
    <tr>
      <th>에브리씽 에브리웨어 올 앳 원스</th>
      <td>706.31</td>
    </tr>
    <tr>
      <th>사랑할 땐 누구나 최악이 된다</th>
      <td>703.74</td>
    </tr>
    <tr>
      <th>날씨의 아이</th>
      <td>701.31</td>
    </tr>
    <tr>
      <th>너의 췌장을 먹고 싶어</th>
      <td>653.11</td>
    </tr>
  </tbody>
</table>

---

#### 하위 10개 영화
```py
movie_best.tail(10)
```
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>point</th>
    </tr>
    <tr>
      <th>name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>컴백홈</th>
      <td>21.68</td>
    </tr>
    <tr>
      <th>120BPM</th>
      <td>19.39</td>
    </tr>
    <tr>
      <th>연애의 기술</th>
      <td>17.84</td>
    </tr>
    <tr>
      <th>리스본행 야간열차</th>
      <td>16.62</td>
    </tr>
    <tr>
      <th>이상한 나라의 수학자</th>
      <td>16.40</td>
    </tr>
    <tr>
      <th>남영동1985</th>
      <td>14.48</td>
    </tr>
    <tr>
      <th>강남좀비</th>
      <td>14.17</td>
    </tr>
    <tr>
      <th>어린 의뢰인</th>
      <td>9.06</td>
    </tr>
    <tr>
      <th>접속</th>
      <td>8.67</td>
    </tr>
    <tr>
      <th>미니언즈2</th>
      <td>7.69</td>
    </tr>
  </tbody>
</table>
</div>

---

```py
movie_pivot = pd.pivot_table(data=movie, index="date", columns="name", values="point")
movie_pivot.head()
```
<div class="output"><div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi></bdi></div><div class="output_subarea output_html rendered_html output_result" dir="auto"><div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>name</th>
      <th>120BPM</th>
      <th>3000년의 기다림</th>
      <th>강남좀비</th>
      <th>거북이는 의외로 빨리 헤엄친다</th>
      <th>겨울왕국</th>
      <th>겨울왕국 2</th>
      <th>고속도로 가족</th>
      <th>공동경비구역 JSA</th>
      <th>공조2: 인터내셔날</th>
      <th>광대: 소리꾼</th>
      <th>...</th>
      <th>피아니스트의 전설</th>
      <th>하우스 오브 구찌</th>
      <th>한산: 용의 출현</th>
      <th>해피 투게더</th>
      <th>헌트</th>
      <th>헤어질 결심</th>
      <th>혼자 사는 사람들</th>
      <th>홀리 모터스</th>
      <th>화양연화</th>
      <th>환절기</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2022-10-23</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.59</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.76</td>
      <td>9.19</td>
      <td>8.38</td>
      <td>8.68</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>8.17</td>
    </tr>
    <tr>
      <th>2022-10-24</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.59</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.76</td>
      <td>9.19</td>
      <td>8.38</td>
      <td>8.68</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>8.17</td>
    </tr>
    <tr>
      <th>2022-10-25</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.58</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.76</td>
      <td>9.19</td>
      <td>8.38</td>
      <td>8.68</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>8.17</td>
    </tr>
    <tr>
      <th>2022-10-26</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.58</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.76</td>
      <td>9.19</td>
      <td>8.38</td>
      <td>8.68</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>8.17</td>
    </tr>
    <tr>
      <th>2022-10-27</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.58</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.76</td>
      <td>9.19</td>
      <td>8.38</td>
      <td>8.67</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.8</td>
      <td>8.17</td>
    </tr>
  </tbody>
</table>
<p></p>
</div></div></div></div>

---

```py
movie_pivot.to_excel("../data/03. movie_pivot.xlsx")
```
```py
import platform
import seaborn as sns
from matplotlib import font_manager, rc
path = "C:/Windows/Fonts/malgun.ttf"

if platform.system() == "Darwin":
    rc("font", family="Arial Unicode MS")
elif platform.system() == "Windows":
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc("font", family=font_name)
else:
    print("Unknows system. sorry")
```
```py
target_col = ['라라랜드', '너의 이름은.', '날씨의 아이', '극장판 주술회전 0', '비긴 어게인']
plt.figure(figsize=(20, 8))
plt.title("날짜별 평점")
plt.xlabel("날짜")
plt.ylabel("평점")
plt.xticks(rotation=("vertical"))
plt.tick_params(bottom="off", labelbottom="off")
plt.plot(movie_pivot[target_col])
plt.legend(target_col, loc="best")
plt.grid(True)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/07727a38-0dcb-4510-a90f-6c07f7923761/image.png)




