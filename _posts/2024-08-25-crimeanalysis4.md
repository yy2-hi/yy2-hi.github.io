---
layout: single
title: "Project 2 - 서울시 범죄 현황 데이터 분석 (4)"
categories: Data Analysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 서울시 범죄현황 데이터 시각화

```py
crime_anal_norm.head()
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
      <th>강남구</th>
      <td>0.357143</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.977118</td>
      <td>0.733773</td>
      <td>80.038760</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>53.470867</td>
      <td>88.130935</td>
      <td>561052</td>
      <td>3238</td>
      <td>0.813607</td>
      <td>84.328112</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>0.285714</td>
      <td>0.358974</td>
      <td>0.310078</td>
      <td>0.477799</td>
      <td>0.463880</td>
      <td>95.000000</td>
      <td>92.857143</td>
      <td>100.000000</td>
      <td>51.425314</td>
      <td>86.996047</td>
      <td>440359</td>
      <td>1010</td>
      <td>0.379289</td>
      <td>85.255701</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>0.500000</td>
      <td>0.128205</td>
      <td>0.420543</td>
      <td>0.332879</td>
      <td>0.509351</td>
      <td>73.271889</td>
      <td>80.000000</td>
      <td>85.714286</td>
      <td>54.991817</td>
      <td>89.344852</td>
      <td>328002</td>
      <td>831</td>
      <td>0.378196</td>
      <td>76.664569</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>0.428571</td>
      <td>0.307692</td>
      <td>0.624031</td>
      <td>0.572868</td>
      <td>0.593143</td>
      <td>81.987578</td>
      <td>83.333333</td>
      <td>100.000000</td>
      <td>44.555397</td>
      <td>83.678516</td>
      <td>520929</td>
      <td>2109</td>
      <td>0.505261</td>
      <td>78.710965</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>0.285714</td>
      <td>0.282051</td>
      <td>0.540698</td>
      <td>0.718060</td>
      <td>0.438577</td>
      <td>83.870968</td>
      <td>54.545455</td>
      <td>100.000000</td>
      <td>40.098634</td>
      <td>84.071906</td>
      <td>372298</td>
      <td>878</td>
      <td>0.453020</td>
      <td>72.517393</td>
    </tr>
  </tbody>
</table>

---

#### pairplot 강도, 살인, 폭력에 대한 상관관계 확인

```py
sns.pairplot(data=crime_anal_norm, vars=["살인", "강도", "폭력"], kind="reg", height=3);
```
![](https://velog.velcdn.com/images/yy2hi/post/e522a34b-30ef-48bc-80e1-62426c3902ca/image.png)

---

#### "인구수", "CCTV"와 "살인", "강도"의 상관관계 확인

```py
def drawGraph():
    sns.pairplot(data=crime_anal_norm, 
                 x_vars=["인구수", "CCTV"],
                 y_vars=["살인", "강도"], 
                 kind="reg",
                 height=4)
    plt.show()
drawGraph()
```
![](https://velog.velcdn.com/images/yy2hi/post/cecab182-56af-4d9f-8fac-7a5994f3db7e/image.png)

---

#### "인구수", "CCTV" 와 "살인검거율", "폭력검거율"의 상관관계 확인
```py
def drawGraph():
    sns.pairplot(data=crime_anal_norm, 
                 x_vars=["인구수", "CCTV"],
                 y_vars=["살인검거율", "폭력검거율"], 
                 kind="reg",
                 height=4)
    plt.show()
drawGraph()
```
![](https://velog.velcdn.com/images/yy2hi/post/6f4b7140-7a64-42b1-850f-501bb96bab17/image.png)

---

#### "인구수", "CCTV" 와 "절도검거율", "강도검거율"의 상관관계 확인
```py

def drawGraph():
    sns.pairplot(data=crime_anal_norm, 
                 x_vars=["인구수", "CCTV"],
                 y_vars=["절도검거율", "강도검거율"], 
                 kind="reg",
                 height=4)
    plt.show()
drawGraph()
```
![](https://velog.velcdn.com/images/yy2hi/post/ce440d41-a3fe-4de2-80d0-3cf1249bf472/image.png)

---

#### 검거율 heatmap
```py
# "검거" 컬럼을 기준으로 정렬

def drawGraph():
    
    # 데이터 프레임 생성
    target_col = ["강간검거율", "강도검거율", "살인검거율", "절도검거율", "폭력검거율", "검거"]
    crime_anal_norm_sort = crime_anal_norm.sort_values(by="검거", ascending=False) # 내림차순
    
    # 그래프 설정
    plt.figure(figsize=(10, 10))
    sns.heatmap(
        data=crime_anal_norm_sort[target_col],
        annot=True, # 데이터 값 표현
        fmt="f", # d: 정수, f: 실수
        linewidths=0.5, # 간격설정
        cmap="RdPu"
    )
    plt.title("범죄 검거 비율(정규화된 검거의 합으로 정렬)")
    plt.show()
drawGraph()    
```
![](https://velog.velcdn.com/images/yy2hi/post/9dc905b1-4088-4a01-a7aa-b3bcc5e37254/image.png)

---

#### 범죄발생 건수 heatmap
```py
# "범죄" 컬럼을 기준으로 정렬

def drawGraph():
    
    # 데이터 프레임 생성
    target_col = ["살인", "강도", "강간", "절도", "폭력", "범죄"]
    crime_anal_norm_sort = crime_anal_norm.sort_values(by="범죄", ascending=False) # 내림차순
    
    # 그래프 설정
    plt.figure(figsize=(10, 10))
    sns.heatmap(
        data=crime_anal_norm_sort[target_col],
        annot=True, # 데이터값 표현
        fmt="f", # 실수값으로 표현
        linewidth=0.5, # 간격설정
        cmap="RdPu"
    )
    plt.title("범죄 비율(정규화된 발생 건수로 정렬)")
    plt.show()
drawGraph()
```
![](https://velog.velcdn.com/images/yy2hi/post/ade88aba-4b34-4c4e-bbe8-16d619404f36/image.png)

---

#### 데이터 저장

```py
crime_anal_norm.to_csv("../data/02. crime_in_Seoul_final.csv", sep=",", encoding="utf-8")
```
---

## folium
### folium.Map()
- location: tuple or list, default None Latitude and Longitude of Map (Northing, Easting).
```py
m = folium.Map(location=[37.5665512, 126.97805437], zoom_start=18) # 0 ~ 18
m
```
![](https://velog.velcdn.com/images/yy2hi/post/0b713c64-cce9-41ca-9a3e-4751ca7a8d33/image.png)

---

#### save("path")
```py
m.save("./folium.html")
```

---

#### tiles option

- "OpenStreetMap"
- "Mapbox Bright" (Limited levels of zoom for free tiles)
- "Mapbox Control Room" (Limited levels of zoom for free tiles)
- "Stamen" (Terrain, Toner, and Watercolor)
- "Cloudmade" (Must pass API key)
- "Mapbox" (Must pass API key)
- "CartoDB" (positron and dark_matter)
```py
m = folium.Map(
    location=[37.5665512, 126.97805437],
    zoom_start=14, # 0 ~ 18
    tiles="openstreetmap"
    )
folium.Marker((37.56583779,126.97512197)).add_to(m)
folium.Marker(
    location=[37.5665512, 126.97805437],
    popup="<b>서울</b>",
    tooltip="<i>서울</i>"
).add_to(m)
m

folium.Marker(
    location=[37.54878936,126.973356],
    popup="<a href='https://zero-base.co.kr/' target=_'blink'>제로베이스</a>",
    tooltip="<i>Zerobase</i>"
).add_to(m)
m
```
![](https://velog.velcdn.com/images/yy2hi/post/5cafd53d-8bb5-45a7-bc67-63692167196a/image.png)

---

### folium.Marker()
- 지도에 마커 생성

```py
m = folium.Map(
    location=[37.5665512, 126.97805437],
    zoom_start=14, # 0 ~ 18
    tiles="openstreetmap"
    )
folium.Marker((37.56583779,126.97512197)).add_to(m)
folium.Marker(
    location=[37.5665512, 126.97805437],
    popup="<b>서울</b>",
    tooltip="<i>서울</i>"
).add_to(m)
m

folium.Marker(
    location=[37.54878936,126.973356],
    popup="<a href='https://zero-base.co.kr/' target=_'blink'>제로베이스</a>",
    tooltip="<i>Zerobase</i>"
).add_to(m)
m
```
![](https://velog.velcdn.com/images/yy2hi/post/7e388b34-bd47-4dae-b90e-d7a2264226d8/image.png)

---

### folium.Icon()
- https://www.w3schools.com/bootstrap/bootstrap_ref_comp_glyphs.asp
- https://fontawesome.com/icons

```py
m = folium.Map(
    location=[37.5665512, 126.97805437],
    zoom_start=13, # 0 ~ 18
    tiles="openstreetmap"
    )

# icon basic
folium.Marker(
(37.54878936,126.97336),
icon=folium.Icon(color="black", icon='info-sign')
).add_to(m)

# icon _color
folium.Marker(
    (37.54712, 127.047219),
    popup="<b>Subway</b>",
    tooltip="icon_color",
    icon=folium.Icon(
        color="red",
        icon_color="pink",
        icon="cloud"
    )
).add_to(m)

# Icon custom
folium.Marker(
    location=[37.540372,127.069276],
    popup="건대입구역",
    tooltip="Icon custiom",
    icon=folium.Icon(
        color="purple",
        icon_color="white",
        icon="glyphicon-cloud",
        angle=50,
        prefix="glyphicon") # glyphicon
).add_to(m)

# tooltip
folium.Marker(
    location=[37.544569,127.055974],
    popup="<b>Subway</b>",
    tooltip="<i>성수역</i>"
).add_to(m)

# html
folium.Marker(
    location=[37.5030426,127.041588],
    popup="<a href='https://zero-base.co.kr/' target=_'blink'>제로베이스</a>",
    tooltip="<i>Zerobase</i>"
).add_to(m)

m
```
![](https://velog.velcdn.com/images/yy2hi/post/ae25071a-ce33-4e3e-b434-e70c10b9b6f7/image.png)

---

### folium.ClickForMarker()
- 지도위에 마우스로 클릭했을 때 마커 생성
```py
m = folium.Map(
    location=[37.5445, 127.0558],
    zoom_start=14,
    tile="OpenStreetMap"
)

m.add_child(folium.ClickForMarker(popup="ClickForMarker"))
```
![](https://velog.velcdn.com/images/yy2hi/post/346920ba-4883-4788-8156-858e9c1fde07/image.png)

---

### folium.LatLngPopup()
- 지도를 마우스로 클릭했을 때 위도 경도 정보 반환
```py
m = folium.Map(
    location=[37.5445, 127.0558],
    zoom_start=14,
    tile="OpenStreetMap"
)

m.add_child(folium.LatLngPopup())
```
![](https://velog.velcdn.com/images/yy2hi/post/eca63ba2-87db-48cd-914e-9cdb90c04c96/image.png)

---

### folium.Circle(), folium.CircleMarker()
```py
m = folium.Map(
    location=[37.5445, 127.0558],
    zoom_start=14,
    tile="OpenStreetMap"
)

# Circle
folium.Circle(
    location=[37.5574, 127.04370],
    radius=100,
    fill=True,
    color="#eb9e34",
    fill_color="red",
    popup="Circle Popup",
    tooltip="Circle Tooltip"
).add_to(m)

# CircleMarker
folium.Circle(
    location=[37.5434, 127.04470],
    radius=100,
    fill=True,
    color="#34ebc6",
    fill_color="#c636eb",
    popup="CircleMarker Popup",
    tooltip="CircleMarker Tooltip"
).add_to(m)

m
```
![](https://velog.velcdn.com/images/yy2hi/post/a116a000-8fe0-4b1d-9e07-550c45538cf4/image.png)

---

### folium.Choropleth
```py
state_data = pd.read_csv("../data/02. US_Unemployment_Oct2012.csv")
state_data.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>Unemployment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48</th>
      <td>WI</td>
      <td>6.8</td>
    </tr>
    <tr>
      <th>49</th>
      <td>WY</td>
      <td>5.1</td>
    </tr>
  </tbody>
</table>

```py
m = folium.Map([43, -102], zoom_start=3)
folium.Choropleth(
    geo_data="../data/02. us-states.json", # 경계선 좌표값이 담긴 데이터
    data=state_data, #Series or DataFrame
    columns=["State", "Unemployment"], # DataFrame columns
    key_on="feature.id",
    fill_color="BuPu",
    fill_opacity=1, # 0~1
    line_opacity=1, # 0~1
    legend_name="unemployment rate (%)" 
).add_to(m)

m
```
![](https://velog.velcdn.com/images/yy2hi/post/054d0bdb-8f3c-4075-aba7-a7346d2e430f/image.png)

---

## 아파트 유형 지도 시각화
- 공공데이터포털, https://data.go.kr/data/15066101/fileData.do

```py
df = pd.read_csv("../data/02. 서울특별시 동작구_주택유형별 위치 정보 및 세대수 현황_20210825.csv", encoding="cp949")
df.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>연번</th>
      <th>분류</th>
      <th>건물명</th>
      <th>행정동</th>
      <th>주소</th>
      <th>세대수</th>
      <th>위도</th>
      <th>경도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>165</th>
      <td>166</td>
      <td>연립주택</td>
      <td>능내연립</td>
      <td>사당5동</td>
      <td>서울특별시 동작구 사당로8길 39</td>
      <td>22</td>
      <td>37.483599</td>
      <td>126.968672</td>
    </tr>
    <tr>
      <th>166</th>
      <td>167</td>
      <td>연립주택</td>
      <td>천록</td>
      <td>대방동</td>
      <td>서울특별시 동작구 등용로 43</td>
      <td>29</td>
      <td>37.505475</td>
      <td>126.933434</td>
    </tr>
  </tbody>
</table>

---

#### NaN 데이터 제거

```py
df = df.dropna()
df.info()

=>

<class 'pandas.core.frame.DataFrame'>
Int64Index: 163 entries, 0 to 166
Data columns (total 8 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   연번      163 non-null    int64  
 1   분류      163 non-null    object 
 2   건물명     163 non-null    object 
 3   행정동     163 non-null    object 
 4   주소      163 non-null    object 
 5   세대수     163 non-null    int64  
 6   위도      163 non-null    float64
 7   경도      163 non-null    float64
dtypes: float64(2), int64(2), object(4)
memory usage: 11.5+ KB
-------------------------------------------
df = df.reset_index(drop=True)
df.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>연번</th>
      <th>분류</th>
      <th>건물명</th>
      <th>행정동</th>
      <th>주소</th>
      <th>세대수</th>
      <th>위도</th>
      <th>경도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>161</th>
      <td>166</td>
      <td>연립주택</td>
      <td>능내연립</td>
      <td>사당5동</td>
      <td>서울특별시 동작구 사당로8길 39</td>
      <td>22</td>
      <td>37.483599</td>
      <td>126.968672</td>
    </tr>
    <tr>
      <th>162</th>
      <td>167</td>
      <td>연립주택</td>
      <td>천록</td>
      <td>대방동</td>
      <td>서울특별시 동작구 등용로 43</td>
      <td>29</td>
      <td>37.505475</td>
      <td>126.933434</td>
    </tr>
  </tbody>
</table>

```py
df.columns

=>

Index(['연번 ', '분류 ', '건물명', '행정동', '주소', '세대수', '위도', '경도'], dtype='object')
--------------------------------------------------------------------------
df["연번 "]

=>

0        1
1        2
2        3
3        4
4        5
      ... 
158    163
159    164
160    165
161    166
162    167
Name: 연번 , Length: 163, dtype: int64
-----------------------------------------------------------
df = df.rename(columns={"연번 ": "연번", "분류 ": "분류"})
df.연번[:10]

=>

0     1
1     2
2     3
3     4
4     5
5     6
6     7
7     8
8     9
9    10
Name: 연번, dtype: int64
------------------------
del df["연번"]
df.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>분류</th>
      <th>건물명</th>
      <th>행정동</th>
      <th>주소</th>
      <th>세대수</th>
      <th>위도</th>
      <th>경도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>161</th>
      <td>연립주택</td>
      <td>능내연립</td>
      <td>사당5동</td>
      <td>서울특별시 동작구 사당로8길 39</td>
      <td>22</td>
      <td>37.483599</td>
      <td>126.968672</td>
    </tr>
    <tr>
      <th>162</th>
      <td>연립주택</td>
      <td>천록</td>
      <td>대방동</td>
      <td>서울특별시 동작구 등용로 43</td>
      <td>29</td>
      <td>37.505475</td>
      <td>126.933434</td>
    </tr>
  </tbody>
</table>

```py
df.describe()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>세대수</th>
      <th>위도</th>
      <th>경도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>163.000000</td>
      <td>163.000000</td>
      <td>163.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>371.920245</td>
      <td>37.497442</td>
      <td>126.949817</td>
    </tr>
    <tr>
      <th>std</th>
      <td>413.115354</td>
      <td>0.009532</td>
      <td>0.019861</td>
    </tr>
    <tr>
      <th>min</th>
      <td>21.000000</td>
      <td>37.477376</td>
      <td>126.906940</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>86.000000</td>
      <td>37.490626</td>
      <td>126.933284</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>199.000000</td>
      <td>37.496940</td>
      <td>126.949902</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>518.500000</td>
      <td>37.505321</td>
      <td>126.967196</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2621.000000</td>
      <td>37.514280</td>
      <td>126.981966</td>
    </tr>
  </tbody>
</table>

---

#### folium

```py
# folium

m = folium.Map(
    location=[37.497112,126.94437795],
    zoom_start=13)
for idx, rows in df.iterrows():
    # location
    lat, lng = rows.위도, rows.경도
    
    # Marker
    folium.Marker(
        location=[lat, lng],
        popup=rows.주소,
        tooltip=rows.분류,
        icon=folium.Icon(
            icon="home",
            color="lightred" if rows.세대수 >= 199 else "lightblue",
            icon_color="darked" if rows.세대수 >= 199 else "darkblue",
        )
    ).add_to(m)

    # CircleMarker
    folium.Circle(
        location=[lat, lng],
        radius=rows.세대수 * 0.5,
        fill=True,
        color="pink" if rows.세대수 >= 518 else "green",
        fill_color="pink" if rows.세대수 >= 518 else "green",
    ).add_to(m)
    
m
```
![](https://velog.velcdn.com/images/yy2hi/post/e80e012d-82b7-4b06-b219-eaa899988488/image.png)

