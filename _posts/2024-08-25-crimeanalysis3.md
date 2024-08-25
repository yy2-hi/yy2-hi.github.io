---
layout: single
title: "Project 2 - 서울시 범죄 현황 데이터 분석 (3)"
categories: Data Analysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 범죄 데이터 정렬을 위한 데이터 정리
```py
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

---

#### 정규화
```py
#최대값 1, 최소값 0
crime_anal_gu["강도"] / crime_anal_gu["강도"].max()

구별
강남구     1.000000
강동구     0.358974
강북구     0.128205
관악구     0.307692
광진구     0.282051
구로구     0.256410
금천구     0.179487
노원구     0.153846
도봉구     0.128205
동대문구    0.256410
동작구     0.179487
마포구     0.102564
서대문구    0.128205
서초구     0.333333
성동구     0.076923
성북구     0.205128
송파구     0.384615
양천구     0.435897
영등포구    0.487179
용산구     0.230769
은평구     0.230769
종로구     0.307692
중구      0.205128
중랑구     0.358974
Name: 강도, dtype: float64

col = ["살인", "강도", "강간", "절도", "폭력"]
crime_anal_norm = crime_anal_gu[col] / crime_anal_gu[col].max()
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
      <td>0.357143</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.977118</td>
      <td>0.733773</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>0.285714</td>
      <td>0.358974</td>
      <td>0.310078</td>
      <td>0.477799</td>
      <td>0.463880</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>0.500000</td>
      <td>0.128205</td>
      <td>0.420543</td>
      <td>0.332879</td>
      <td>0.509351</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>0.428571</td>
      <td>0.307692</td>
      <td>0.624031</td>
      <td>0.572868</td>
      <td>0.593143</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>0.285714</td>
      <td>0.282051</td>
      <td>0.540698</td>
      <td>0.718060</td>
      <td>0.438577</td>
    </tr>
  </tbody>
</table>

---

#### 검거율 추가
```py
col2 = ["강간검거율", "강도검거율", "살인검거율", "절도검거율", "폭력검거율"]
crime_anal_norm[col2] = crime_anal_gu[col2]
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
    </tr>
  </tbody>
</table>

---

#### 구별 CCTV자료에서 인구수, CCTV 수 추가
```py
result_CCTV = pd.read_csv("../data/01. CCTV_result.csv", index_col="구별", encoding="utf-8")
result_CCTV.head()
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
      <th>소계</th>
      <th>최근증가율</th>
      <th>인구수</th>
      <th>한국인</th>
      <th>외국인</th>
      <th>고령자</th>
      <th>외국인비율</th>
      <th>고령자비율</th>
      <th>CCTV비율</th>
      <th>오차</th>
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
      <td>3238</td>
      <td>150.619195</td>
      <td>561052</td>
      <td>556164</td>
      <td>4888</td>
      <td>65060</td>
      <td>0.871220</td>
      <td>11.596073</td>
      <td>0.577130</td>
      <td>1549.200326</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>1010</td>
      <td>166.490765</td>
      <td>440359</td>
      <td>436223</td>
      <td>4136</td>
      <td>56161</td>
      <td>0.939234</td>
      <td>12.753458</td>
      <td>0.229358</td>
      <td>-544.642322</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>831</td>
      <td>125.203252</td>
      <td>328002</td>
      <td>324479</td>
      <td>3523</td>
      <td>56530</td>
      <td>1.074079</td>
      <td>17.234651</td>
      <td>0.253352</td>
      <td>-598.750923</td>
    </tr>
    <tr>
      <th>강서구</th>
      <td>911</td>
      <td>134.793814</td>
      <td>608255</td>
      <td>601691</td>
      <td>6564</td>
      <td>76032</td>
      <td>1.079153</td>
      <td>12.500021</td>
      <td>0.149773</td>
      <td>-830.268578</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>2109</td>
      <td>149.290780</td>
      <td>520929</td>
      <td>503297</td>
      <td>17632</td>
      <td>70046</td>
      <td>3.384722</td>
      <td>13.446362</td>
      <td>0.404854</td>
      <td>464.799395</td>
    </tr>
  </tbody>
</table>
</div>

```py
crime_anal_norm[["인구수", "CCTV"]] = result_CCTV[["인구수", "소계"]]
crime_anal_norm.head()
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
    </tr>
  </tbody>
</table>
</div>

---

#### 정규화된 범죄발생 건수 전체 평균을 구해서 범죄 컬럼 대표값으로 사용
```py
col = ["강간", "강도", "살인", "절도", "폭력"]
crime_anal_norm["범죄"] = np.mean(crime_anal_norm[col], axis=1) #행렬의 평균
crime_anal_norm.head()
```
<div class="output_area"><div class="run_this_cell"></div><div class="prompt output_prompt"><bdi>Out[112]:</bdi></div><div class="output_subarea output_html rendered_html output_result" dir="auto"><div>
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
    </tr>
  </tbody>
</table>
</div></div></div>

---

## np.mean()
```py
np.array(
    [[0.357143, 1.000000, 1.000000, 0.977118, 0.733773],
    [0.285714, 0.358974, 0.310078, 0.477799, 0.463880]]
)

=>

array([[0.357143, 1.      , 1.      , 0.977118, 0.733773],
       [0.285714, 0.358974, 0.310078, 0.477799, 0.46388 ]])

------------------------------------------------------------
       
np.mean(np.array(
    [[0.357143, 1.000000, 1.000000, 0.977118, 0.733773],
    [0.285714, 0.358974, 0.310078, 0.477799, 0.463880]]
), axis=1) # axis=1 : 행 기준, axis=0 : 열 기준

=>

array([0.8136068, 0.379289 ])
```
---

#### 검거율의 평균을 검거 컬럼의 대표값으로 사용
```py
col = ["강간검거율", "강도검거율", "살인검거율", "절도검거율", "폭력검거율"]
crime_anal_norm["검거"] = np.mean(crime_anal_norm[col], axis=1) # axis=1 : 행 기준
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

## 최종 데이터 프레임
```py
crime_anal_norm
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
    <tr>
      <th>구로구</th>
      <td>0.642857</td>
      <td>0.256410</td>
      <td>0.529070</td>
      <td>0.520294</td>
      <td>0.580125</td>
      <td>66.300366</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>45.078534</td>
      <td>84.702908</td>
      <td>441559</td>
      <td>1884</td>
      <td>0.505751</td>
      <td>79.216362</td>
    </tr>
    <tr>
      <th>금천구</th>
      <td>0.428571</td>
      <td>0.179487</td>
      <td>0.339147</td>
      <td>0.344320</td>
      <td>0.402090</td>
      <td>81.714286</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>51.740506</td>
      <td>88.736890</td>
      <td>253491</td>
      <td>1348</td>
      <td>0.338723</td>
      <td>84.438336</td>
    </tr>
    <tr>
      <th>노원구</th>
      <td>0.357143</td>
      <td>0.153846</td>
      <td>0.308140</td>
      <td>0.505857</td>
      <td>0.461313</td>
      <td>89.308176</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>39.849219</td>
      <td>84.419714</td>
      <td>558075</td>
      <td>1566</td>
      <td>0.357260</td>
      <td>82.715422</td>
    </tr>
    <tr>
      <th>도봉구</th>
      <td>0.214286</td>
      <td>0.128205</td>
      <td>0.238372</td>
      <td>0.235903</td>
      <td>0.264210</td>
      <td>98.373984</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>56.812933</td>
      <td>90.839695</td>
      <td>346234</td>
      <td>825</td>
      <td>0.216195</td>
      <td>89.205322</td>
    </tr>
    <tr>
      <th>동대문구</th>
      <td>0.357143</td>
      <td>0.256410</td>
      <td>0.368217</td>
      <td>0.528466</td>
      <td>0.484415</td>
      <td>83.157895</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>55.206186</td>
      <td>89.969720</td>
      <td>366011</td>
      <td>1870</td>
      <td>0.398930</td>
      <td>85.666760</td>
    </tr>
    <tr>
      <th>동작구</th>
      <td>0.571429</td>
      <td>0.179487</td>
      <td>0.629845</td>
      <td>0.333969</td>
      <td>0.304547</td>
      <td>45.846154</td>
      <td>100.000000</td>
      <td>75.000000</td>
      <td>45.187602</td>
      <td>86.935581</td>
      <td>408493</td>
      <td>1302</td>
      <td>0.403855</td>
      <td>70.593867</td>
    </tr>
    <tr>
      <th>마포구</th>
      <td>0.285714</td>
      <td>0.102564</td>
      <td>0.773256</td>
      <td>0.688368</td>
      <td>0.538871</td>
      <td>80.200501</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>37.198259</td>
      <td>85.062947</td>
      <td>385783</td>
      <td>980</td>
      <td>0.477755</td>
      <td>80.492341</td>
    </tr>
    <tr>
      <th>서대문구</th>
      <td>0.428571</td>
      <td>0.128205</td>
      <td>0.339147</td>
      <td>0.409425</td>
      <td>0.362303</td>
      <td>84.000000</td>
      <td>80.000000</td>
      <td>100.000000</td>
      <td>50.033267</td>
      <td>83.198381</td>
      <td>325028</td>
      <td>1254</td>
      <td>0.333530</td>
      <td>79.446329</td>
    </tr>
    <tr>
      <th>서초구</th>
      <td>0.357143</td>
      <td>0.333333</td>
      <td>0.829457</td>
      <td>0.600654</td>
      <td>0.428676</td>
      <td>63.317757</td>
      <td>76.923077</td>
      <td>100.000000</td>
      <td>50.204082</td>
      <td>86.783576</td>
      <td>445401</td>
      <td>2297</td>
      <td>0.509853</td>
      <td>75.445698</td>
    </tr>
    <tr>
      <th>성동구</th>
      <td>0.285714</td>
      <td>0.076923</td>
      <td>0.201550</td>
      <td>0.353037</td>
      <td>0.296846</td>
      <td>75.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>69.135802</td>
      <td>86.967264</td>
      <td>312711</td>
      <td>1327</td>
      <td>0.242814</td>
      <td>86.220613</td>
    </tr>
    <tr>
      <th>성북구</th>
      <td>0.285714</td>
      <td>0.205128</td>
      <td>0.298450</td>
      <td>0.400436</td>
      <td>0.386505</td>
      <td>75.974026</td>
      <td>100.000000</td>
      <td>75.000000</td>
      <td>49.319728</td>
      <td>86.290323</td>
      <td>455407</td>
      <td>1651</td>
      <td>0.315247</td>
      <td>77.316815</td>
    </tr>
    <tr>
      <th>송파구</th>
      <td>0.642857</td>
      <td>0.384615</td>
      <td>0.453488</td>
      <td>0.692727</td>
      <td>0.603044</td>
      <td>78.632479</td>
      <td>80.000000</td>
      <td>88.888889</td>
      <td>41.211168</td>
      <td>85.375494</td>
      <td>671173</td>
      <td>1081</td>
      <td>0.555346</td>
      <td>74.821606</td>
    </tr>
    <tr>
      <th>양천구</th>
      <td>1.000000</td>
      <td>0.435897</td>
      <td>0.786822</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>85.467980</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>49.713974</td>
      <td>85.918592</td>
      <td>475018</td>
      <td>2482</td>
      <td>0.844544</td>
      <td>84.220109</td>
    </tr>
    <tr>
      <th>영등포구</th>
      <td>0.928571</td>
      <td>0.487179</td>
      <td>0.689922</td>
      <td>0.637701</td>
      <td>0.658783</td>
      <td>63.202247</td>
      <td>73.684211</td>
      <td>100.000000</td>
      <td>40.153780</td>
      <td>83.690509</td>
      <td>402024</td>
      <td>1277</td>
      <td>0.680431</td>
      <td>72.146149</td>
    </tr>
    <tr>
      <th>용산구</th>
      <td>0.285714</td>
      <td>0.230769</td>
      <td>0.486434</td>
      <td>0.405612</td>
      <td>0.437110</td>
      <td>85.258964</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>40.228341</td>
      <td>84.228188</td>
      <td>244444</td>
      <td>2096</td>
      <td>0.369128</td>
      <td>81.943099</td>
    </tr>
    <tr>
      <th>은평구</th>
      <td>0.428571</td>
      <td>0.230769</td>
      <td>0.302326</td>
      <td>0.453827</td>
      <td>0.488449</td>
      <td>91.025641</td>
      <td>77.777778</td>
      <td>100.000000</td>
      <td>53.421369</td>
      <td>86.636637</td>
      <td>491202</td>
      <td>2108</td>
      <td>0.380788</td>
      <td>81.772285</td>
    </tr>
    <tr>
      <th>종로구</th>
      <td>0.428571</td>
      <td>0.307692</td>
      <td>0.461240</td>
      <td>0.528466</td>
      <td>0.414925</td>
      <td>74.369748</td>
      <td>75.000000</td>
      <td>33.333333</td>
      <td>39.587629</td>
      <td>87.361909</td>
      <td>164257</td>
      <td>1619</td>
      <td>0.428179</td>
      <td>61.930524</td>
    </tr>
    <tr>
      <th>중구</th>
      <td>0.214286</td>
      <td>0.205128</td>
      <td>0.383721</td>
      <td>0.585671</td>
      <td>0.407957</td>
      <td>74.747475</td>
      <td>87.500000</td>
      <td>100.000000</td>
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
      <td>100.000000</td>
      <td>87.500000</td>
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

## Seaborn
```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib import rc

plt.rcParams["axes.unicode_minus"] = False
rc("font", family="malgun gothic")
%matplotlib inline # get_ipython().run_line_magic("matplotlib", "inline")
```
---
```py
np.linspace(0, 14, 100) # 0부터 14까지 100개

=>

array([ 0.        ,  0.14141414,  0.28282828,  0.42424242,  0.56565657,
									.
                                    .
                                    .
       13.43434343, 13.57575758, 13.71717172, 13.85858586, 14.        ])
```
---
```py
x = np.linspace(0, 14, 100)
y1 = np.sin(x)
y2 = 2 * np.sin(x + 0.5)
y3 = 3 * np.sin(x + 1.0)
y4 = 4 * np.sin(x + 1.5)

plt.figure(figsize=(10, 6))
plt.plot(x, y1, x, y2, x, y3, x, y4)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/3689a77b-fb25-4893-90d1-ad988a1fe4cb/image.png)

---

#### style : "white", "whitegrid", "dark", "darkgrid"
```py
sns.set_style("dark")
plt.figure(figsize=(10, 6))
plt.plot(x, y1, x, y2, x, y3, x, y4)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/e0c315ac-4d3d-432a-a6dd-05681e0b7241/image.png)

---

### seaborn tips data
- boxplot
- swarmplot
- lmplot

```py
tips = sns.load_dataset("tips")
tips
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
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
    </tr>
    <tr>
      <th>239</th>
      <td>29.03</td>
      <td>5.92</td>
      <td>Male</td>
      <td>No</td>
      <td>Sat</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>240</th>
      <td>27.18</td>
      <td>2.00</td>
      <td>Female</td>
      <td>Yes</td>
      <td>Sat</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>241</th>
      <td>22.67</td>
      <td>2.00</td>
      <td>Male</td>
      <td>Yes</td>
      <td>Sat</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>242</th>
      <td>17.82</td>
      <td>1.75</td>
      <td>Male</td>
      <td>No</td>
      <td>Sat</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>243</th>
      <td>18.78</td>
      <td>3.00</td>
      <td>Female</td>
      <td>No</td>
      <td>Thur</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
  </tbody>
</table>

---

#### boxplot
```py
plt.figure(figsize=(8, 6))
sns.boxplot(x=tips["total_bill"])
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/a75b2556-d4e3-46ed-a7ad-fe47fe993cfc/image.png)

---

```py
plt.figure(figsize=(8, 6))
sns.boxplot(x="day", y="total_bill", data=tips)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/16744f07-160c-42f7-8cf7-63a1240a6bb7/image.png)

---

#### boxplot hue, palette option

```py

# hue: 카테고리 데이터 표현

plt.figure(figsize=(8, 6))
sns.boxplot(x="day", y="total_bill", data=tips, hue="smoker", palette="Set3") # Set 1 ~ 3
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/dc54b2b3-f250-49c6-b3ff-0e4fc63eb004/image.png)

---

#### swarmplot
- color : 0~1 사이 -> 검은색부터 흰색 사이 값 조절
```py
plt.figure(figsize=(8, 6))
sns.swarmplot(x="day", y="total_bill", data=tips, color="0")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/5b3e2b87-4090-4afe-93ab-6042e2de73b3/image.png)

---

#### boxplot with swarmplot

```py
plt.figure(figsize=(8, 6))
sns.boxplot(x="day", y="total_bill", data=tips)
sns.swarmplot(x="day", y="total_bill", data=tips, color="0")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/04950a63-f83f-4ca9-ba48-38cee5be543b/image.png)

---

#### lmplot
- total_bill과 tip 사이 관계 파악

```py
sns.set_style("darkgrid")
sns.lmplot(x="total_bill", y="tip", data=tips, height=7) # size => height
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/a3fa36ef-6ed8-47c2-9fec-d987294560a0/image.png)

---

- hue option
```python
sns.set_style("darkgrid")
sns.lmplot(x="total_bill", y="tip", data=tips, height=7, hue="smoker")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/e3000780-26f8-4277-aaf9-59cab28b561b/image.png)

---

### flights datas
- heatmap

```python
flights = sns.load_dataset("flights")
flights.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>month</th>
      <th>passengers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1949</td>
      <td>Jan</td>
      <td>112</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1949</td>
      <td>Feb</td>
      <td>118</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1949</td>
      <td>Mar</td>
      <td>132</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1949</td>
      <td>Apr</td>
      <td>129</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1949</td>
      <td>May</td>
      <td>121</td>
    </tr>
  </tbody>
</table>

---

#### pivot(index, columns, values)

```py
flights = flights.pivot(index="month", columns="year", values="passengers")
flights.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>year</th>
      <th>1949</th>
      <th>1950</th>
      <th>1951</th>
      <th>1952</th>
      <th>1953</th>
      <th>1954</th>
      <th>1955</th>
      <th>1956</th>
      <th>1957</th>
      <th>1958</th>
      <th>1959</th>
      <th>1960</th>
    </tr>
    <tr>
      <th>month</th>
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
      <th>Jan</th>
      <td>112</td>
      <td>115</td>
      <td>145</td>
      <td>171</td>
      <td>196</td>
      <td>204</td>
      <td>242</td>
      <td>284</td>
      <td>315</td>
      <td>340</td>
      <td>360</td>
      <td>417</td>
    </tr>
    <tr>
      <th>Feb</th>
      <td>118</td>
      <td>126</td>
      <td>150</td>
      <td>180</td>
      <td>196</td>
      <td>188</td>
      <td>233</td>
      <td>277</td>
      <td>301</td>
      <td>318</td>
      <td>342</td>
      <td>391</td>
    </tr>
    <tr>
      <th>Mar</th>
      <td>132</td>
      <td>141</td>
      <td>178</td>
      <td>193</td>
      <td>236</td>
      <td>235</td>
      <td>267</td>
      <td>317</td>
      <td>356</td>
      <td>362</td>
      <td>406</td>
      <td>419</td>
    </tr>
    <tr>
      <th>Apr</th>
      <td>129</td>
      <td>135</td>
      <td>163</td>
      <td>181</td>
      <td>235</td>
      <td>227</td>
      <td>269</td>
      <td>313</td>
      <td>348</td>
      <td>348</td>
      <td>396</td>
      <td>461</td>
    </tr>
    <tr>
      <th>May</th>
      <td>121</td>
      <td>125</td>
      <td>172</td>
      <td>183</td>
      <td>229</td>
      <td>234</td>
      <td>270</td>
      <td>318</td>
      <td>355</td>
      <td>363</td>
      <td>420</td>
      <td>472</td>
    </tr>
  </tbody>
</table>

#### heatmap
```py
plt.figure(figsize=(10, 8))
sns.heatmap(data=flights, annot=True, fmt="d") # annot=True : 데이터 값 표시, fmt="d" : 정수형 표현
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/22a3a10a-3f32-4ae3-a6d1-6c440249be01/image.png)

---

#### colormap

```py
plt.figure(figsize=(10, 8))
sns.heatmap(flights, annot=True, fmt="d", cmap="YlGnBu")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/e57ae578-6555-49e6-8d7a-7089f61480f1/image.png)

---

### iris data
- pairplot

```py
iris = sns.load_dataset("iris")
iris.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>145</th>
      <td>6.7</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.3</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>146</th>
      <td>6.3</td>
      <td>2.5</td>
      <td>5.0</td>
      <td>1.9</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>147</th>
      <td>6.5</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.0</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>148</th>
      <td>6.2</td>
      <td>3.4</td>
      <td>5.4</td>
      <td>2.3</td>
      <td>virginica</td>
    </tr>
    <tr>
      <th>149</th>
      <td>5.9</td>
      <td>3.0</td>
      <td>5.1</td>
      <td>1.8</td>
      <td>virginica</td>
    </tr>
  </tbody>
</table>

#### pairplot

```py
sns.set_style("ticks")
sns.pairplot(iris)
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/4d705dd9-168b-458e-aa72-12461b1e3594/image.png)

---

- hue option

```py
sns.pairplot(iris, hue="species")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/094f0196-f77a-44e9-bf2b-d4fd3204e0a6/image.png)

---

#### 원하는 컬럼만 pairplot

```py
sns.pairplot(iris, x_vars=["sepal_width", "sepal_length"], 
                   y_vars=["petal_width", "petal_length"])
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/6c888b83-23f0-4d9f-abf8-ffe74286c024/image.png)

---

### anscombe data
- lmplot
```py
anscombe = sns.load_dataset("anscombe")
anscombe.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>dataset</th>
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>IV</td>
      <td>8.0</td>
      <td>5.25</td>
    </tr>
    <tr>
      <th>40</th>
      <td>IV</td>
      <td>19.0</td>
      <td>12.50</td>
    </tr>
    <tr>
      <th>41</th>
      <td>IV</td>
      <td>8.0</td>
      <td>5.56</td>
    </tr>
    <tr>
      <th>42</th>
      <td>IV</td>
      <td>8.0</td>
      <td>7.91</td>
    </tr>
    <tr>
      <th>43</th>
      <td>IV</td>
      <td>8.0</td>
      <td>6.89</td>
    </tr>
  </tbody>
</table>

---

#### lmplot

```py
sns.set_style("darkgrid")
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'I'"), ci=None, height=7) # ci : 신뢰구간 선택
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/8da92c91-bb26-4ee6-b594-5ae2633e8128/image.png)

---

#### scatter 크기 조정
```py
sns.set_style("darkgrid")
sns.lmplot(x="x", y="y", data=anscombe.query("dataset == 'I'"), ci=None, height=7, scatter_kws={"s":80}) # ci : 신뢰구간 선택
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/581f04f8-d978-44c1-9b21-9d67bcef8ef3/image.png)

---

#### order option
```py
sns.set_style("darkgrid")
sns.lmplot(
    x="x",
    y="y",
    data=anscombe.query("dataset == 'II'"),
    order=1,
    ci=None, height=7,
    scatter_kws={"s":80}) # ci : 신뢰구간 선택
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/fa9e0e2f-3ce9-4c81-9aa6-3a4cefec31f2/image.png)

---

```py
sns.set_style("darkgrid")
sns.lmplot(
    x="x",
    y="y",
    data=anscombe.query("dataset == 'II'"),
    order=2,
    ci=None, height=7,
    scatter_kws={"s":80}) # ci : 신뢰구간 선택
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/22b6d3cb-d671-4955-ba45-1a5d63760117/image.png)

---

#### outliner

```py
sns.set_style("darkgrid")
sns.lmplot(
    x="x",
    y="y",
    data=anscombe.query("dataset == 'III'"),
    ci=None, height=7,
    scatter_kws={"s" : 80}) # ci : 신뢰구간 선택
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/bd3228e7-b74e-4543-a5e0-fcfad2349d04/image.png)

---

#### robust
```py
sns.set_style("darkgrid")
sns.lmplot(
    x="x",
    y="y",
    data=anscombe.query("dataset == 'III'"),
    robust=True,
    ci=None,
    height=7,
    scatter_kws={"s" : 80}) # ci : 신뢰구간 선택
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/03bfdf60-9d57-4e88-bbe7-42b090d71a2b/image.png)


