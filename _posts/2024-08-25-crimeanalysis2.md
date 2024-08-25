---
layout: single
title: "Project 2 - 서울시 범죄 현황 데이터 분석 (2)"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 📖 Pandas에 잘 맞춰진 반복문용 명령어 : iterrows()

- Pandas 데이터 프레임 대부분은 2차원이므로 for문을 사용하면 가독률 <span style="color: red">**↓**</span>
∴ Pandas 데이터 프레임으로 반복문을 만들때는 iterrows()를 권장 (인덱스와 내용으로 구분)
---
## Google Maps를 이용한 데이터 정리

#### 구별, lat, lng 컬럼 추가
```py
crime_station["구별"] = np.nan
crime_station["lat"] = np.nan
crime_station["lng"] = np.nan

crime_station.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>죄종</th>
      <th colspan="2" halign="left">강간</th>
      <th colspan="2" halign="left">강도</th>
      <th colspan="2" halign="left">살인</th>
      <th colspan="2" halign="left">절도</th>
      <th colspan="2" halign="left">폭력</th>
      <th>구별</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
    <tr>
      <th>발생검거</th>
      <th>검거</th>
      <th>발생</th>
      <th>검거</th>
      <th>발생</th>
      <th>검거</th>
      <th>발생</th>
      <th>검거</th>
      <th>발생</th>
      <th>검거</th>
      <th>발생</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
    <tr>
      <th>구분</th>
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
      <th>강남</th>
      <td>269.0</td>
      <td>339.0</td>
      <td>26.0</td>
      <td>24.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>1129.0</td>
      <td>2438.0</td>
      <td>2096.0</td>
      <td>2336.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>강동</th>
      <td>152.0</td>
      <td>160.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>902.0</td>
      <td>1754.0</td>
      <td>2201.0</td>
      <td>2530.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>강북</th>
      <td>159.0</td>
      <td>217.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>672.0</td>
      <td>1222.0</td>
      <td>2482.0</td>
      <td>2778.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>강서</th>
      <td>239.0</td>
      <td>275.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>9.0</td>
      <td>1070.0</td>
      <td>1952.0</td>
      <td>2768.0</td>
      <td>3204.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>관악</th>
      <td>264.0</td>
      <td>322.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>937.0</td>
      <td>2103.0</td>
      <td>2707.0</td>
      <td>3235.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

---

#### iterrows()
- 경찰서 이름으로 소속된 구 이름 얻기
- 구 이름, 위도, 경도 정보 저장
- 반복문으로 NaN 채우기

```py
count = 0

for idx, rows in crime_station.iterrows():
    station_name = "서울" + str(idx) + "경찰서"
    tmp = gmaps.geocode(station_name, language="ko")
    
    tmp_gu = tmp[0].get("formatted_address")
    
    lat = tmp[0].get("geometry")["location"]["lat"]
    lng = tmp[0].get("geometry")["location"]["lng"]
    
    if count == 4:
        crime_station.loc[idx, "구별"] = "관악구"
    else:
        crime_station.loc[idx, "lat"] = lat
        crime_station.loc[idx, "lng"] = lng
        crime_station.loc[idx, "구별"] = tmp_gu.split()[2]
        
    print(count)
    count += 1
    
crime_station.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
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
    <tr>
      <th>구분</th>
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
      <th>강남</th>
      <td>269.0</td>
      <td>339.0</td>
      <td>26.0</td>
      <td>24.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>1129.0</td>
      <td>2438.0</td>
      <td>2096.0</td>
      <td>2336.0</td>
      <td>강남구</td>
      <td>37.509435</td>
      <td>127.066958</td>
    </tr>
    <tr>
      <th>강동</th>
      <td>152.0</td>
      <td>160.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>902.0</td>
      <td>1754.0</td>
      <td>2201.0</td>
      <td>2530.0</td>
      <td>강동구</td>
      <td>37.528511</td>
      <td>127.126822</td>
    </tr>
    <tr>
      <th>강북</th>
      <td>159.0</td>
      <td>217.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>672.0</td>
      <td>1222.0</td>
      <td>2482.0</td>
      <td>2778.0</td>
      <td>강북구</td>
      <td>37.637304</td>
      <td>127.027340</td>
    </tr>
    <tr>
      <th>강서</th>
      <td>239.0</td>
      <td>275.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>9.0</td>
      <td>1070.0</td>
      <td>1952.0</td>
      <td>2768.0</td>
      <td>3204.0</td>
      <td>양천구</td>
      <td>37.539783</td>
      <td>126.829997</td>
    </tr>
    <tr>
      <th>관악</th>
      <td>264.0</td>
      <td>322.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>937.0</td>
      <td>2103.0</td>
      <td>2707.0</td>
      <td>3235.0</td>
      <td>관악구</td>
      <td>37.474395</td>
      <td>126.951349</td>
    </tr>
  </tbody>
</table>

---

#### 데이터 정리
```py
crime_station.columns.get_level_values(0)[2] + crime_station.columns.get_level_values(1)[2]
len(crime_station.columns.get_level_values(0))

'강도검거'
13

tmp = [
    crime_station.columns.get_level_values(0)[n] + crime_station.columns.get_level_values(1)[n]
    for n in range(0, len(crime_station.columns.get_level_values(0)))
]
tmp, len(tmp)

(['강간검거',
  '강간발생',
  '강도검거',
  '강도발생',
  '살인검거',
  '살인발생',
  '절도검거',
  '절도발생',
  '폭력검거',
  '폭력발생',
  '구별',
  'lat',
  'lng'],
 13)
 
 crime_station.columns = tmp
 crime_station.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
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
    <tr>
      <th>구분</th>
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
      <th>강남</th>
      <td>269.0</td>
      <td>339.0</td>
      <td>26.0</td>
      <td>24.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>1129.0</td>
      <td>2438.0</td>
      <td>2096.0</td>
      <td>2336.0</td>
      <td>강남구</td>
      <td>37.509435</td>
      <td>127.066958</td>
    </tr>
    <tr>
      <th>강동</th>
      <td>152.0</td>
      <td>160.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>902.0</td>
      <td>1754.0</td>
      <td>2201.0</td>
      <td>2530.0</td>
      <td>강동구</td>
      <td>37.528511</td>
      <td>127.126822</td>
    </tr>
    <tr>
      <th>강북</th>
      <td>159.0</td>
      <td>217.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>672.0</td>
      <td>1222.0</td>
      <td>2482.0</td>
      <td>2778.0</td>
      <td>강북구</td>
      <td>37.637304</td>
      <td>127.027340</td>
    </tr>
    <tr>
      <th>강서</th>
      <td>239.0</td>
      <td>275.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>9.0</td>
      <td>1070.0</td>
      <td>1952.0</td>
      <td>2768.0</td>
      <td>3204.0</td>
      <td>강서구</td>
      <td>37.539783</td>
      <td>126.829997</td>
    </tr>
    <tr>
      <th>관악</th>
      <td>264.0</td>
      <td>322.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>937.0</td>
      <td>2103.0</td>
      <td>2707.0</td>
      <td>3235.0</td>
      <td>관악구</td>
      <td>37.474395</td>
      <td>126.951349</td>
    </tr>
  </tbody>
</table>
---

#### 데이터 저장
```py
crime_station.to_csv("../data/02. crime_in_Seoul_raw.csv", sep=",", encoding="utf-8")
```

---

## 구별 데이터로 정리
```py
crime_anal_station = pd.read_csv(
    "../data/02. crime_in_Seoul_raw.csv", index_col=0, encoding="utf-8") # index_col "구분"을 인덱스 컬럼으로 설정
crime_anal_station.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
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
    <tr>
      <th>구분</th>
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
      <th>강남</th>
      <td>269.0</td>
      <td>339.0</td>
      <td>26.0</td>
      <td>24.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>1129.0</td>
      <td>2438.0</td>
      <td>2096.0</td>
      <td>2336.0</td>
      <td>강남구</td>
      <td>37.509435</td>
      <td>127.066958</td>
    </tr>
    <tr>
      <th>강동</th>
      <td>152.0</td>
      <td>160.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>902.0</td>
      <td>1754.0</td>
      <td>2201.0</td>
      <td>2530.0</td>
      <td>강동구</td>
      <td>37.528511</td>
      <td>127.126822</td>
    </tr>
    <tr>
      <th>강북</th>
      <td>159.0</td>
      <td>217.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>672.0</td>
      <td>1222.0</td>
      <td>2482.0</td>
      <td>2778.0</td>
      <td>강북구</td>
      <td>37.637304</td>
      <td>127.027340</td>
    </tr>
    <tr>
      <th>강서</th>
      <td>239.0</td>
      <td>275.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>9.0</td>
      <td>1070.0</td>
      <td>1952.0</td>
      <td>2768.0</td>
      <td>3204.0</td>
      <td>양천구</td>
      <td>37.539783</td>
      <td>126.829997</td>
    </tr>
    <tr>
      <th>관악</th>
      <td>264.0</td>
      <td>322.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>937.0</td>
      <td>2103.0</td>
      <td>2707.0</td>
      <td>3235.0</td>
      <td>관악구</td>
      <td>37.474395</td>
      <td>126.951349</td>
    </tr>
  </tbody>
</table>

---

```py
crime_anal_gu = pd.pivot_table(crime_anal_station, index="구별", aggfunc=np.sum)

del crime_anal_gu["lat"]
crime_anal_gu.drop("lng", axis=1, inplace=True)

crime_anal_gu.head()
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>413.0</td>
      <td>516.0</td>
      <td>42.0</td>
      <td>39.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>1918.0</td>
      <td>3587.0</td>
      <td>3527.0</td>
      <td>4002.0</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>152.0</td>
      <td>160.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>902.0</td>
      <td>1754.0</td>
      <td>2201.0</td>
      <td>2530.0</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>159.0</td>
      <td>217.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>672.0</td>
      <td>1222.0</td>
      <td>2482.0</td>
      <td>2778.0</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>264.0</td>
      <td>322.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>937.0</td>
      <td>2103.0</td>
      <td>2707.0</td>
      <td>3235.0</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>234.0</td>
      <td>279.0</td>
      <td>6.0</td>
      <td>11.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1057.0</td>
      <td>2636.0</td>
      <td>2011.0</td>
      <td>2392.0</td>
    </tr>
  </tbody>
</table>
</div>

#### 검거율 생성
- 하나의 컬럼을 다른 컬럼으로 나누기
```py
crime_anal_gu["강도검거"] / crime_anal_gu["강도발생"]

구별
강남구     1.076923
강동구     0.928571
강북구     0.800000
관악구     0.833333
광진구     0.545455
구로구     1.300000
금천구     1.000000
노원구     1.500000
도봉구     1.000000
동대문구    1.200000
동작구     1.000000
마포구     1.750000
서대문구    0.800000
서초구     0.769231
성동구     1.666667
성북구     1.000000
송파구     0.800000
양천구     1.000000
영등포구    0.736842
용산구     1.111111
은평구     0.777778
종로구     0.750000
중구      0.875000
중랑구     1.000000
dtype: float64
```
---
- 다수의 컬럼을 다른 컬럼으로 나누기
```py
crime_anal_gu[["강도검거", "살인검거"]].div(crime_anal_gu["강도발생"], axis=0).head(3)
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
      <th>강도검거</th>
      <th>살인검거</th>
    </tr>
    <tr>
      <th>구별</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>1.076923</td>
      <td>0.128205</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>0.928571</td>
      <td>0.357143</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>0.800000</td>
      <td>1.200000</td>
    </tr>
  </tbody>
</table>
</div>

---

- 다수의 컬럼을 다수의 컬럼으로 각각 나누기
```py
num =["강간검거", "강도검거", "살인검거", "절도검거", "폭력검거"]
den =["강간발생", "강도발생", "살인발생", "절도발생", "폭력발생"]

crime_anal_gu[num].div(crime_anal_gu[den].values).head()
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
      <th>강간검거</th>
      <th>강도검거</th>
      <th>살인검거</th>
      <th>절도검거</th>
      <th>폭력검거</th>
    </tr>
    <tr>
      <th>구별</th>
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
      <td>0.800388</td>
      <td>1.076923</td>
      <td>1.000000</td>
      <td>0.534709</td>
      <td>0.881309</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>0.950000</td>
      <td>0.928571</td>
      <td>1.250000</td>
      <td>0.514253</td>
      <td>0.869960</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>0.732719</td>
      <td>0.800000</td>
      <td>0.857143</td>
      <td>0.549918</td>
      <td>0.893449</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>0.819876</td>
      <td>0.833333</td>
      <td>1.166667</td>
      <td>0.445554</td>
      <td>0.836785</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>0.838710</td>
      <td>0.545455</td>
      <td>1.000000</td>
      <td>0.400986</td>
      <td>0.840719</td>
    </tr>
  </tbody>
</table>
</div>

```py
target = ["강간검거율", "강도검거율", "살인검거율", "절도검거율", "폭력검거율"]

num =["강간검거", "강도검거", "살인검거", "절도검거", "폭력검거"]
den =["강간발생", "강도발생", "살인발생", "절도발생", "폭력발생"]

crime_anal_gu[target] = crime_anal_gu[num].div(crime_anal_gu[den].values) * 100
crime_anal_gu.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
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
      <th>강간검거율</th>
      <th>강도검거율</th>
      <th>살인검거율</th>
      <th>절도검거율</th>
      <th>폭력검거율</th>
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
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>413.0</td>
      <td>516.0</td>
      <td>42.0</td>
      <td>39.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>1918.0</td>
      <td>3587.0</td>
      <td>3527.0</td>
      <td>4002.0</td>
      <td>80.038760</td>
      <td>107.692308</td>
      <td>100.000000</td>
      <td>53.470867</td>
      <td>88.130935</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>152.0</td>
      <td>160.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>902.0</td>
      <td>1754.0</td>
      <td>2201.0</td>
      <td>2530.0</td>
      <td>95.000000</td>
      <td>92.857143</td>
      <td>125.000000</td>
      <td>51.425314</td>
      <td>86.996047</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>159.0</td>
      <td>217.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>672.0</td>
      <td>1222.0</td>
      <td>2482.0</td>
      <td>2778.0</td>
      <td>73.271889</td>
      <td>80.000000</td>
      <td>85.714286</td>
      <td>54.991817</td>
      <td>89.344852</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>264.0</td>
      <td>322.0</td>
      <td>10.0</td>
      <td>12.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>937.0</td>
      <td>2103.0</td>
      <td>2707.0</td>
      <td>3235.0</td>
      <td>81.987578</td>
      <td>83.333333</td>
      <td>116.666667</td>
      <td>44.555397</td>
      <td>83.678516</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>234.0</td>
      <td>279.0</td>
      <td>6.0</td>
      <td>11.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>1057.0</td>
      <td>2636.0</td>
      <td>2011.0</td>
      <td>2392.0</td>
      <td>83.870968</td>
      <td>54.545455</td>
      <td>100.000000</td>
      <td>40.098634</td>
      <td>84.071906</td>
    </tr>
  </tbody>
</table>

---

#### 필요없는 칼럼 제거
```py
del crime_anal_gu["강간검거"]
del crime_anal_gu["강도검거"]
crime_anal_gu.drop(["살인검거", "절도검거", "폭력검거"], axis=1, inplace=True)

crime_anal_gu.head()
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
      <th>강간발생</th>
      <th>강도발생</th>
      <th>살인발생</th>
      <th>절도발생</th>
      <th>폭력발생</th>
      <th>강간검거율</th>
      <th>강도검거율</th>
      <th>살인검거율</th>
      <th>절도검거율</th>
      <th>폭력검거율</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>516.0</td>
      <td>39.0</td>
      <td>5.0</td>
      <td>3587.0</td>
      <td>4002.0</td>
      <td>80.038760</td>
      <td>107.692308</td>
      <td>100.000000</td>
      <td>53.470867</td>
      <td>88.130935</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>160.0</td>
      <td>14.0</td>
      <td>4.0</td>
      <td>1754.0</td>
      <td>2530.0</td>
      <td>95.000000</td>
      <td>92.857143</td>
      <td>125.000000</td>
      <td>51.425314</td>
      <td>86.996047</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>217.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>1222.0</td>
      <td>2778.0</td>
      <td>73.271889</td>
      <td>80.000000</td>
      <td>85.714286</td>
      <td>54.991817</td>
      <td>89.344852</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>322.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>2103.0</td>
      <td>3235.0</td>
      <td>81.987578</td>
      <td>83.333333</td>
      <td>116.666667</td>
      <td>44.555397</td>
      <td>83.678516</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>279.0</td>
      <td>11.0</td>
      <td>4.0</td>
      <td>2636.0</td>
      <td>2392.0</td>
      <td>83.870968</td>
      <td>54.545455</td>
      <td>100.000000</td>
      <td>40.098634</td>
      <td>84.071906</td>
    </tr>
  </tbody>
</table>
</div>

---

#### 검거율 100보다 큰 숫자 변경
```py
crime_anal_gu[crime_anal_gu[target] > 100] = 100
crime_anal_gu.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>강간발생</th>
      <th>강도발생</th>
      <th>살인발생</th>
      <th>절도발생</th>
      <th>폭력발생</th>
      <th>강간검거율</th>
      <th>강도검거율</th>
      <th>살인검거율</th>
      <th>절도검거율</th>
      <th>폭력검거율</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>516.0</td>
      <td>39.0</td>
      <td>5.0</td>
      <td>3587.0</td>
      <td>4002.0</td>
      <td>80.038760</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>53.470867</td>
      <td>88.130935</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>160.0</td>
      <td>14.0</td>
      <td>4.0</td>
      <td>1754.0</td>
      <td>2530.0</td>
      <td>95.000000</td>
      <td>92.857143</td>
      <td>100.000000</td>
      <td>51.425314</td>
      <td>86.996047</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>217.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>1222.0</td>
      <td>2778.0</td>
      <td>73.271889</td>
      <td>80.000000</td>
      <td>85.714286</td>
      <td>54.991817</td>
      <td>89.344852</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>322.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>2103.0</td>
      <td>3235.0</td>
      <td>81.987578</td>
      <td>83.333333</td>
      <td>100.000000</td>
      <td>44.555397</td>
      <td>83.678516</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>279.0</td>
      <td>11.0</td>
      <td>4.0</td>
      <td>2636.0</td>
      <td>2392.0</td>
      <td>83.870968</td>
      <td>54.545455</td>
      <td>100.000000</td>
      <td>40.098634</td>
      <td>84.071906</td>
    </tr>
  </tbody>
</table>

---

#### 컬럼 이름 변경
```py
crime_anal_gu.rename(
    columns={"강간발생": "강간", "강도발생": "강도", "살인발생": "살인", "절도발생": "절도", "폭력발생": "폭력"},
    inplace=True)
crime_anal_gu.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>강간</th>
      <th>강도</th>
      <th>살인</th>
      <th>절도</th>
      <th>폭력</th>
      <th>강간검거율</th>
      <th>강도검거율</th>
      <th>살인검거율</th>
      <th>절도검거율</th>
      <th>폭력검거율</th>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>516.0</td>
      <td>39.0</td>
      <td>5.0</td>
      <td>3587.0</td>
      <td>4002.0</td>
      <td>80.038760</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>53.470867</td>
      <td>88.130935</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>160.0</td>
      <td>14.0</td>
      <td>4.0</td>
      <td>1754.0</td>
      <td>2530.0</td>
      <td>95.000000</td>
      <td>92.857143</td>
      <td>100.000000</td>
      <td>51.425314</td>
      <td>86.996047</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>217.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>1222.0</td>
      <td>2778.0</td>
      <td>73.271889</td>
      <td>80.000000</td>
      <td>85.714286</td>
      <td>54.991817</td>
      <td>89.344852</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>322.0</td>
      <td>12.0</td>
      <td>6.0</td>
      <td>2103.0</td>
      <td>3235.0</td>
      <td>81.987578</td>
      <td>83.333333</td>
      <td>100.000000</td>
      <td>44.555397</td>
      <td>83.678516</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>279.0</td>
      <td>11.0</td>
      <td>4.0</td>
      <td>2636.0</td>
      <td>2392.0</td>
      <td>83.870968</td>
      <td>54.545455</td>
      <td>100.000000</td>
      <td>40.098634</td>
      <td>84.071906</td>
    </tr>
  </tbody>
</table>