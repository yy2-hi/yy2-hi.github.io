---
layout: single
title: "Project 2 - 서울시 범죄 현황 데이터 분석 (1)"
categories: Data Analysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---


# Project 02. Analysis Seoul Crime
## 프로젝트 개요
![](https://velog.velcdn.com/images/yy2hi/post/6ecd2ac0-36b5-4ebc-b513-e3dc092f7e90/image.png)

- 실제 강남3구가 범죄로부터 안전한지 데이터로 확인

## 데이터 개요
```python
import numpy as np
import pandas as pd

# 데이터 읽기
crime_raw_data = pd.read_csv('../data/02. crime_in_Seoul.csv', thousands=",", encoding="euc-kr")
                                                            # thousands 숫자값을 문자로 인식할 수 있기 때문에 설정
crime_raw_data.head()
```

|	|구분|죄종|발생검거|건수|
|::|:-:|:--:|-----|:--:|
|0|	중부|	살인|	발생|	2.0|
|1|	중부|	살인|	검거|	2.0|
|2|	중부|	강도|	발생|	3.0|
|3|	중부|	강도|	검거|	3.0|
|4|	중부|	강간|	발생|	141.0|

---

```python
crime_raw_data.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 65534 entries, 0 to 65533
Data columns (total 4 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   구분      310 non-null    object 
 1   죄종      310 non-null    object 
 2   발생검거    310 non-null    object 
 3   건수      310 non-null    float64
dtypes: float64(1), object(3)
memory usage: 2.0+ MB
```
- info(): 데이터의 개요 확인하기
- RangeIndex가 65534인데, 데이터가 310개뿐인 것 확인
---
```python
crime_raw_data['죄종'].unique()

=>

array(['살인', '강도', '강간', '절도', '폭력', nan], dtype=object)
```
- 특정 컬럼에서 unique 조사
- nan 값 발견
---
```python
crime_raw_data[crime_raw_data['죄종'].isnull()].head()
```
|	|구분|죄종|발생검거|건수|
|::|:-:|:--:|-----|:--:|
|310|NaN|NaN|NaN|NaN|
|311|NaN|NaN|NaN|NaN|
|312|NaN|NaN|NaN|NaN|
|313|NaN|NaN|NaN|NaN|
|314|NaN|NaN|NaN|NaN|

---

```py
crime_raw_data = crime_raw_data[crime_raw_data['죄종'].notnull()]
crime_raw_data.info()

=>

<class 'pandas.core.frame.DataFrame'>
Int64Index: 310 entries, 0 to 309
Data columns (total 4 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   구분      310 non-null    object 
 1   죄종      310 non-null    object 
 2   발생검거    310 non-null    object 
 3   건수      310 non-null    float64
dtypes: float64(1), object(3)
memory usage: 12.1+ KB
```
---
```py
crime_raw_data.head()
```
|	|구분|죄종|발생검거|건수|
|::|:-:|:--:|-----|:--:|
|0|	중부|	살인|	발생|	2.0|
|1|	중부|	살인|	검거|	2.0|
|2|	중부|	강도|	발생|	3.0|
|3|	중부|	강도|	검거|	3.0|
|4|	중부|	강간|	발생|	141.0|

---

```py
crime_raw_data.tail()
```
|	|구분|죄종|발생검거|건수|
|::|:-:|:--:|-----|:--:|
|305|	수서|	강간|	검거|	144.0|
|306|	수서|	절도|	발생|	1149.0|
|307|	수서|	절도|	검거|	789.0|
|308|	수서|	폭력|	발생|	1666.0|
|309|	수서|	폭력|	검거|	1431.0|

---

## Pandas pivot table
- index, columns, values, aggfunc

```python
df = pd.read_excel("../data/02. sales-funnel.xlsx")
df.head()
```

|	|Account|Name|Rep|Manager|Product|Quantity|Price|Status
|:::|::-----|----|---|-------|-------|--------|-----|------|
0|714466|Trantow-Barrows|Craig Booker|Debra Henley|CPU|1|30000|presented
1|714466|Trantow-Barrows|Craig Booker|Debra Henley|Software|1|10000|presented
2|714466|Trantow-Barrows|Craig Booker|Debra Henley|Maintenance|2|5000|pending
3|737550|Fritsch, Russel and Anderson|Craig Booker|Debra Henley|CPU|1|35000|declined
4|146832|Kiehn-Spinka|Daniel Hilton|Debra Henley|CPU|2|65000|won

---

### index 설정
##### Name 컬럼을 인덱스로 설정
```python
df.pivot_table(index="Name")	# pd.pivot_table(df, index="Name")
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Barton LLC</th>
      <td>740150</td>
      <td>35000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Fritsch, Russel and Anderson</th>
      <td>737550</td>
      <td>35000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Herman LLC</th>
      <td>141962</td>
      <td>65000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Jerde-Hilpert</th>
      <td>412290</td>
      <td>5000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kassulke, Ondricka and Metz</th>
      <td>307599</td>
      <td>7000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>Keeling LLC</th>
      <td>688981</td>
      <td>100000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>Kiehn-Spinka</th>
      <td>146832</td>
      <td>65000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Koepp Ltd</th>
      <td>729833</td>
      <td>35000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kulas Inc</th>
      <td>218895</td>
      <td>25000</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th>Purdy-Kunde</th>
      <td>163416</td>
      <td>30000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Stokes LLC</th>
      <td>239344</td>
      <td>7500</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Trantow-Barrows</th>
      <td>714466</td>
      <td>15000</td>
      <td>1.333333</td>
    </tr>
  </tbody>
</table>

---
#### 멀티 인덱스 설정
```py
df.pivot_table(index=["Name", "Rep", "Manager"])
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Account</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Name</th>
      <th>Rep</th>
      <th>Manager</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Barton LLC</th>
      <th>John Smith</th>
      <th>Debra Henley</th>
      <td>740150</td>
      <td>35000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Fritsch, Russel and Anderson</th>
      <th>Craig Booker</th>
      <th>Debra Henley</th>
      <td>737550</td>
      <td>35000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Herman LLC</th>
      <th>Cedric Moss</th>
      <th>Fred Anderson</th>
      <td>141962</td>
      <td>65000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Jerde-Hilpert</th>
      <th>John Smith</th>
      <th>Debra Henley</th>
      <td>412290</td>
      <td>5000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kassulke, Ondricka and Metz</th>
      <th>Wendy Yule</th>
      <th>Fred Anderson</th>
      <td>307599</td>
      <td>7000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>Keeling LLC</th>
      <th>Wendy Yule</th>
      <th>Fred Anderson</th>
      <td>688981</td>
      <td>100000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>Kiehn-Spinka</th>
      <th>Daniel Hilton</th>
      <th>Debra Henley</th>
      <td>146832</td>
      <td>65000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Koepp Ltd</th>
      <th>Wendy Yule</th>
      <th>Fred Anderson</th>
      <td>729833</td>
      <td>35000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kulas Inc</th>
      <th>Daniel Hilton</th>
      <th>Debra Henley</th>
      <td>218895</td>
      <td>25000</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th>Purdy-Kunde</th>
      <th>Cedric Moss</th>
      <th>Fred Anderson</th>
      <td>163416</td>
      <td>30000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Stokes LLC</th>
      <th>Cedric Moss</th>
      <th>Fred Anderson</th>
      <td>239344</td>
      <td>7500</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Trantow-Barrows</th>
      <th>Craig Booker</th>
      <th>Debra Henley</th>
      <td>714466</td>
      <td>15000</td>
      <td>1.333333</td>
    </tr>
  </tbody>
</table>

---

```py
df.pivot_table(index=["Manager", "Rep"])
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Account</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>720237.0</td>
      <td>20000.000000</td>
      <td>1.250000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>194874.0</td>
      <td>38333.333333</td>
      <td>1.666667</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>576220.0</td>
      <td>20000.000000</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>196016.5</td>
      <td>27500.000000</td>
      <td>1.250000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>614061.5</td>
      <td>44250.000000</td>
      <td>3.000000</td>
    </tr>
  </tbody>
</table>

---

### values 설정
```py
df.pivot_table(index=["Manager", "Rep"], values="Price")
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>20000.000000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>38333.333333</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>20000.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>27500.000000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>44250.000000</td>
    </tr>
  </tbody>
</table>

---

#### Price 컬럼 sum 연산 적용
```py
df.pivot_table(index=["Manager", "Rep"], values="Price", aggfunc=np.sum)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>80000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>115000</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>40000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>110000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>177000</td>
    </tr>
  </tbody>
</table>

---

```py
df.pivot_table(index=["Manager", "Rep"], values="Price", aggfunc=[np.sum, len])
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th>sum</th>
      <th>len</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>Price</th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>80000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>115000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>40000</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>110000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>177000</td>
      <td>4</td>
    </tr>
  </tbody>
</table>

### columns 설정
#### Product를 컬럼으로 지정
```py
df.pivot_table(index=["Manager", "Rep"], values="Price", columns="Product", aggfunc=np.sum)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>65000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
      <td>10000.0</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>105000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10000.0</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>35000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>95000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
      <td>10000.0</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>165000.0</td>
      <td>7000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

---

#### Nan 값 설정 : fill_value
```py
df.pivot_table(index=["Manager", "Rep"], values="Price", columns="Product", aggfunc=np.sum, fill_value=0)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>65000</td>
      <td>5000</td>
      <td>0</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>105000</td>
      <td>0</td>
      <td>0</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>35000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>95000</td>
      <td>5000</td>
      <td>0</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>165000</td>
      <td>7000</td>
      <td>5000</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

---

#### 2개 이상 index, values 설정
```py
df.pivot_table(index=["Manager", "Rep", "Product"], values=["Price", "Quantity"], aggfunc=np.sum, fill_value=0)
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
      <th></th>
      <th></th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th>Product</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="7" valign="top">Debra Henley</th>
      <th rowspan="3" valign="top">Craig Booker</th>
      <th>CPU</th>
      <td>65000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Daniel Hilton</th>
      <th>CPU</th>
      <td>105000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">John Smith</th>
      <th>CPU</th>
      <td>35000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Fred Anderson</th>
      <th rowspan="3" valign="top">Cedric Moss</th>
      <th>CPU</th>
      <td>95000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Wendy Yule</th>
      <th>CPU</th>
      <td>165000</td>
      <td>7</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>7000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Monitor</th>
      <td>5000</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>

### aggfunc 2개 이상 설정
```py
df.pivot_table(
    index=["Manager", "Rep", "Product"],
    values=["Price", "Quantity"],
    aggfunc=[np.sum, np.mean], fill_value=0,
    margins=True # 총계(All) 추가
)
```
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th colspan="2" halign="left">sum</th>
      <th colspan="2" halign="left">mean</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th>Price</th>
      <th>Quantity</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th>Product</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="7" valign="top">Debra Henley</th>
      <th rowspan="3" valign="top">Craig Booker</th>
      <th>CPU</th>
      <td>65000</td>
      <td>2</td>
      <td>32500.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>2</td>
      <td>5000.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
      <td>10000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Daniel Hilton</th>
      <th>CPU</th>
      <td>105000</td>
      <td>4</td>
      <td>52500.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
      <td>10000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">John Smith</th>
      <th>CPU</th>
      <td>35000</td>
      <td>1</td>
      <td>35000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>2</td>
      <td>5000.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Fred Anderson</th>
      <th rowspan="3" valign="top">Cedric Moss</th>
      <th>CPU</th>
      <td>95000</td>
      <td>3</td>
      <td>47500.000000</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>1</td>
      <td>5000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
      <td>10000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Wendy Yule</th>
      <th>CPU</th>
      <td>165000</td>
      <td>7</td>
      <td>82500.000000</td>
      <td>3.500000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>7000</td>
      <td>3</td>
      <td>7000.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>Monitor</th>
      <td>5000</td>
      <td>2</td>
      <td>5000.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>All</th>
      <th></th>
      <th></th>
      <td>522000</td>
      <td>30</td>
      <td>30705.882353</td>
      <td>1.764706</td>
    </tr>
  </tbody>
</table>

---

### 서울시 범죄 현황 데이터 정리
#### 데이터 정리
```python
crime_station = crime_raw_data.pivot_table(
    crime_raw_data,
    index="구분",
    columns=["죄종", "발생검거"],
    aggfunc=[np.sum]
)
crime_station.head()
```
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="10" halign="left">sum</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="10" halign="left">건수</th>
    </tr>
    <tr>
      <th>죄종</th>
      <th colspan="2" halign="left">강간</th>
      <th colspan="2" halign="left">강도</th>
      <th colspan="2" halign="left">살인</th>
      <th colspan="2" halign="left">절도</th>
      <th colspan="2" halign="left">폭력</th>
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
    </tr>
  </tbody>
</table>
</div>

- 경찰서 이름을 index하도록 정리
- default가 평균(mean)
- column이 multi로 잡힌 모습

---

#### 다중 컬럼에서 특정 컬럼 제거
```python
crime_station.columns # Multiindex

=>

MultiIndex([('sum', '건수', '강간', '검거'),
            ('sum', '건수', '강간', '발생'),
            ('sum', '건수', '강도', '검거'),
            ('sum', '건수', '강도', '발생'),
            ('sum', '건수', '살인', '검거'),
            ('sum', '건수', '살인', '발생'),
            ('sum', '건수', '절도', '검거'),
            ('sum', '건수', '절도', '발생'),
            ('sum', '건수', '폭력', '검거'),
            ('sum', '건수', '폭력', '발생')],
           names=[None, None, '죄종', '발생검거'])
```
---
```python
crime_station["sum", "건수", "강도", "검거"][:5]

=>

구분
강남    26.0
강동    13.0
강북     4.0
강서    10.0
관악    10.0
Name: (sum, 건수, 강도, 검거), dtype: float64
```
---
```python
crime_station.columns = crime_station.columns.droplevel([0, 1]) #다중 컬럼에서 특정 컬럼 제거
crime_station.columns

=>

MultiIndex([('강간', '검거'),
            ('강간', '발생'),
            ('강도', '검거'),
            ('강도', '발생'),
            ('살인', '검거'),
            ('살인', '발생'),
            ('절도', '검거'),
            ('절도', '발생'),
            ('폭력', '검거'),
            ('폭력', '발생')],
           names=['죄종', '발생검거'])
```
---
```py
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
    </tr>
  </tbody>
</table>

---

```py
crime_station.index

=>

Index(['강남', '강동', '강북', '강서', '관악', '광진', '구로', '금천', '남대문', '노원', '도봉',
       '동대문', '동작', '마포', '방배', '서대문', '서부', '서초', '성동', '성북', '송파', '수서',
       '양천', '영등포', '용산', '은평', '종로', '종암', '중랑', '중부', '혜화'],
      dtype='object', name='구분')
```
→ 경찰서 이름으로 해당 구 이름을 알아내야 함

---

## Google Maps API 설치 및 테스트
```py
import googlemaps
gmaps_key = "~"
gmaps = googlemaps.Client(key=gmaps_key)
gmaps.geocode("서울영등포경찰서", language="ko")

=>

[{'address_components': [{'long_name': '608',
    'short_name': '608',
    'types': ['premise']},
   {'long_name': '국회대로',
    'short_name': '국회대로',
    'types': ['political', 'sublocality', 'sublocality_level_4']},
   {'long_name': '영등포구',
    'short_name': '영등포구',
    'types': ['political', 'sublocality', 'sublocality_level_1']},
   {'long_name': '서울특별시',
    'short_name': '서울특별시',
    'types': ['administrative_area_level_1', 'political']},
   {'long_name': '대한민국',
    'short_name': 'KR',
    'types': ['country', 'political']},
   {'long_name': '150-043',
    'short_name': '150-043',
    'types': ['postal_code']}],
  'formatted_address': '대한민국 서울특별시 영등포구 국회대로 608',
  'geometry': {'location': {'lat': 37.5260441, 'lng': 126.9008091},
   'location_type': 'ROOFTOP',
   'viewport': {'northeast': {'lat': 37.5273930802915,
     'lng': 126.9021580802915},
    'southwest': {'lat': 37.5246951197085, 'lng': 126.8994601197085}}},
  'partial_match': True,
  'place_id': 'ChIJ1TimJLaffDURptXOs0Tj6sY',
  'plus_code': {'compound_code': 'GWG2+C8 대한민국 서울특별시',
   'global_code': '8Q98GWG2+C8'},
  'types': ['establishment', 'point_of_interest', 'police']}]
```

#### 출처
서울시 관서별 5대 범죄 현황, https://www.data.go.kr/data/15054738/fileData.do?recommendDataYn=Y