---
layout: single
title: "Project 8 - 인구 소멸 위기 지역 데이터 분석"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# 1. 배경
#### 목표
- 인구 소멸 위기 지역 파악
- 인구 소멸 위기 지역 지도 표현
- 지도 표현에 대한 카르토그램 표현

# 2. 데이터 읽고 인구 소멸 지역 계산
#### requirements
```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib
import set_matplotlib_hangul
import warnings

matplotlib.rcParams['axes.unicode_minus'] = False
warnings.filterwarnings(action="ignore")
%matplotlib inline
```

#### fillna()
```py
datas = {
    "A": np.random.randint(1, 45, 8),
    "B": np.random.randint(1, 45, 8),
    "C": np.random.randint(1, 45, 8)
}

fillna_df = pd.DataFrame(datas)
fillna_df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10</td>
      <td>38</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>26</td>
      <td>8</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>24</td>
      <td>34</td>
      <td>20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>14</td>
      <td>39</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24</td>
      <td>38</td>
      <td>30</td>
    </tr>
    <tr>
      <th>5</th>
      <td>42</td>
      <td>37</td>
      <td>8</td>
    </tr>
    <tr>
      <th>6</th>
      <td>15</td>
      <td>25</td>
      <td>5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>13</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>

---

```py
fillna_df.loc[2:4, ["A"]] = np.nan
fillna_df.loc[3:5, ["B"]] = np.nan
fillna_df.loc[4:7, ["C"]] = np.nan
fillna_df
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.0</td>
      <td>38.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>26.0</td>
      <td>8.0</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>34.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>42.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>15.0</td>
      <td>25.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>13.0</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

---

```py
fillna_df.fillna(method="ffill") # pad
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.0</td>
      <td>38.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>26.0</td>
      <td>8.0</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>26.0</td>
      <td>34.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>26.0</td>
      <td>34.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>26.0</td>
      <td>34.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>42.0</td>
      <td>34.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>15.0</td>
      <td>25.0</td>
      <td>39.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>13.0</td>
      <td>2.0</td>
      <td>39.0</td>
    </tr>
  </tbody>
</table>

---

### 데이터 불러오기

```py
population = pd.read_excel("../data/07_population_raw_data.xlsx", header=1)
population.fillna(method="pad", inplace=True)
population
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>행정구역(동읍면)별(1)</th>
      <th>행정구역(동읍면)별(2)</th>
      <th>항목</th>
      <th>계</th>
      <th>20 - 24세</th>
      <th>25 - 29세</th>
      <th>30 - 34세</th>
      <th>35 - 39세</th>
      <th>65 - 69세</th>
      <th>70 - 74세</th>
      <th>75 - 79세</th>
      <th>80 - 84세</th>
      <th>85 - 89세</th>
      <th>90 - 94세</th>
      <th>95 - 99세</th>
      <th>100+</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>전국</td>
      <td>소계</td>
      <td>총인구수 (명)</td>
      <td>51696216.0</td>
      <td>3541061.0</td>
      <td>3217367.0</td>
      <td>3517868</td>
      <td>4016272.0</td>
      <td>2237345.0</td>
      <td>1781229.0</td>
      <td>1457890</td>
      <td>909130.0</td>
      <td>416164.0</td>
      <td>141488.0</td>
      <td>34844</td>
      <td>17562.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>전국</td>
      <td>소계</td>
      <td>남자인구수 (명)</td>
      <td>25827594.0</td>
      <td>1877127.0</td>
      <td>1682988.0</td>
      <td>1806754</td>
      <td>2045265.0</td>
      <td>1072395.0</td>
      <td>806680.0</td>
      <td>600607</td>
      <td>319391.0</td>
      <td>113221.0</td>
      <td>32695.0</td>
      <td>7658</td>
      <td>4137.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>전국</td>
      <td>소계</td>
      <td>여자인구수 (명)</td>
      <td>25868622.0</td>
      <td>1663934.0</td>
      <td>1534379.0</td>
      <td>1711114</td>
      <td>1971007.0</td>
      <td>1164950.0</td>
      <td>974549.0</td>
      <td>857283</td>
      <td>589739.0</td>
      <td>302943.0</td>
      <td>108793.0</td>
      <td>27186</td>
      <td>13425.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서울특별시</td>
      <td>소계</td>
      <td>총인구수 (명)</td>
      <td>9930616.0</td>
      <td>690728.0</td>
      <td>751973.0</td>
      <td>803507</td>
      <td>817467.0</td>
      <td>448956.0</td>
      <td>350580.0</td>
      <td>251961</td>
      <td>141649.0</td>
      <td>66067.0</td>
      <td>24153.0</td>
      <td>7058</td>
      <td>5475.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서울특별시</td>
      <td>소계</td>
      <td>남자인구수 (명)</td>
      <td>4876789.0</td>
      <td>347534.0</td>
      <td>372249.0</td>
      <td>402358</td>
      <td>410076.0</td>
      <td>211568.0</td>
      <td>163766.0</td>
      <td>112076</td>
      <td>54033.0</td>
      <td>19595.0</td>
      <td>6146.0</td>
      <td>1900</td>
      <td>1406.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>841</th>
      <td>제주특별자치도</td>
      <td>제주시</td>
      <td>남자인구수 (명)</td>
      <td>235977.0</td>
      <td>17377.0</td>
      <td>13118.0</td>
      <td>15084</td>
      <td>18350.0</td>
      <td>8474.0</td>
      <td>6782.0</td>
      <td>4941</td>
      <td>2737.0</td>
      <td>854.0</td>
      <td>226.0</td>
      <td>53</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>842</th>
      <td>제주특별자치도</td>
      <td>제주시</td>
      <td>여자인구수 (명)</td>
      <td>234688.0</td>
      <td>15261.0</td>
      <td>12245.0</td>
      <td>14687</td>
      <td>18062.0</td>
      <td>9265.0</td>
      <td>7877.0</td>
      <td>7178</td>
      <td>5649.0</td>
      <td>3122.0</td>
      <td>1387.0</td>
      <td>460</td>
      <td>137.0</td>
    </tr>
    <tr>
      <th>843</th>
      <td>제주특별자치도</td>
      <td>서귀포시</td>
      <td>총인구수 (명)</td>
      <td>170932.0</td>
      <td>10505.0</td>
      <td>8067.0</td>
      <td>9120</td>
      <td>11606.0</td>
      <td>8686.0</td>
      <td>7460.0</td>
      <td>6456</td>
      <td>4521.0</td>
      <td>1855.0</td>
      <td>733.0</td>
      <td>242</td>
      <td>77.0</td>
    </tr>
    <tr>
      <th>844</th>
      <td>제주특별자치도</td>
      <td>서귀포시</td>
      <td>남자인구수 (명)</td>
      <td>86568.0</td>
      <td>5600.0</td>
      <td>4247.0</td>
      <td>4693</td>
      <td>6082.0</td>
      <td>4237.0</td>
      <td>3441.0</td>
      <td>2611</td>
      <td>1494.0</td>
      <td>370.0</td>
      <td>103.0</td>
      <td>29</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>845</th>
      <td>제주특별자치도</td>
      <td>서귀포시</td>
      <td>여자인구수 (명)</td>
      <td>84364.0</td>
      <td>4905.0</td>
      <td>3820.0</td>
      <td>4427</td>
      <td>5524.0</td>
      <td>4449.0</td>
      <td>4019.0</td>
      <td>3845</td>
      <td>3027.0</td>
      <td>1485.0</td>
      <td>630.0</td>
      <td>213</td>
      <td>68.0</td>
    </tr>
  </tbody>
</table>

---

```py
population.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 846 entries, 0 to 845
Data columns (total 16 columns):
 #   Column         Non-Null Count  Dtype  
---  ------         --------------  -----  
 0   행정구역(동읍면)별(1)  846 non-null    object 
 1   행정구역(동읍면)별(2)  846 non-null    object 
 2   항목             846 non-null    object 
 3   계              846 non-null    float64
 4   20 - 24세       846 non-null    float64
 5   25 - 29세       846 non-null    float64
 6   30 - 34세       846 non-null    int64  
 7   35 - 39세       846 non-null    float64
 8   65 - 69세       846 non-null    float64
 9   70 - 74세       846 non-null    float64
 10  75 - 79세       846 non-null    int64  
 11  80 - 84세       846 non-null    float64
 12  85 - 89세       846 non-null    float64
 13  90 - 94세       846 non-null    float64
 14  95 - 99세       846 non-null    int64  
 15  100+           846 non-null    float64
dtypes: float64(10), int64(3), object(3)
memory usage: 105.9+ KB
```

---

#### 컬럼 이름 변경

```py
population.rename(
    columns={
        "행정구역(동읍면)별(1)": "광역시도",
        "행정구역(동읍면)별(2)": "시도",
        "계": "인구수"
    }, inplace=True
)
population.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>항목</th>
      <th>인구수</th>
      <th>20 - 24세</th>
      <th>25 - 29세</th>
      <th>30 - 34세</th>
      <th>35 - 39세</th>
      <th>65 - 69세</th>
      <th>70 - 74세</th>
      <th>75 - 79세</th>
      <th>80 - 84세</th>
      <th>85 - 89세</th>
      <th>90 - 94세</th>
      <th>95 - 99세</th>
      <th>100+</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>전국</td>
      <td>소계</td>
      <td>총인구수 (명)</td>
      <td>51696216.0</td>
      <td>3541061.0</td>
      <td>3217367.0</td>
      <td>3517868</td>
      <td>4016272.0</td>
      <td>2237345.0</td>
      <td>1781229.0</td>
      <td>1457890</td>
      <td>909130.0</td>
      <td>416164.0</td>
      <td>141488.0</td>
      <td>34844</td>
      <td>17562.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>전국</td>
      <td>소계</td>
      <td>남자인구수 (명)</td>
      <td>25827594.0</td>
      <td>1877127.0</td>
      <td>1682988.0</td>
      <td>1806754</td>
      <td>2045265.0</td>
      <td>1072395.0</td>
      <td>806680.0</td>
      <td>600607</td>
      <td>319391.0</td>
      <td>113221.0</td>
      <td>32695.0</td>
      <td>7658</td>
      <td>4137.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>전국</td>
      <td>소계</td>
      <td>여자인구수 (명)</td>
      <td>25868622.0</td>
      <td>1663934.0</td>
      <td>1534379.0</td>
      <td>1711114</td>
      <td>1971007.0</td>
      <td>1164950.0</td>
      <td>974549.0</td>
      <td>857283</td>
      <td>589739.0</td>
      <td>302943.0</td>
      <td>108793.0</td>
      <td>27186</td>
      <td>13425.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서울특별시</td>
      <td>소계</td>
      <td>총인구수 (명)</td>
      <td>9930616.0</td>
      <td>690728.0</td>
      <td>751973.0</td>
      <td>803507</td>
      <td>817467.0</td>
      <td>448956.0</td>
      <td>350580.0</td>
      <td>251961</td>
      <td>141649.0</td>
      <td>66067.0</td>
      <td>24153.0</td>
      <td>7058</td>
      <td>5475.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서울특별시</td>
      <td>소계</td>
      <td>남자인구수 (명)</td>
      <td>4876789.0</td>
      <td>347534.0</td>
      <td>372249.0</td>
      <td>402358</td>
      <td>410076.0</td>
      <td>211568.0</td>
      <td>163766.0</td>
      <td>112076</td>
      <td>54033.0</td>
      <td>19595.0</td>
      <td>6146.0</td>
      <td>1900</td>
      <td>1406.0</td>
    </tr>
  </tbody>
</table>

---

#### 소계 제거
```py
population = population[population["시도"] != "소계"]
population.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>항목</th>
      <th>인구수</th>
      <th>20 - 24세</th>
      <th>25 - 29세</th>
      <th>30 - 34세</th>
      <th>35 - 39세</th>
      <th>65 - 69세</th>
      <th>70 - 74세</th>
      <th>75 - 79세</th>
      <th>80 - 84세</th>
      <th>85 - 89세</th>
      <th>90 - 94세</th>
      <th>95 - 99세</th>
      <th>100+</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>총인구수 (명)</td>
      <td>152737.0</td>
      <td>11379.0</td>
      <td>11891.0</td>
      <td>10684</td>
      <td>10379.0</td>
      <td>7411.0</td>
      <td>6636.0</td>
      <td>5263</td>
      <td>3104.0</td>
      <td>1480.0</td>
      <td>602.0</td>
      <td>234</td>
      <td>220.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>남자인구수 (명)</td>
      <td>75201.0</td>
      <td>5620.0</td>
      <td>6181.0</td>
      <td>5387</td>
      <td>5034.0</td>
      <td>3411.0</td>
      <td>3009.0</td>
      <td>2311</td>
      <td>1289.0</td>
      <td>506.0</td>
      <td>207.0</td>
      <td>89</td>
      <td>73.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>여자인구수 (명)</td>
      <td>77536.0</td>
      <td>5759.0</td>
      <td>5710.0</td>
      <td>5297</td>
      <td>5345.0</td>
      <td>4000.0</td>
      <td>3627.0</td>
      <td>2952</td>
      <td>1815.0</td>
      <td>974.0</td>
      <td>395.0</td>
      <td>145</td>
      <td>147.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>서울특별시</td>
      <td>중구</td>
      <td>총인구수 (명)</td>
      <td>125249.0</td>
      <td>8216.0</td>
      <td>9529.0</td>
      <td>10332</td>
      <td>10107.0</td>
      <td>6399.0</td>
      <td>5313.0</td>
      <td>4127</td>
      <td>2502.0</td>
      <td>1260.0</td>
      <td>469.0</td>
      <td>158</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>서울특별시</td>
      <td>중구</td>
      <td>남자인구수 (명)</td>
      <td>62204.0</td>
      <td>4142.0</td>
      <td>4792.0</td>
      <td>5192</td>
      <td>5221.0</td>
      <td>3113.0</td>
      <td>2405.0</td>
      <td>1752</td>
      <td>929.0</td>
      <td>414.0</td>
      <td>132.0</td>
      <td>56</td>
      <td>51.0</td>
    </tr>
  </tbody>
</table>

---

```py
population.is_copy = False # copy 했을 때 warning X

population.rename(
    columns={"항목": "구분"}, inplace=True
)
population.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>구분</th>
      <th>인구수</th>
      <th>20 - 24세</th>
      <th>25 - 29세</th>
      <th>30 - 34세</th>
      <th>35 - 39세</th>
      <th>65 - 69세</th>
      <th>70 - 74세</th>
      <th>75 - 79세</th>
      <th>80 - 84세</th>
      <th>85 - 89세</th>
      <th>90 - 94세</th>
      <th>95 - 99세</th>
      <th>100+</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>총인구수 (명)</td>
      <td>152737.0</td>
      <td>11379.0</td>
      <td>11891.0</td>
      <td>10684</td>
      <td>10379.0</td>
      <td>7411.0</td>
      <td>6636.0</td>
      <td>5263</td>
      <td>3104.0</td>
      <td>1480.0</td>
      <td>602.0</td>
      <td>234</td>
      <td>220.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>남자인구수 (명)</td>
      <td>75201.0</td>
      <td>5620.0</td>
      <td>6181.0</td>
      <td>5387</td>
      <td>5034.0</td>
      <td>3411.0</td>
      <td>3009.0</td>
      <td>2311</td>
      <td>1289.0</td>
      <td>506.0</td>
      <td>207.0</td>
      <td>89</td>
      <td>73.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>여자인구수 (명)</td>
      <td>77536.0</td>
      <td>5759.0</td>
      <td>5710.0</td>
      <td>5297</td>
      <td>5345.0</td>
      <td>4000.0</td>
      <td>3627.0</td>
      <td>2952</td>
      <td>1815.0</td>
      <td>974.0</td>
      <td>395.0</td>
      <td>145</td>
      <td>147.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>서울특별시</td>
      <td>중구</td>
      <td>총인구수 (명)</td>
      <td>125249.0</td>
      <td>8216.0</td>
      <td>9529.0</td>
      <td>10332</td>
      <td>10107.0</td>
      <td>6399.0</td>
      <td>5313.0</td>
      <td>4127</td>
      <td>2502.0</td>
      <td>1260.0</td>
      <td>469.0</td>
      <td>158</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>서울특별시</td>
      <td>중구</td>
      <td>남자인구수 (명)</td>
      <td>62204.0</td>
      <td>4142.0</td>
      <td>4792.0</td>
      <td>5192</td>
      <td>5221.0</td>
      <td>3113.0</td>
      <td>2405.0</td>
      <td>1752</td>
      <td>929.0</td>
      <td>414.0</td>
      <td>132.0</td>
      <td>56</td>
      <td>51.0</td>
    </tr>
  </tbody>
</table>

---

```py
population.loc[population["구분"] == "총인구수 (명)", "구분"] = "합계"
population.loc[population["구분"] == "남자인구수 (명)", "구분"] = "남자"
population.loc[population["구분"] == "여자인구수 (명)", "구분"] = "여자"
population.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>구분</th>
      <th>인구수</th>
      <th>20 - 24세</th>
      <th>25 - 29세</th>
      <th>30 - 34세</th>
      <th>35 - 39세</th>
      <th>65 - 69세</th>
      <th>70 - 74세</th>
      <th>75 - 79세</th>
      <th>80 - 84세</th>
      <th>85 - 89세</th>
      <th>90 - 94세</th>
      <th>95 - 99세</th>
      <th>100+</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>합계</td>
      <td>152737.0</td>
      <td>11379.0</td>
      <td>11891.0</td>
      <td>10684</td>
      <td>10379.0</td>
      <td>7411.0</td>
      <td>6636.0</td>
      <td>5263</td>
      <td>3104.0</td>
      <td>1480.0</td>
      <td>602.0</td>
      <td>234</td>
      <td>220.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>남자</td>
      <td>75201.0</td>
      <td>5620.0</td>
      <td>6181.0</td>
      <td>5387</td>
      <td>5034.0</td>
      <td>3411.0</td>
      <td>3009.0</td>
      <td>2311</td>
      <td>1289.0</td>
      <td>506.0</td>
      <td>207.0</td>
      <td>89</td>
      <td>73.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>여자</td>
      <td>77536.0</td>
      <td>5759.0</td>
      <td>5710.0</td>
      <td>5297</td>
      <td>5345.0</td>
      <td>4000.0</td>
      <td>3627.0</td>
      <td>2952</td>
      <td>1815.0</td>
      <td>974.0</td>
      <td>395.0</td>
      <td>145</td>
      <td>147.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>서울특별시</td>
      <td>중구</td>
      <td>합계</td>
      <td>125249.0</td>
      <td>8216.0</td>
      <td>9529.0</td>
      <td>10332</td>
      <td>10107.0</td>
      <td>6399.0</td>
      <td>5313.0</td>
      <td>4127</td>
      <td>2502.0</td>
      <td>1260.0</td>
      <td>469.0</td>
      <td>158</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>서울특별시</td>
      <td>중구</td>
      <td>남자</td>
      <td>62204.0</td>
      <td>4142.0</td>
      <td>4792.0</td>
      <td>5192</td>
      <td>5221.0</td>
      <td>3113.0</td>
      <td>2405.0</td>
      <td>1752</td>
      <td>929.0</td>
      <td>414.0</td>
      <td>132.0</td>
      <td>56</td>
      <td>51.0</td>
    </tr>
  </tbody>
</table>

---

#### 소멸지역 조사를 위한 데이터
```py
population["20-39세"] = (
    population["20 - 24세"] +
    population["25 - 29세"] +
    population["30 - 34세"] +
    population["35 - 39세"]
)

population["65세이상"] = (
    population["65 - 69세"] +
    population["70 - 74세"] +
    population["75 - 79세"] +
    population["80 - 84세"] +
    population["85 - 89세"] +
    population["90 - 94세"] +
    population["95 - 99세"] +
    population["100+"]
)

population.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>구분</th>
      <th>인구수</th>
      <th>20 - 24세</th>
      <th>25 - 29세</th>
      <th>30 - 34세</th>
      <th>35 - 39세</th>
      <th>65 - 69세</th>
      <th>70 - 74세</th>
      <th>75 - 79세</th>
      <th>80 - 84세</th>
      <th>85 - 89세</th>
      <th>90 - 94세</th>
      <th>95 - 99세</th>
      <th>100+</th>
      <th>20-39세</th>
      <th>65세이상</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>841</th>
      <td>제주특별자치도</td>
      <td>제주시</td>
      <td>남자</td>
      <td>235977.0</td>
      <td>17377.0</td>
      <td>13118.0</td>
      <td>15084</td>
      <td>18350.0</td>
      <td>8474.0</td>
      <td>6782.0</td>
      <td>4941</td>
      <td>2737.0</td>
      <td>854.0</td>
      <td>226.0</td>
      <td>53</td>
      <td>17.0</td>
      <td>63929.0</td>
      <td>24084.0</td>
    </tr>
    <tr>
      <th>842</th>
      <td>제주특별자치도</td>
      <td>제주시</td>
      <td>여자</td>
      <td>234688.0</td>
      <td>15261.0</td>
      <td>12245.0</td>
      <td>14687</td>
      <td>18062.0</td>
      <td>9265.0</td>
      <td>7877.0</td>
      <td>7178</td>
      <td>5649.0</td>
      <td>3122.0</td>
      <td>1387.0</td>
      <td>460</td>
      <td>137.0</td>
      <td>60255.0</td>
      <td>35075.0</td>
    </tr>
    <tr>
      <th>843</th>
      <td>제주특별자치도</td>
      <td>서귀포시</td>
      <td>합계</td>
      <td>170932.0</td>
      <td>10505.0</td>
      <td>8067.0</td>
      <td>9120</td>
      <td>11606.0</td>
      <td>8686.0</td>
      <td>7460.0</td>
      <td>6456</td>
      <td>4521.0</td>
      <td>1855.0</td>
      <td>733.0</td>
      <td>242</td>
      <td>77.0</td>
      <td>39298.0</td>
      <td>30030.0</td>
    </tr>
    <tr>
      <th>844</th>
      <td>제주특별자치도</td>
      <td>서귀포시</td>
      <td>남자</td>
      <td>86568.0</td>
      <td>5600.0</td>
      <td>4247.0</td>
      <td>4693</td>
      <td>6082.0</td>
      <td>4237.0</td>
      <td>3441.0</td>
      <td>2611</td>
      <td>1494.0</td>
      <td>370.0</td>
      <td>103.0</td>
      <td>29</td>
      <td>9.0</td>
      <td>20622.0</td>
      <td>12294.0</td>
    </tr>
    <tr>
      <th>845</th>
      <td>제주특별자치도</td>
      <td>서귀포시</td>
      <td>여자</td>
      <td>84364.0</td>
      <td>4905.0</td>
      <td>3820.0</td>
      <td>4427</td>
      <td>5524.0</td>
      <td>4449.0</td>
      <td>4019.0</td>
      <td>3845</td>
      <td>3027.0</td>
      <td>1485.0</td>
      <td>630.0</td>
      <td>213</td>
      <td>68.0</td>
      <td>18676.0</td>
      <td>17736.0</td>
    </tr>
  </tbody>
</table>

---


#### pivot_table

```py
pop = pd.pivot_table(
    data=population,
    index=["광역시도", "시도"],
    columns=["구분"],
    values=["인구수", "20-39세", "65세이상"]
)

pop
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3">20-39세</th>
      <th colspan="3">65세이상</th>
      <th colspan="3">인구수</th>
    </tr>
    <tr>
      <th></th>
      <th>구분</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
    </tr>
    <tr>
      <th>광역시도</th>
      <th>시도</th>
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
      <th rowspan="5" valign="top">강원도</th>
      <th>강릉시</th>
      <td>26286.0</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>15767.0</td>
      <td>21912.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
    </tr>
    <tr>
      <th>고성군</th>
      <td>4494.0</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>2900.0</td>
      <td>4251.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
    </tr>
    <tr>
      <th>동해시</th>
      <td>11511.0</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>6392.0</td>
      <td>8732.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
    </tr>
    <tr>
      <th>삼척시</th>
      <td>8708.0</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>5892.0</td>
      <td>8718.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
    </tr>
    <tr>
      <th>속초시</th>
      <td>9956.0</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>5139.0</td>
      <td>7613.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">충청북도</th>
      <th>진천군</th>
      <td>9391.0</td>
      <td>7622.0</td>
      <td>17013.0</td>
      <td>4731.0</td>
      <td>6575.0</td>
      <td>11306.0</td>
      <td>36387.0</td>
      <td>33563.0</td>
      <td>69950.0</td>
    </tr>
    <tr>
      <th>청원구</th>
      <td>32216.0</td>
      <td>27805.0</td>
      <td>60021.0</td>
      <td>8417.0</td>
      <td>11914.0</td>
      <td>20331.0</td>
      <td>97006.0</td>
      <td>93807.0</td>
      <td>190813.0</td>
    </tr>
    <tr>
      <th>청주시</th>
      <td>128318.0</td>
      <td>115719.0</td>
      <td>244037.0</td>
      <td>37882.0</td>
      <td>53671.0</td>
      <td>91553.0</td>
      <td>419323.0</td>
      <td>415874.0</td>
      <td>835197.0</td>
    </tr>
    <tr>
      <th>충주시</th>
      <td>26600.0</td>
      <td>22757.0</td>
      <td>49357.0</td>
      <td>14407.0</td>
      <td>20383.0</td>
      <td>34790.0</td>
      <td>104877.0</td>
      <td>103473.0</td>
      <td>208350.0</td>
    </tr>
    <tr>
      <th>흥덕구</th>
      <td>40933.0</td>
      <td>37675.0</td>
      <td>78608.0</td>
      <td>9788.0</td>
      <td>13671.0</td>
      <td>23459.0</td>
      <td>127647.0</td>
      <td>125916.0</td>
      <td>253563.0</td>
    </tr>
  </tbody>
</table>

---

#### 소멸 비율 계산

```py
pop["소멸비율"] = pop["20-39세", "여자"] / (pop["65세이상", "합계"] / 2)
pop.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3">20-39세</th>
      <th colspan="3">65세이상</th>
      <th colspan="3">인구수</th>
      <th>소멸비율</th>
    </tr>
    <tr>
      <th></th>
      <th>구분</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th></th>
    </tr>
    <tr>
      <th>광역시도</th>
      <th>시도</th>
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
      <th rowspan="5" valign="top">충청북도</th>
      <th>진천군</th>
      <td>9391.0</td>
      <td>7622.0</td>
      <td>17013.0</td>
      <td>4731.0</td>
      <td>6575.0</td>
      <td>11306.0</td>
      <td>36387.0</td>
      <td>33563.0</td>
      <td>69950.0</td>
      <td>1.348311</td>
    </tr>
    <tr>
      <th>청원구</th>
      <td>32216.0</td>
      <td>27805.0</td>
      <td>60021.0</td>
      <td>8417.0</td>
      <td>11914.0</td>
      <td>20331.0</td>
      <td>97006.0</td>
      <td>93807.0</td>
      <td>190813.0</td>
      <td>2.735232</td>
    </tr>
    <tr>
      <th>청주시</th>
      <td>128318.0</td>
      <td>115719.0</td>
      <td>244037.0</td>
      <td>37882.0</td>
      <td>53671.0</td>
      <td>91553.0</td>
      <td>419323.0</td>
      <td>415874.0</td>
      <td>835197.0</td>
      <td>2.527913</td>
    </tr>
    <tr>
      <th>충주시</th>
      <td>26600.0</td>
      <td>22757.0</td>
      <td>49357.0</td>
      <td>14407.0</td>
      <td>20383.0</td>
      <td>34790.0</td>
      <td>104877.0</td>
      <td>103473.0</td>
      <td>208350.0</td>
      <td>1.308249</td>
    </tr>
    <tr>
      <th>흥덕구</th>
      <td>40933.0</td>
      <td>37675.0</td>
      <td>78608.0</td>
      <td>9788.0</td>
      <td>13671.0</td>
      <td>23459.0</td>
      <td>127647.0</td>
      <td>125916.0</td>
      <td>253563.0</td>
      <td>3.211987</td>
    </tr>
  </tbody>
</table>

---

#### 소멸 위기 지역 컬럼 생성
```py
pop["소멸위기지역"] = pop["소멸비율"] < 1.0
pop
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3">20-39세</th>
      <th colspan="3">65세이상</th>
      <th colspan="3">인구수</th>
      <th>소멸비율</th>
      <th>소멸위기지역</th>
    </tr>
    <tr>
      <th></th>
      <th>구분</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th></th>
      <th></th>
    </tr>
    <tr>
      <th>광역시도</th>
      <th>시도</th>
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
      <th rowspan="5" valign="top">강원도</th>
      <th>강릉시</th>
      <td>26286.0</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>15767.0</td>
      <td>21912.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
      <td>1.226041</td>
      <td>False</td>
    </tr>
    <tr>
      <th>고성군</th>
      <td>4494.0</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>2900.0</td>
      <td>4251.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
      <td>0.707314</td>
      <td>True</td>
    </tr>
    <tr>
      <th>동해시</th>
      <td>11511.0</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>6392.0</td>
      <td>8732.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
      <td>1.289738</td>
      <td>False</td>
    </tr>
    <tr>
      <th>삼척시</th>
      <td>8708.0</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>5892.0</td>
      <td>8718.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
      <td>0.973990</td>
      <td>True</td>
    </tr>
    <tr>
      <th>속초시</th>
      <td>9956.0</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>5139.0</td>
      <td>7613.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
      <td>1.372647</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">충청북도</th>
      <th>진천군</th>
      <td>9391.0</td>
      <td>7622.0</td>
      <td>17013.0</td>
      <td>4731.0</td>
      <td>6575.0</td>
      <td>11306.0</td>
      <td>36387.0</td>
      <td>33563.0</td>
      <td>69950.0</td>
      <td>1.348311</td>
      <td>False</td>
    </tr>
    <tr>
      <th>청원구</th>
      <td>32216.0</td>
      <td>27805.0</td>
      <td>60021.0</td>
      <td>8417.0</td>
      <td>11914.0</td>
      <td>20331.0</td>
      <td>97006.0</td>
      <td>93807.0</td>
      <td>190813.0</td>
      <td>2.735232</td>
      <td>False</td>
    </tr>
    <tr>
      <th>청주시</th>
      <td>128318.0</td>
      <td>115719.0</td>
      <td>244037.0</td>
      <td>37882.0</td>
      <td>53671.0</td>
      <td>91553.0</td>
      <td>419323.0</td>
      <td>415874.0</td>
      <td>835197.0</td>
      <td>2.527913</td>
      <td>False</td>
    </tr>
    <tr>
      <th>충주시</th>
      <td>26600.0</td>
      <td>22757.0</td>
      <td>49357.0</td>
      <td>14407.0</td>
      <td>20383.0</td>
      <td>34790.0</td>
      <td>104877.0</td>
      <td>103473.0</td>
      <td>208350.0</td>
      <td>1.308249</td>
      <td>False</td>
    </tr>
    <tr>
      <th>흥덕구</th>
      <td>40933.0</td>
      <td>37675.0</td>
      <td>78608.0</td>
      <td>9788.0</td>
      <td>13671.0</td>
      <td>23459.0</td>
      <td>127647.0</td>
      <td>125916.0</td>
      <td>253563.0</td>
      <td>3.211987</td>
      <td>False</td>
    </tr>
  </tbody>
</table>

---

#### 소멸 위기 지역 조회
```py
pop[pop["소멸위기지역"] == True].index.get_level_values(1)

=>

Index(['고성군', '삼척시', '양양군', '영월군', '정선군', '평창군', '홍천군', '횡성군', '가평군', '양평군',
       '연천군', '거창군', '고성군', '남해군', '밀양시', '산청군', '의령군', '창녕군', 
       								.
                                    .
                                    .
       '괴산군', '단양군',
       '보은군', '영동군', '옥천군'],
      dtype='object', name='시도')
```

---

```py
pop.reset_index(inplace=True)
pop.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th colspan="3">20-39세</th>
      <th colspan="3">65세이상</th>
      <th colspan="3">인구수</th>
      <th>소멸비율</th>
      <th>소멸위기지역</th>
    </tr>
    <tr>
      <th>구분</th>
      <th></th>
      <th></th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th>남자</th>
      <th>여자</th>
      <th>합계</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도</td>
      <td>강릉시</td>
      <td>26286.0</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>15767.0</td>
      <td>21912.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
      <td>1.226041</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강원도</td>
      <td>고성군</td>
      <td>4494.0</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>2900.0</td>
      <td>4251.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
      <td>0.707314</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도</td>
      <td>동해시</td>
      <td>11511.0</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>6392.0</td>
      <td>8732.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
      <td>1.289738</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도</td>
      <td>삼척시</td>
      <td>8708.0</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>5892.0</td>
      <td>8718.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
      <td>0.973990</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도</td>
      <td>속초시</td>
      <td>9956.0</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>5139.0</td>
      <td>7613.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
      <td>1.372647</td>
      <td>False</td>
    </tr>
  </tbody>
</table>

---

```py
tmp_columns = [
    pop.columns.get_level_values(0)[n] + pop.columns.get_level_values(1)[n]
    for n in range(0, len(pop.columns.get_level_values(0)))
]

pop.columns = tmp_columns
pop.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>20-39세남자</th>
      <th>20-39세여자</th>
      <th>20-39세합계</th>
      <th>65세이상남자</th>
      <th>65세이상여자</th>
      <th>65세이상합계</th>
      <th>인구수남자</th>
      <th>인구수여자</th>
      <th>인구수합계</th>
      <th>소멸비율</th>
      <th>소멸위기지역</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도</td>
      <td>강릉시</td>
      <td>26286.0</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>15767.0</td>
      <td>21912.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
      <td>1.226041</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강원도</td>
      <td>고성군</td>
      <td>4494.0</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>2900.0</td>
      <td>4251.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
      <td>0.707314</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도</td>
      <td>동해시</td>
      <td>11511.0</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>6392.0</td>
      <td>8732.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
      <td>1.289738</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도</td>
      <td>삼척시</td>
      <td>8708.0</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>5892.0</td>
      <td>8718.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
      <td>0.973990</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도</td>
      <td>속초시</td>
      <td>9956.0</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>5139.0</td>
      <td>7613.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
      <td>1.372647</td>
      <td>False</td>
    </tr>
  </tbody>
</table>

---

# 3. 지도 시각화를 위한 지역별 ID 만들기
```py
pop.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 264 entries, 0 to 263
Data columns (total 13 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   광역시도      264 non-null    object 
 1   시도        264 non-null    object 
 2   20-39세남자  264 non-null    float64
 3   20-39세여자  264 non-null    float64
 4   20-39세합계  264 non-null    float64
 5   65세이상남자   264 non-null    float64
 6   65세이상여자   264 non-null    float64
 7   65세이상합계   264 non-null    float64
 8   인구수남자     264 non-null    float64
 9   인구수여자     264 non-null    float64
 10  인구수합계     264 non-null    float64
 11  소멸비율      264 non-null    float64
 12  소멸위기지역    264 non-null    bool   
dtypes: bool(1), float64(10), object(2)
memory usage: 25.1+ KB
```

---

```py
pop["시도"].unique()

=>

array(['강릉시', '고성군', '동해시', '삼척시', '속초시', '양구군', '양양군', '영월군', '원주시',
       '인제군', '정선군', '철원군', '춘천시', '태백시', '평창군', '홍천군', '화천군', '횡성군',

										.
                                        .
                                        .

'단양군',
       '보은군', '상당구', '서원구', '영동군', '옥천군', '음성군', '제천시', '증평군', '진천군',
       '청원구', '청주시', '충주시', '흥덕구'], dtype=object)
```

---

```py
si_name = [None] * len(pop)

tmp_gu_dict = {
    "수원": ["장안구", "권선구", "팔달구", "영통구"],
    "성남": ["수정구", "중원구", "분당구"],
    "안양": ["만안구", "동안구"],
    "안산": ["상록구", "단원구"],
    "고양": ["덕양구", "일산동구", "일산서구"],
    "용인": ["처인구", "기흥구", "수지구"],
    "청주": ["상당구", "서원구", "흥덕구", "청원구"],
    "천안": ["동남구", "서북구"],
    "전주": ["완산구", "덕진구"],
    "포항": ["남구", "북구"],
    "창원": ["의창구", "성산구", "진해구", "마산합포구", "마산회원구"],
    "부천": ["오정구", "원미구", "소사구"]
}
```
---

```py
pop["광역시도"].unique()

=>

array(['강원도', '경기도', '경상남도', '경상북도', '광주광역시', '대구광역시', '대전광역시', '부산광역시',
       '서울특별시', '세종특별자치시', '울산광역시', '인천광역시', '전라남도', '전라북도', '제주특별자치도',
       '충청남도', '충청북도'], dtype=object)
```

---

```py
pop["시도"].unique()

=>

array(['강릉시', '고성군', '동해시', '삼척시', '속초시', '양구군', '양양군', '영월군', '원주시',
       '인제군', '정선군', '철원군', '춘천시', '태백시', '평창군', '홍천군', '화천군', '횡성군',
											.
											.
                                            .
       '보은군', '상당구', '서원구', '영동군', '옥천군', '음성군', '제천시', '증평군', '진천군',
       '청원구', '청주시', '충주시', '흥덕구'], dtype=object)
```

---

### (1) 일반 시 이름과 세종시, 광역시도 일반 구 정리

```py
for idx, row in pop.iterrows():
    if row["광역시도"][-3:] not in ["광역시", "특별시", "자치시"]:
        si_name[idx] = row["시도"][:-1]

    elif row["광역시도"] == "세종특별자치시":
        si_name[idx] = "세종"

    else:
        if len(row["시도"]) == 2:
            si_name[idx] = row["광역시도"][:2] + " " + row["시도"]
        else:
            si_name[idx] = row["광역시도"][:2] + " " + row["시도"][:-1]
```

---
### (2) 행정구

```py
for idx, row in pop.iterrows():
    if row["광역시도"][-3:] not in ["광역시", "특별시", "자치시"]:
        for keys, values in tmp_gu_dict.items():        # dataframe -> iterrows() == dictionary -> items()
            if row["시도"] in values:
                if len(row["시도"]) == 2:
                    si_name[idx] = keys + " " + row["시도"]

                elif row["시도"] in ["마산합포구", "마산회원구"]:
                    si_name[idx] = keys + " " + row["시도"][2:-1]

                else:
                    si_name[idx] = keys + " " + row["시도"][:-1]
```
---
### (3) 고성군
```py
for idx, row in pop.iterrows():
    if row["광역시도"][-3:] not in ["광역시", "특별시", "자치시"]:
        if row["시도"][:-1] == "고성" and row["광역시도"] == "강원도":
            si_name[idx] = "고성(강원)"
        elif row["시도"][:-1] == "고성" and row["광역시도"] == "경상남도":
            si_name[idx] = "고성(경남)"
```
```py
pop["ID"] = si_name
pop
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>20-39세남자</th>
      <th>20-39세여자</th>
      <th>20-39세합계</th>
      <th>65세이상남자</th>
      <th>65세이상여자</th>
      <th>65세이상합계</th>
      <th>인구수남자</th>
      <th>인구수여자</th>
      <th>인구수합계</th>
      <th>소멸비율</th>
      <th>소멸위기지역</th>
      <th>ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도</td>
      <td>강릉시</td>
      <td>26286.0</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>15767.0</td>
      <td>21912.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
      <td>1.226041</td>
      <td>False</td>
      <td>강릉</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강원도</td>
      <td>고성군</td>
      <td>4494.0</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>2900.0</td>
      <td>4251.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
      <td>0.707314</td>
      <td>True</td>
      <td>고성(강원)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도</td>
      <td>동해시</td>
      <td>11511.0</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>6392.0</td>
      <td>8732.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
      <td>1.289738</td>
      <td>False</td>
      <td>동해</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도</td>
      <td>삼척시</td>
      <td>8708.0</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>5892.0</td>
      <td>8718.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
      <td>0.973990</td>
      <td>True</td>
      <td>삼척</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도</td>
      <td>속초시</td>
      <td>9956.0</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>5139.0</td>
      <td>7613.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
      <td>1.372647</td>
      <td>False</td>
      <td>속초</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>259</th>
      <td>충청북도</td>
      <td>진천군</td>
      <td>9391.0</td>
      <td>7622.0</td>
      <td>17013.0</td>
      <td>4731.0</td>
      <td>6575.0</td>
      <td>11306.0</td>
      <td>36387.0</td>
      <td>33563.0</td>
      <td>69950.0</td>
      <td>1.348311</td>
      <td>False</td>
      <td>진천</td>
    </tr>
    <tr>
      <th>260</th>
      <td>충청북도</td>
      <td>청원구</td>
      <td>32216.0</td>
      <td>27805.0</td>
      <td>60021.0</td>
      <td>8417.0</td>
      <td>11914.0</td>
      <td>20331.0</td>
      <td>97006.0</td>
      <td>93807.0</td>
      <td>190813.0</td>
      <td>2.735232</td>
      <td>False</td>
      <td>청주 청원</td>
    </tr>
    <tr>
      <th>261</th>
      <td>충청북도</td>
      <td>청주시</td>
      <td>128318.0</td>
      <td>115719.0</td>
      <td>244037.0</td>
      <td>37882.0</td>
      <td>53671.0</td>
      <td>91553.0</td>
      <td>419323.0</td>
      <td>415874.0</td>
      <td>835197.0</td>
      <td>2.527913</td>
      <td>False</td>
      <td>청주</td>
    </tr>
    <tr>
      <th>262</th>
      <td>충청북도</td>
      <td>충주시</td>
      <td>26600.0</td>
      <td>22757.0</td>
      <td>49357.0</td>
      <td>14407.0</td>
      <td>20383.0</td>
      <td>34790.0</td>
      <td>104877.0</td>
      <td>103473.0</td>
      <td>208350.0</td>
      <td>1.308249</td>
      <td>False</td>
      <td>충주</td>
    </tr>
    <tr>
      <th>263</th>
      <td>충청북도</td>
      <td>흥덕구</td>
      <td>40933.0</td>
      <td>37675.0</td>
      <td>78608.0</td>
      <td>9788.0</td>
      <td>13671.0</td>
      <td>23459.0</td>
      <td>127647.0</td>
      <td>125916.0</td>
      <td>253563.0</td>
      <td>3.211987</td>
      <td>False</td>
      <td>청주 흥덕</td>
    </tr>
  </tbody>
</table>

---

```py
del pop["20-39세남자"]
del pop["65세이상남자"]
del pop["65세이상여자"]
```

---

# 4. 지도 그리기(카르토그램)
```py
draw_korea_raw = pd.read_excel("../data/07_draw_korea_raw.xlsx")
draw_korea_raw
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>철원</td>
      <td>화천</td>
      <td>양구</td>
      <td>고성(강원)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>양주</td>
      <td>동두천</td>
      <td>연천</td>
      <td>포천</td>
      <td>의정부</td>
      <td>인제</td>
      <td>춘천</td>
      <td>속초</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>고양 덕양</td>
      <td>고양 일산동</td>
      <td>서울 도봉</td>
      <td>서울 노원</td>
      <td>남양주</td>
      <td>홍천</td>
      <td>횡성</td>
      <td>양양</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>파주</td>
      <td>고양 일산서</td>
      <td>김포</td>
      <td>서울 강북</td>
      <td>서울 성북</td>
      <td>가평</td>
      <td>구리</td>
      <td>하남</td>
      <td>정선</td>
      <td>강릉</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>부천 소사</td>
      <td>안양 만안</td>
      <td>광명</td>
      <td>서울 서대문</td>
      <td>서울 종로</td>
      <td>서울 동대문</td>
      <td>서울 중랑</td>
      <td>양평</td>
      <td>태백</td>
      <td>동해</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>인천 강화</td>
      <td>부천 원미</td>
      <td>안양 동안</td>
      <td>서울 은평</td>
      <td>서울 마포</td>
      <td>서울 중구</td>
      <td>서울 성동</td>
      <td>서울 강동</td>
      <td>여주</td>
      <td>원주</td>
      <td>삼척</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NaN</td>
      <td>인천 서구</td>
      <td>부천 오정</td>
      <td>시흥</td>
      <td>서울 강서</td>
      <td>서울 동작</td>
      <td>서울 용산</td>
      <td>서울 광진</td>
      <td>서울 송파</td>
      <td>이천</td>
      <td>평창</td>
      <td>울진</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>NaN</td>
      <td>인천 동구</td>
      <td>인천 계양</td>
      <td>안산 상록</td>
      <td>서울 양천</td>
      <td>서울 관악</td>
      <td>서울 서초</td>
      <td>성남 중원</td>
      <td>과천</td>
      <td>광주</td>
      <td>영월</td>
      <td>영덕</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>인천 부평</td>
      <td>안산 단원</td>
      <td>서울 영등포</td>
      <td>서울 금천</td>
      <td>서울 강남</td>
      <td>성남 분당</td>
      <td>성남 수정</td>
      <td>용인 수지</td>
      <td>문경</td>
      <td>봉화</td>
      <td>NaN</td>
      <td>울릉</td>
    </tr>
    <tr>
      <th>9</th>
      <td>NaN</td>
      <td>인천 중구</td>
      <td>인천 남구</td>
      <td>화성</td>
      <td>서울 구로</td>
      <td>군포</td>
      <td>의왕</td>
      <td>수원 영통</td>
      <td>용인 기흥</td>
      <td>용인 처인</td>
      <td>안동</td>
      <td>영양</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10</th>
      <td>인천 옹진</td>
      <td>인천 연수</td>
      <td>인천 남동</td>
      <td>오산</td>
      <td>안성</td>
      <td>수원 권선</td>
      <td>수원 장안</td>
      <td>제천</td>
      <td>예천</td>
      <td>영주</td>
      <td>구미</td>
      <td>청송</td>
      <td>포항 북구</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>태안</td>
      <td>아산</td>
      <td>천안 동남</td>
      <td>천안 서북</td>
      <td>평택</td>
      <td>음성</td>
      <td>수원 팔달</td>
      <td>단양</td>
      <td>상주</td>
      <td>김천</td>
      <td>군위</td>
      <td>의성</td>
      <td>포항 남구</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>NaN</td>
      <td>당진</td>
      <td>홍성</td>
      <td>예산</td>
      <td>공주</td>
      <td>진천</td>
      <td>충주</td>
      <td>청주 흥덕</td>
      <td>괴산</td>
      <td>칠곡</td>
      <td>영천</td>
      <td>경산</td>
      <td>경주</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13</th>
      <td>NaN</td>
      <td>서산</td>
      <td>보령</td>
      <td>청양</td>
      <td>세종</td>
      <td>대전 대덕</td>
      <td>증평</td>
      <td>청주 청원</td>
      <td>보은</td>
      <td>고령</td>
      <td>청도</td>
      <td>성주</td>
      <td>울산 북구</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>부여</td>
      <td>논산</td>
      <td>계룡</td>
      <td>대전 동구</td>
      <td>청주 상당</td>
      <td>청주 서원</td>
      <td>대구 북구</td>
      <td>대구 중구</td>
      <td>대구 수성</td>
      <td>울산 울주</td>
      <td>울산 동구</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>서천</td>
      <td>금산</td>
      <td>대전 유성</td>
      <td>대전 중구</td>
      <td>옥천</td>
      <td>영동</td>
      <td>대구 서구</td>
      <td>대구 남구</td>
      <td>대구 동구</td>
      <td>울산 중구</td>
      <td>울산 남구</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>군산</td>
      <td>익산</td>
      <td>대전 서구</td>
      <td>무주</td>
      <td>거창</td>
      <td>합천</td>
      <td>대구 달서</td>
      <td>대구 달성</td>
      <td>부산 금정</td>
      <td>부산 동래</td>
      <td>부산 기장</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>부안</td>
      <td>김제</td>
      <td>완주</td>
      <td>장수</td>
      <td>함양</td>
      <td>창녕</td>
      <td>밀양</td>
      <td>부산 북구</td>
      <td>부산 부산진</td>
      <td>부산 연제</td>
      <td>부산 해운대</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>18</th>
      <td>NaN</td>
      <td>고창</td>
      <td>정읍</td>
      <td>전주 덕진</td>
      <td>진안</td>
      <td>남원</td>
      <td>진주</td>
      <td>의령</td>
      <td>부산 강서</td>
      <td>부산 사상</td>
      <td>부산 동구</td>
      <td>부산 중구</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>NaN</td>
      <td>영광</td>
      <td>장성</td>
      <td>전주 완산</td>
      <td>임실</td>
      <td>산청</td>
      <td>함안</td>
      <td>양산</td>
      <td>창원 합포</td>
      <td>부산 서구</td>
      <td>부산 사하</td>
      <td>부산 남구</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20</th>
      <td>NaN</td>
      <td>함평</td>
      <td>담양</td>
      <td>순창</td>
      <td>구례</td>
      <td>하동</td>
      <td>창원 의창</td>
      <td>창원 성산</td>
      <td>창원 진해</td>
      <td>김해</td>
      <td>부산 영도</td>
      <td>부산 수영</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>신안</td>
      <td>무안</td>
      <td>광주 광산</td>
      <td>곡성</td>
      <td>화순</td>
      <td>광양</td>
      <td>사천</td>
      <td>창원 회원</td>
      <td>통영</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>22</th>
      <td>목포</td>
      <td>나주</td>
      <td>광주 서구</td>
      <td>광주 북구</td>
      <td>순천</td>
      <td>고흥</td>
      <td>남해</td>
      <td>고성(경남)</td>
      <td>거제</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>23</th>
      <td>해남</td>
      <td>영암</td>
      <td>광주 남구</td>
      <td>광주 동구</td>
      <td>여수</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>진도</td>
      <td>강진</td>
      <td>장흥</td>
      <td>보성</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>완도</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>제주</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>서귀포</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

---

```py
draw_korea_raw_stacked = pd.DataFrame(draw_korea_raw.stack())
draw_korea_raw_stacked
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">0</th>
      <th>7</th>
      <td>철원</td>
    </tr>
    <tr>
      <th>8</th>
      <td>화천</td>
    </tr>
    <tr>
      <th>9</th>
      <td>양구</td>
    </tr>
    <tr>
      <th>10</th>
      <td>고성(강원)</td>
    </tr>
    <tr>
      <th>1</th>
      <th>3</th>
      <td>양주</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">24</th>
      <th>2</th>
      <td>장흥</td>
    </tr>
    <tr>
      <th>3</th>
      <td>보성</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">25</th>
      <th>2</th>
      <td>완도</td>
    </tr>
    <tr>
      <th>5</th>
      <td>제주</td>
    </tr>
    <tr>
      <th>26</th>
      <th>5</th>
      <td>서귀포</td>
    </tr>
  </tbody>
</table>

---

```py
draw_korea_raw_stacked.reset_index(inplace=True)
draw_korea_raw_stacked
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>level_0</th>
      <th>level_1</th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>7</td>
      <td>철원</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>8</td>
      <td>화천</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>9</td>
      <td>양구</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>10</td>
      <td>고성(강원)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>3</td>
      <td>양주</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>247</th>
      <td>24</td>
      <td>2</td>
      <td>장흥</td>
    </tr>
    <tr>
      <th>248</th>
      <td>24</td>
      <td>3</td>
      <td>보성</td>
    </tr>
    <tr>
      <th>249</th>
      <td>25</td>
      <td>2</td>
      <td>완도</td>
    </tr>
    <tr>
      <th>250</th>
      <td>25</td>
      <td>5</td>
      <td>제주</td>
    </tr>
    <tr>
      <th>251</th>
      <td>26</td>
      <td>5</td>
      <td>서귀포</td>
    </tr>
  </tbody>
</table>

---

#### 콜럼 재정의
```py
draw_korea_raw_stacked.rename(
    columns={
    "level_0": "y",
    "level_1": "x",
    0: "ID"
    }, inplace=True
)

draw_korea = draw_korea_raw_stacked
draw_korea
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>y</th>
      <th>x</th>
      <th>ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>7</td>
      <td>철원</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>8</td>
      <td>화천</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>9</td>
      <td>양구</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>10</td>
      <td>고성(강원)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>3</td>
      <td>양주</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>247</th>
      <td>24</td>
      <td>2</td>
      <td>장흥</td>
    </tr>
    <tr>
      <th>248</th>
      <td>24</td>
      <td>3</td>
      <td>보성</td>
    </tr>
    <tr>
      <th>249</th>
      <td>25</td>
      <td>2</td>
      <td>완도</td>
    </tr>
    <tr>
      <th>250</th>
      <td>25</td>
      <td>5</td>
      <td>제주</td>
    </tr>
    <tr>
      <th>251</th>
      <td>26</td>
      <td>5</td>
      <td>서귀포</td>
    </tr>
  </tbody>
</table>

---

#### 우리나라 경계선
```py
BORDER_LINES = [
    [(5, 1), (5, 2), (7, 2), (7, 3), (11, 3), (11, 0)], # 인천
    [(5, 4), (5, 5), (2, 5), (2, 7), (4, 7), (4, 9), (7, 9), (7, 7), (9, 7), (9, 5), (10, 5), (10, 4), (5, 4)], # 서울
    [(1, 7), (1, 8), (3, 8), (3, 10), (10, 10), (10, 7), (12, 7), (12, 6), (11, 6), (11, 5), (12, 5), (12, 4), (11, 4), (11, 3)], # 경기도
    [(8, 10), (8, 11), (6, 11), (6, 12)], # 강원도
    [(12, 5), (13, 5), (13, 4), (14, 4), (14, 5), (15, 5),(15, 4), (16, 4), (16, 2)], # 충청북도
    [(16, 4), (17, 4), (17, 5), (16, 5), (16, 6), (19, 6), (19, 5), (20, 5), (20, 4), (21, 4), (21, 3), (19, 3), (19, 1)], # 전라북도
    [(13, 5), (13, 6), (16, 6)],
    [(13, 5), (14, 5)], # 대전시, 세종시
    [(21 ,2), (21, 3), (22, 3), (22, 4), (24, 4), (24, 2), (21, 2)], # 광주
    [(20, 5), (21, 5), (21, 6), (23, 6)], # 전라남도
    [(10, 8), (12, 8), (12, 9), (14, 9), (14, 8), (16, 8), (16, 6)], # 충청북도
    [(14, 9), (14, 11), (14, 12), (13, 12), (13, 13)], # 경상북도
    [(15, 8), (17, 8), (17, 10), (16, 10), (16, 11), (14, 11)], # 대구
    [(17, 9), (18, 9), (18, 8), (19, 8), (19, 9), (20, 9), (20, 10), (21, 10)], # 부산

    [(16, 11), (16, 13)],
    [(27, 5), (27, 6), (25, 6)]
]
```

---

```py
def plot_text_simple(draw_korea):
    for idx, row in draw_korea.iterrows():
        if len(row["ID"].split()) == 2:
            dispname = "{}\n{}".format(row["ID"].split()[0], row["ID"].split()[1])
        elif row["ID"][:2] == "고성":
            dispname = "고성"
        else:
            dispname = row["ID"]
        if len(dispname.splitlines()[-1]) >= 3:
            fontsize, linespacing = 9.5, 1.5
        else:
            fontsize, linespacing = 11, 1.2

        plt.annotate(
            dispname,
            (row["x"] + 0.5, row["y"] + 0.5),
            weight="bold",
            fontsize=fontsize,
            linespacing=linespacing,
            ha="center", # 수평 정렬
            va="center", # 수직 정렬

        )        
```

---

```py
def simpleDraw(draw_korea):
    plt.figure(figsize=(8, 11))
    plot_text_simple(draw_korea)

    for path in BORDER_LINES:
        ys, xs = zip(*path)
        plt.plot(xs, ys, c="black", lw=1.5)

    plt.gca().invert_yaxis()
    plt.axis("off")
    plt.tight_layout()
    plt.show()
```
```py
simpleDraw(draw_korea)
```
![](https://velog.velcdn.com/images/yy2hi/post/9f791e78-699a-47c7-afcb-ab5c2bbbb037/image.png)

---

#### 검증 작업
```py
set(draw_korea["ID"].unique()) - set(pop["ID"].unique())

=>

set()
```

---

```py
set(pop["ID"].unique()) - set(draw_korea["ID"].unique())

=>

{'고양', '부천', '성남', '수원', '안산', '안양', '용인', '전주', '창원', '천안', '청주', '포항'}
```

---

```py
tmp_list = list(set(pop["ID"].unique()) - set(draw_korea["ID"].unique()))

for tmp in tmp_list:
    pop = pop.drop(pop[pop["ID"] == tmp].index)
print(set(pop["ID"].unique()) - set(draw_korea["ID"].unique()))

=>

set()
```

---

#### merge()
```py
pop = pd.merge(pop, draw_korea, how="left", on="ID")
pop.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align:right">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>20-39세여자</th>
      <th>20-39세합계</th>
      <th>65세이상합계</th>
      <th>인구수남자</th>
      <th>인구수여자</th>
      <th>인구수합계</th>
      <th>소멸비율</th>
      <th>소멸위기지역</th>
      <th>ID</th>
      <th>y</th>
      <th>x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>강원도</td>
      <td>강릉시</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
      <td>1.226041</td>
      <td>0</td>
      <td>강릉</td>
      <td>3</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>강원도</td>
      <td>고성군</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
      <td>0.707314</td>
      <td>1</td>
      <td>고성(강원)</td>
      <td>0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>강원도</td>
      <td>동해시</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
      <td>1.289738</td>
      <td>0</td>
      <td>동해</td>
      <td>4</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>강원도</td>
      <td>삼척시</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
      <td>0.973990</td>
      <td>1</td>
      <td>삼척</td>
      <td>5</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>강원도</td>
      <td>속초시</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
      <td>1.372647</td>
      <td>0</td>
      <td>속초</td>
      <td>1</td>
      <td>10</td>
    </tr>
  </tbody>
</table>

---

### 그림 그리기 위한 데이터를 계산하는 함수
- 색상을 만들 때, 최소값을 흰색
- blockedMap: 인구현황(pop)
- targetData: 그리고싶은 컬럼

```py
def get_data_info(targetData, blockedMap):
    whitelabelmin = (
        max(blockedMap[targetData]) - min(blockedMap[targetData])
        
    ) * 0.25 + min(blockedMap[targetData])
    
    vmin = min(blockedMap[targetData])
    vmax = max(blockedMap[targetData])

    mapdata = blockedMap.pivot_table(index="y", columns="x", values=targetData)

    return mapdata, vmax, vmin, whitelabelmin
```
```py
def get_data_info_for_zero_center(targetData, blockedMap):
    whitelabelmin = 5
    tmp_max = max(
        [np.abs(min(blockedMap[targetData])), np.abs(max(blockedMap[targetData]))]
    )

    vmin, vmax = -tmp_max, tmp_max

    mapdata = blockedMap.pivot_table(index="y", columns="x", values=targetData)

    return mapdata, vmax, vmin, whitelabelmin
```
```py
def plot_text(targetData, blockedMap, whitelabelmin):
    for idx, row in blockedMap.iterrows():
        if len(row["ID"].split()) == 2:
            dispname = "{}\n{}".format(row["ID"].split()[0], row["ID"].split()[1])
        elif row["ID"][:2] == "고성":
            dispname = "고성"
        else:
            dispname = row["ID"]
        if len(dispname.splitlines()[-1]) >= 3:
            fontsize, linespacing = 9.5, 1.5
        else:
            fontsize, linespacing = 11, 1.2

        annocolor = "white" if np.abs(row[targetData]) > whitelabelmin else "black"

        plt.annotate(
            dispname,
            (row["x"] + 0.5, row["y"] + 0.5),
            weight="bold",
            color=annocolor,
            fontsize=fontsize,
            linespacing=linespacing,
            ha="center", # 수평 정렬
            va="center", # 수직 정렬

        )        
```
```py
def drawKorea(targetData, blockedMap, cmapname, zeroCenter=False):
    if zeroCenter:
        masked_mapdata, vmax, vmin, whitelabelmin = get_data_info_for_zero_center(targetData, blockedMap)

    if not zeroCenter:
        masked_mapdata, vmax, vmin, whitelabelmin = get_data_info(targetData, blockedMap)

    plt.figure(figsize=(8, 11))
    plt.pcolor(masked_mapdata, vmin=vmin, vmax=vmax, cmap=cmapname, edgecolor="#aaaaaa", linewidth=0.5)

    plot_text(targetData, blockedMap, whitelabelmin)

    for path in BORDER_LINES:
        ys, xs = zip(*path)
        plt.plot(xs, ys, c="black", lw=1.5)

    plt.gca().invert_yaxis()
    plt.axis("off")
    plt.tight_layout()
    cb = plt.colorbar(shrink=0.1, aspect=10)
    cb.set_label(targetData)
    plt.show()
```

---

```py
drawKorea("인구수합계", pop, "Blues")
```
![](https://velog.velcdn.com/images/yy2hi/post/b2cf7b02-6cd2-4114-88f4-bfdc774275d6/image.png)

---

```py
pop["소멸위기지역"]

=>

0      0
1      1
2      0
3      1
4      0
      ..
247    0
248    0
249    0
250    0
251    0
Name: 소멸위기지역, Length: 252, dtype: int64
```

---

#### 소멸 위기 지역
```py
pop["소멸위기지역"] = [1 if con else 0 for con in pop["소멸위기지역"]]
drawKorea("소멸위기지역", pop, "Reds")
```
![](https://velog.velcdn.com/images/yy2hi/post/d9ff3643-b39b-4ccc-8c71-b869ebbf57eb/image.png)

---

#### 여성비
```py
pop["여성비"] = (pop["인구수여자"] / pop["인구수합계"] - 0.5) * 100
drawKorea("여성비", pop, "RdBu", zeroCenter=True)
```
![](https://velog.velcdn.com/images/yy2hi/post/505d2775-9310-4020-9c01-79328e7fed72/image.png)

---

#### 2030여성비
```py
pop["2030여성비"] = (pop["20-39세여자"] / pop["20-39세합계"] - 0.5) * 100
drawKorea("2030여성비", pop, "RdBu", zeroCenter=True)
```
![](https://velog.velcdn.com/images/yy2hi/post/16900653-e7fc-4c5c-a98b-fc0a055b28f6/image.png)


---

# 5. folium
```py
import folium
import json

pop_folium = pop.set_index("ID")
pop_folium
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>광역시도</th>
      <th>시도</th>
      <th>20-39세여자</th>
      <th>20-39세합계</th>
      <th>65세이상합계</th>
      <th>인구수남자</th>
      <th>인구수여자</th>
      <th>인구수합계</th>
      <th>소멸비율</th>
      <th>소멸위기지역</th>
      <th>y</th>
      <th>x</th>
      <th>여성비</th>
      <th>2030여성비</th>
    </tr>
    <tr>
      <th>ID</th>
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
      <th>강릉</th>
      <td>강원도</td>
      <td>강릉시</td>
      <td>23098.0</td>
      <td>49384.0</td>
      <td>37679.0</td>
      <td>106231.0</td>
      <td>107615.0</td>
      <td>213846.0</td>
      <td>1.226041</td>
      <td>0</td>
      <td>3</td>
      <td>11</td>
      <td>0.323597</td>
      <td>-3.227766</td>
    </tr>
    <tr>
      <th>고성(강원)</th>
      <td>강원도</td>
      <td>고성군</td>
      <td>2529.0</td>
      <td>7023.0</td>
      <td>7151.0</td>
      <td>15899.0</td>
      <td>14215.0</td>
      <td>30114.0</td>
      <td>0.707314</td>
      <td>1</td>
      <td>0</td>
      <td>10</td>
      <td>-2.796042</td>
      <td>-13.989748</td>
    </tr>
    <tr>
      <th>동해</th>
      <td>강원도</td>
      <td>동해시</td>
      <td>9753.0</td>
      <td>21264.0</td>
      <td>15124.0</td>
      <td>47166.0</td>
      <td>46131.0</td>
      <td>93297.0</td>
      <td>1.289738</td>
      <td>0</td>
      <td>4</td>
      <td>11</td>
      <td>-0.554680</td>
      <td>-4.133747</td>
    </tr>
    <tr>
      <th>삼척</th>
      <td>강원도</td>
      <td>삼척시</td>
      <td>7115.0</td>
      <td>15823.0</td>
      <td>14610.0</td>
      <td>35253.0</td>
      <td>34346.0</td>
      <td>69599.0</td>
      <td>0.973990</td>
      <td>1</td>
      <td>5</td>
      <td>11</td>
      <td>-0.651590</td>
      <td>-5.033812</td>
    </tr>
    <tr>
      <th>속초</th>
      <td>강원도</td>
      <td>속초시</td>
      <td>8752.0</td>
      <td>18708.0</td>
      <td>12752.0</td>
      <td>40288.0</td>
      <td>41505.0</td>
      <td>81793.0</td>
      <td>1.372647</td>
      <td>0</td>
      <td>1</td>
      <td>10</td>
      <td>0.743951</td>
      <td>-3.217875</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>증평</th>
      <td>충청북도</td>
      <td>증평군</td>
      <td>4554.0</td>
      <td>10085.0</td>
      <td>5323.0</td>
      <td>19110.0</td>
      <td>18198.0</td>
      <td>37308.0</td>
      <td>1.711065</td>
      <td>0</td>
      <td>13</td>
      <td>6</td>
      <td>-1.222258</td>
      <td>-4.843827</td>
    </tr>
    <tr>
      <th>진천</th>
      <td>충청북도</td>
      <td>진천군</td>
      <td>7622.0</td>
      <td>17013.0</td>
      <td>11306.0</td>
      <td>36387.0</td>
      <td>33563.0</td>
      <td>69950.0</td>
      <td>1.348311</td>
      <td>0</td>
      <td>12</td>
      <td>5</td>
      <td>-2.018585</td>
      <td>-5.198965</td>
    </tr>
    <tr>
      <th>청주 청원</th>
      <td>충청북도</td>
      <td>청원구</td>
      <td>27805.0</td>
      <td>60021.0</td>
      <td>20331.0</td>
      <td>97006.0</td>
      <td>93807.0</td>
      <td>190813.0</td>
      <td>2.735232</td>
      <td>0</td>
      <td>13</td>
      <td>7</td>
      <td>-0.838255</td>
      <td>-3.674547</td>
    </tr>
    <tr>
      <th>충주</th>
      <td>충청북도</td>
      <td>충주시</td>
      <td>22757.0</td>
      <td>49357.0</td>
      <td>34790.0</td>
      <td>104877.0</td>
      <td>103473.0</td>
      <td>208350.0</td>
      <td>1.308249</td>
      <td>0</td>
      <td>12</td>
      <td>6</td>
      <td>-0.336933</td>
      <td>-3.893065</td>
    </tr>
    <tr>
      <th>청주 흥덕</th>
      <td>충청북도</td>
      <td>흥덕구</td>
      <td>37675.0</td>
      <td>78608.0</td>
      <td>23459.0</td>
      <td>127647.0</td>
      <td>125916.0</td>
      <td>253563.0</td>
      <td>3.211987</td>
      <td>0</td>
      <td>12</td>
      <td>7</td>
      <td>-0.341335</td>
      <td>-2.072308</td>
    </tr>
  </tbody>
</table>

---

#### 인구수합계 지도시각화
```py
geo_path = "../data/07_skorea_municipalities_geo_simple.json"
geo_str = json.load(open(geo_path, encoding="utf-8"))

mymap = folium.Map(location=[36.2002, 127.054], zoom_start=7)
mymap.choropleth(
    geo_data=geo_str,
    data=pop_folium["인구수합계"],
    key_on="feature.id",
    columns=[pop_folium.index, pop_folium["인구수합계"]],
    fill_color="YlGnBu"
)
mymap
```
![](https://velog.velcdn.com/images/yy2hi/post/68c1fa54-f339-4bd0-b24b-6b0449685f5b/image.png)


---

#### 소멸 위기 지역 지도시각화
```py
mymap = folium.Map(location=[36.2002, 127.054], zoom_start=7)
mymap.choropleth(
    geo_data=geo_str,
    data=pop_folium["소멸위기지역"],
    key_on="feature.id",
    columns=[pop_folium.index, pop_folium["소멸위기지역"]],
    fill_color="PuRd"
)
mymap
```
![](https://velog.velcdn.com/images/yy2hi/post/3adb5b8a-08fc-4f84-b148-64f4a7b3c5d5/image.png)
