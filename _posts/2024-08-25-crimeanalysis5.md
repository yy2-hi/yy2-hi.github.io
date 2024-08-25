---
layout: single
title: "Project 2 - 서울시 범죄 현황 데이터 분석 (5)"
categories: Data Analysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 서울시 범죄 현황에 대한 지도 시각화
```py
crime_anal_norm.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>살인</th>
      <th>강도</th>
      <th>강간</th>
      <th>절도</th>
      <th>폭력</th>
      <th>강간검거율</th>
      <th>강도검거율</th>
      <th>살인검거율</th>
      <th>절도검거율</th>
      <th>폭력검거율</th>
      <th>인구수</th>
      <th>CCTV</th>
      <th>범죄</th>
      <th>검거</th>
    </tr>
    <tr>
      <th>구별</th>
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
      <th>중구</th>
      <td>0.214286</td>
      <td>0.205128</td>
      <td>0.383721</td>
      <td>0.585671</td>
      <td>0.407957</td>
      <td>74.747475</td>
      <td>87.5</td>
      <td>100.0</td>
      <td>42.511628</td>
      <td>89.707865</td>
      <td>134593</td>
      <td>1023</td>
      <td>0.359353</td>
      <td>78.893394</td>
    </tr>
    <tr>
      <th>중랑구</th>
      <td>0.571429</td>
      <td>0.358974</td>
      <td>0.317829</td>
      <td>0.460637</td>
      <td>0.580125</td>
      <td>91.463415</td>
      <td>100.0</td>
      <td>87.5</td>
      <td>62.211709</td>
      <td>85.714286</td>
      <td>412780</td>
      <td>916</td>
      <td>0.457799</td>
      <td>85.377882</td>
    </tr>
  </tbody>
</table>

---

### 살인발생 건수 지도 시각화
```py
my_map = folium.Map(
    location=[37.5502, 126.982],
    zoom_start=11,
    tiles="Stamen Toner"
)

folium.Choropleth(
    geo_data=geo_str, # 우리나라 경계선 좌표값이 담긴 데이터
    data=crime_anal_norm["살인"],
    columns=[crime_anal_norm.index, crime_anal_norm["살인"]],
    key_on="feature.id",
    fill_color="PuRd",
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name="정규화된 살인 발생 건수"
).add_to(my_map)

my_map
```
![](https://velog.velcdn.com/images/yy2hi/post/3b85aa2c-93cd-4368-8f38-3ca4a4061aa1/image.png)

---

### 성범죄 건수 지도 시각화

```py
my_map = folium.Map(
    location=[37.5502, 126.982],
    zoom_start=11,
    tiles="Stamen Toner"
)

folium.Choropleth(
    geo_data=geo_str, # 우리나라 경계선 좌표값이 담긴 데이터
    data=crime_anal_norm["강간"],
    columns=[crime_anal_norm.index, crime_anal_norm["강간"]],
    key_on="feature.id",
    fill_color="PuRd",
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name="정규화된 강간 발생 건수"
).add_to(my_map)

my_map
```
![](https://velog.velcdn.com/images/yy2hi/post/7a87307b-3e94-4480-b936-fafee18c56b9/image.png)

### 5대 범죄 건수 지도 시각화

```py

my_map = folium.Map(
    location=[37.5502, 126.982],
    zoom_start=11,
    tiles="Stamen Toner"
)

folium.Choropleth(
    geo_data=geo_str, # 우리나라 경계선 좌표값이 담긴 데이터
    data=crime_anal_norm["범죄"],
    columns=[crime_anal_norm.index, crime_anal_norm["범죄"]],
    key_on="feature.id",
    fill_color="PuRd",
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name="정규화된 5대 범죄 발생 건수"
).add_to(my_map)

my_map
```
![](https://velog.velcdn.com/images/yy2hi/post/ab8b44ee-137a-4ad5-89ae-4081a065b319/image.png)

---

### 인구 대비 범죄 발생 건수
```py
tmp_criminal = crime_anal_norm["범죄"] / crime_anal_norm["인구수"]

my_map = folium.Map(
    location=[37.5502, 126.982],
    zoom_start=11,
    tiles="Stamen Toner"
)

folium.Choropleth(
    geo_data=geo_str, # 우리나라 경계선 좌표값이 담긴 데이터
    data=tmp_criminal,
    columns=[crime_anal_norm.index, tmp_criminal],
    key_on="feature.id",
    fill_color="PuRd",
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name="인구 대비 범죄 발생 건수"
).add_to(my_map)

my_map
```
![](https://velog.velcdn.com/images/yy2hi/post/f2277223-2c26-4377-800e-c1f9564b5106/image.png)

---

#### 경찰서별 정보 범죄발생과 함께 정리
```py
crime_anal_station = pd.read_csv(
    "../data/02. crime_in_Seoul_1st.csv", encoding="utf-8"
)

crime_anal_station.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>구분</th>
      <th>강간검거</th>
      <th>강간발생</th>
      <th>강도검거</th>
      <th>강도발생</th>
      <th>살인검거</th>
      <th>살인발생</th>
      <th>절도검거</th>
      <th>절도발생</th>
      <th>폭력검거</th>
      <th>폭력발생</th>
      <th>구별</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29</th>
      <td>29</td>
      <td>중부</td>
      <td>96.0</td>
      <td>141.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>485.0</td>
      <td>1204.0</td>
      <td>1164.0</td>
      <td>1335.0</td>
      <td>중구</td>
      <td>37.563646</td>
      <td>126.989580</td>
    </tr>
    <tr>
      <th>30</th>
      <td>30</td>
      <td>혜화</td>
      <td>64.0</td>
      <td>101.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>379.0</td>
      <td>988.0</td>
      <td>842.0</td>
      <td>972.0</td>
      <td>종로구</td>
      <td>37.571840</td>
      <td>126.998856</td>
    </tr>
  </tbody>
</table>

```py
col = ["살인검거", "강도검거", "강간검거", "절도검거", "폭력검거"]
tmp = crime_anal_station[col] / crime_anal_station[col].max() # 정규화
crime_anal_station["검거"] = np.mean(tmp, axis=1) # numpy axis=1 : 행(가로), pandas axis=1 : 열(세로)
crime_anal_station.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>구분</th>
      <th>강간검거</th>
      <th>강간발생</th>
      <th>강도검거</th>
      <th>강도발생</th>
      <th>살인검거</th>
      <th>살인발생</th>
      <th>절도검거</th>
      <th>절도발생</th>
      <th>폭력검거</th>
      <th>폭력발생</th>
      <th>구별</th>
      <th>lat</th>
      <th>lng</th>
      <th>검거</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>29</th>
      <td>29</td>
      <td>중부</td>
      <td>96.0</td>
      <td>141.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>485.0</td>
      <td>1204.0</td>
      <td>1164.0</td>
      <td>1335.0</td>
      <td>중구</td>
      <td>37.563646</td>
      <td>126.989580</td>
      <td>0.277182</td>
    </tr>
    <tr>
      <th>30</th>
      <td>30</td>
      <td>혜화</td>
      <td>64.0</td>
      <td>101.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>379.0</td>
      <td>988.0</td>
      <td>842.0</td>
      <td>972.0</td>
      <td>종로구</td>
      <td>37.571840</td>
      <td>126.998856</td>
      <td>0.240065</td>
    </tr>
  </tbody>
</table>

---

## 경찰서 위치 마커 표시
```py
my_map = folium.Map(
    location=[37.5502, 126.982], zoom_start=11
)

for idx, rows in crime_anal_station.iterrows():
    folium.Marker(
        location=[rows["lat"], rows["lng"]]
    ).add_to(my_map)
    
my_map
```
![](https://velog.velcdn.com/images/yy2hi/post/15120575-b86e-4e58-9652-0a3ab02d2eea/image.png)

---

#### 경찰서 검거율 원에 적용
```py
my_map = folium.Map(
    location=[37.5502, 126.982], zoom_start=11
)

folium.Choropleth(
    geo_data=geo_str,
    data=crime_anal_norm["범죄"],
    columns=[crime_anal_norm.index, crime_anal_norm["범죄"]],
    key_on="feature.id",
    fill_color="PuRd",
    fill_opacity=0.7,
    line_opacity=0.2,
    
).add_to(my_map)

for idx, rows in crime_anal_station.iterrows():
    folium.CircleMarker(
        location=[rows["lat"], rows["lng"]],
        radius=rows["검거"] * 50,
        popup=rows["구분"] + " : " + "%.2f" % rows["검거"],
        color="#3186cc",
        fill=True,
        fill_color="#3186cc"
    ).add_to(my_map)
    
my_map
```
![](https://velog.velcdn.com/images/yy2hi/post/2c613cce-3537-44bc-b040-43ea453b53c8/image.png)

---

## 서울시 범죄 현황 발생 장소 분석
#### 추가 검증
```py
crime_loc_raw = pd.read_csv(
    "../data/02. crime_in_Seoul_location.csv", thousands=",", encoding="euc-kr"
)

crime_loc_raw.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>범죄명</th>
      <th>장소</th>
      <th>발생건수</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>63</th>
      <td>폭력</td>
      <td>금융기관</td>
      <td>42</td>
    </tr>
    <tr>
      <th>64</th>
      <td>폭력</td>
      <td>기타</td>
      <td>26382</td>
    </tr>
  </tbody>
</table>

```py
crime_loc_raw.범죄명.unique()

=>

array(['살인', '강도', '강간.추행', '절도', '폭력'], dtype=object)
---------------------------------------------------------------
crime_loc_raw["장소"].unique()

=>

array(['아파트, 연립 다세대', '단독주택', '노상', '상점', '숙박업소, 목욕탕', '유흥 접객업소', '사무실',
       '역, 대합실', '교통수단', '유원지 ', '학교', '금융기관', '기타'], dtype=object)
--------------------------------------------------------------------------------------------------
crime_loc = crime_loc_raw.pivot_table(
    crime_loc_raw, index="장소", columns="범죄명", aggfunc=[np.sum]
)

crime_loc.columns = crime_loc.columns.droplevel([0, 1])
crime_loc.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>범죄명</th>
      <th>강간.추행</th>
      <th>강도</th>
      <th>살인</th>
      <th>절도</th>
      <th>폭력</th>
    </tr>
    <tr>
      <th>장소</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>유흥 접객업소</th>
      <td>398</td>
      <td>13</td>
      <td>8</td>
      <td>2035</td>
      <td>2645</td>
    </tr>
    <tr>
      <th>학교</th>
      <td>33</td>
      <td>0</td>
      <td>0</td>
      <td>400</td>
      <td>203</td>
    </tr>
  </tbody>
</table>

```py
col = ["살인", "강도", "강간", "절도", "폭력"]
crime_loc_norm = crime_loc / crime_loc.max() # 정규화
crime_loc_norm.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>범죄명</th>
      <th>강간.추행</th>
      <th>강도</th>
      <th>살인</th>
      <th>절도</th>
      <th>폭력</th>
    </tr>
    <tr>
      <th>장소</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>교통수단</th>
      <td>0.324718</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.021027</td>
      <td>0.008415</td>
    </tr>
    <tr>
      <th>금융기관</th>
      <td>0.000940</td>
      <td>0.011494</td>
      <td>0.015385</td>
      <td>0.049738</td>
      <td>0.001592</td>
    </tr>
    <tr>
      <th>기타</th>
      <td>1.000000</td>
      <td>0.770115</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>노상</th>
      <td>0.463346</td>
      <td>1.000000</td>
      <td>0.338462</td>
      <td>0.429235</td>
      <td>0.929990</td>
    </tr>
    <tr>
      <th>단독주택</th>
      <td>0.185620</td>
      <td>0.172414</td>
      <td>0.461538</td>
      <td>0.103110</td>
      <td>0.135661</td>
    </tr>
  </tbody>
</table>

```py
crime_loc_norm["종합"] = np.mean(crime_loc_norm, axis=1)
crime_loc_norm.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>범죄명</th>
      <th>강간.추행</th>
      <th>강도</th>
      <th>살인</th>
      <th>절도</th>
      <th>폭력</th>
      <th>종합</th>
    </tr>
    <tr>
      <th>장소</th>
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
      <th>유흥 접객업소</th>
      <td>0.187030</td>
      <td>0.149425</td>
      <td>0.123077</td>
      <td>0.093632</td>
      <td>0.100258</td>
      <td>0.130684</td>
    </tr>
    <tr>
      <th>학교</th>
      <td>0.015508</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.018404</td>
      <td>0.007695</td>
      <td>0.008321</td>
    </tr>
  </tbody>
</table>

---

## 장소별 범죄 발생 장소
```py
crime_loc_norm_sort = crime_loc_norm.sort_values("종합", ascending=False) # 내림차순

def drawGraph():
    plt.figure(figsize=(10, 10))
    sns.heatmap(
        crime_loc_norm_sort,
        annot=True,
        fmt="f",
        linewidths=0.5,
        cmap="RdPu")
    plt.title("범죄 발생 장소")
    plt.show()
drawGraph()
```
![](https://velog.velcdn.com/images/yy2hi/post/de2a44d6-2ed0-4bf5-813f-04f3b43bb786/image.png)

