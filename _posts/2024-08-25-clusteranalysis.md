---
layout: single
title: "Project 9 - 유저 군집 분석"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# 클러스터링, 군집화
![](https://velog.velcdn.com/images/yy2hi/post/6174ce34-b10a-4be3-93cf-9a5973bef2b6/image.png)

- 데이터가 주어졌을 때, 여러 개의 그룹으로 나누는 것
- 유사한 특성을 가진 그룹을 발견해내는 일

![](https://velog.velcdn.com/images/yy2hi/post/0cf56223-bfc3-4e5a-b9e6-78d9c2b89156/image.png)

- 내부 멤버들 간의 사이(Intra-clust)는 가깝고, 그룬 간 사이(Inter-cluster)는 멀게 그룹을 만드는 것
- 그룹에 대한 정답이 있으면 분류 문제이지만 **클러스터링은 비지도 학습**으로 어느 그룹에 있는지 정답은 없다.

# 언제 사용할까?
### 유사한 뉴스 그룹 : 문서 군집화
![](https://velog.velcdn.com/images/yy2hi/post/73b771ba-1157-4d32-8eac-3f5fc415f711/image.png)



### 가까운 위치 좌표끼리 묶기
![](https://velog.velcdn.com/images/yy2hi/post/b3bfb038-3889-4411-ac9d-14a12d81d956/image.png)


### 유사한 유저군 나누기 : 마켓 세분화
![](https://velog.velcdn.com/images/yy2hi/post/53ba5416-88a5-4cd7-a712-fdf49eb8fcac/image.png)

- 시장을 적절한 수로 나누고, 각 시장, 타겟 별로 효과적인 정책을 찾아낸다.

#### SNS 관심사 기반, 자율 주행 이미지 인식 클러스터링

# 종류
![](https://velog.velcdn.com/images/yy2hi/post/d43cf85a-c1b1-4dc4-991c-bc89796e0746/image.png)

## Partition-based Clustering

- 미리 군집(그룹)의 수를 정해두고 클러스터링하는 방식
- 대표적인 알고리즘
  - K-means
  - K-Medoids
  
 ### K-means Clustering
 
 1. 사용자가 미리 군집수(k) 정의
 2. 처음에는 랜덤으로 **k개의 중심점(Centroid)**를 정함
 	- 각 군집은 하나의 중심(Centroid)를 가지고 있다.
3. 데이터 point를 돌면서 **가까운 중심점이 있는 그룹에 각 데이터를 할당**
 	- 이 때 각 개체 간 거리는 Euclidean distance 사용
4. 모두 그룹을 할당했다면, **각 그룹마다 새 중심점(Centroid) - 클러스터 내 평균점**을 새로 구함
5. 3-4 단계를 반복하다가 **더 이상 그룹 이동이 일어나지 않으면 멈춤**
![](https://velog.velcdn.com/images/yy2hi/post/f0eb3d8c-963f-444d-a62f-b4b49d03be21/image.png)

---

## Hierarchical based Clustering
- 여러개의 군집 중에서 가장 유사도가 높은 혹은 **거리가 가까운 군집 두 개를 선택하여 하나로 합치면서 군집 개수를 줄여가는 방법**으로 agglomerative clustering(합체 군집)라고도 한다.
![](https://velog.velcdn.com/images/yy2hi/post/2413c7f4-30a4-4379-b7d2-ca1565d0e65a/image.png)

- 가까운 데이터를 서로 묶기 위해서는 먼저 각 군집 간 거리를 계산해야 한다.

#### Centroid Distance
- 각 군집의 중심점 사이의 거리를 계산하는 방법
- 계층 클러스터링이 아니더라도 사용할 수 있는 방법

#### Median Distance
- Hierarchical based Clustering에서 사용할 수 있는 방법
- 군집 u가 군집 s와 군집 t의 결합으로 생성된 군집이라면, 중심점을 새로 계산하지 않고, 기존 s와 t의 중심점의 평균을 사용한다 -> 더 빠른 계산 가능

---

## Density based Clustering
- 데이터가 밀집한 정도, 밀도를 이용한 클러스터링 방법

### DBSCAN clustering

- 군집의 개수를 사용자가 지정할 필요가 없다.
- 초기 데이터로부터 근접한 데이터를 찾아나가는 방법으로 군집을 확장
- 필요한 파라미터는 2가지로 '근접하다'를 정의
   - 최소거리 a (다른 점들을 이웃으로 묶기 위함)
   - 최소 데이터 개수 b (밀집 지역으로 정의하기 위함)
- 최소거리 a안에 있는 데이터는 이웃이다.
- 최소거리 a안에 최소 데이터 개수 b 이상의 데이터가 있으면, 이 데이터를 core로 정의
- core데이터는 하나의 클러스터를 형성하고, 그 core와 a 거리 내에 있는 점들은 같은 클러스터로 분류

![](https://velog.velcdn.com/images/yy2hi/post/75e21f89-6c88-4695-b7c9-5450bfb1624d/image.png)

- K-means는 중심점을 기준으로 그룹을 형성하기 때문에, **원의 형태**로 군집이 만들어짐
- 서로 이웃한 데이터들을 같은 클러스터에 포함시키기 때문에 **불특정한 모양**의 클러스터가 형성

---

# 정답이 없는 클러스터링, 어떻게 평가할까?
이미 정답이 있는 분류문제와 달리 성능 기준을 만들기 어렵다. 따라서 기준을 사용할 수 있지만, 그 중 클러스터에 대한 정답이 없는 경우 사용할 수 있는 기준을 알아보자

## Silhouette Coefficient: 실루엣 계수

- 모든 데이터 쌍 (i, j)에 대한 거리나 dissimilarity를 구한다.
   - a_i : i와 같은 군집에 속한 원소들의 평균 거리
   - b_i : i와 다른 군집 중 가장 가까운 군집까지의 평균 거리
   ![](https://velog.velcdn.com/images/yy2hi/post/b725dddc-ff3b-4f2b-ba07-4e672a8074f3/image.png)

-> 만약 a 같은 군집 내 평균 거리가 더 가깝다면 양수, 다른 군집과의 거리가 가깝다면 음수가 나온다.
![](https://velog.velcdn.com/images/yy2hi/post/85f006be-48c5-496d-9020-c7bfb8366d5d/image.png)

---

# 다양한 클러스터링 방법을 통해 유저 세그먼트를 나눠보자

## 라이브러리 불러오기
```py
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from matplotlib import colors
```

## 👣 데이터 살펴보기
```py
# 나의 구글 드라이브를 mount (colaboratory 노트북이 떠있는 위치에서 드라이브의 파일에 접근할 수 있게 만드는 것) 하는 명령어
from google.colab import drive
drive.mount('/content/drive')
```

```py
# 판다스로 데이터 불러오기
# 아래에 customer_personality_analysis.csv 가 있는 경로로 이동합니다
# 경로 설정
DRIVE_PATH = "/content/drive/MyDrive/" # 내 드라이브의 경로이다
FILE_PATH_IN_MY_DRIVE = "zerobase/유저 데이터 분석/유저 군집 분석하기/data/customer_personality_analysis.csv" # 내 드라이브 내 파일이 있는 경로
PATH = DRIVE_PATH +  FILE_PATH_IN_MY_DRIVE

df = pd.read_csv(PATH , sep="\t") # csv 파일 읽어오기
df.head()

# ID,Year_Birth,Education
# ID\tYear\t
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Year_Birth</th>
      <th>Education</th>
      <th>Marital_Status</th>
      <th>Income</th>
      <th>Kidhome</th>
      <th>Teenhome</th>
      <th>Dt_Customer</th>
      <th>Recency</th>
      <th>MntWines</th>
      <th>...</th>
      <th>NumWebVisitsMonth</th>
      <th>AcceptedCmp3</th>
      <th>AcceptedCmp4</th>
      <th>AcceptedCmp5</th>
      <th>AcceptedCmp1</th>
      <th>AcceptedCmp2</th>
      <th>Complain</th>
      <th>Z_CostContact</th>
      <th>Z_Revenue</th>
      <th>Response</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5524</td>
      <td>1957</td>
      <td>Graduation</td>
      <td>Single</td>
      <td>58138.0</td>
      <td>0</td>
      <td>0</td>
      <td>04-09-2012</td>
      <td>58</td>
      <td>635</td>
      <td>...</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>11</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2174</td>
      <td>1954</td>
      <td>Graduation</td>
      <td>Single</td>
      <td>46344.0</td>
      <td>1</td>
      <td>1</td>
      <td>08-03-2014</td>
      <td>38</td>
      <td>11</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4141</td>
      <td>1965</td>
      <td>Graduation</td>
      <td>Together</td>
      <td>71613.0</td>
      <td>0</td>
      <td>0</td>
      <td>21-08-2013</td>
      <td>26</td>
      <td>426</td>
      <td>...</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6182</td>
      <td>1984</td>
      <td>Graduation</td>
      <td>Together</td>
      <td>26646.0</td>
      <td>1</td>
      <td>0</td>
      <td>10-02-2014</td>
      <td>26</td>
      <td>11</td>
      <td>...</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>11</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5324</td>
      <td>1981</td>
      <td>PhD</td>
      <td>Married</td>
      <td>58293.0</td>
      <td>1</td>
      <td>0</td>
      <td>19-01-2014</td>
      <td>94</td>
      <td>173</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>11</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

```py
print("데이터 전체의 행 수: ",len(df))
print("데이터 컬럼 수: ",len(df.columns))

=>

데이터 전체의 행 수:  2240
데이터 컬럼 수:  29
```

## 데이터 컬럼 종류

### People (사람)

- ID: Customer's unique identifier
- Year_Birth: Customer's birth year
- Education: Customer's education level (교육 수준)
- Marital_Status: Customer's marital status (결혼 상태)
- Income: Customer's yearly household income
- Kidhome: Number of children in customer's household (어린 아이의 수)
- Teenhome: Number of teenagers in customer's household (10대 수)
- Dt_Customer: Date of customer's enrollment with the company (서비스 가입 날짜)
- Recency: Number of days since customer's last purchase (마지막으로 구매한 날로부터 얼마가 지났는지)
- Complain: 1 if customer complained in the last 2 years, 0 otherwise

### Products (상품)

- MntWines: Amount spent on **wine** in last 2 years
- MntFruits: Amount spent on **fruits** in last 2 years
- MntMeatProducts: Amount spent on **meat** in last 2 years
- MntFishProducts: Amount spent on **fish** in last 2 years
- MntSweetProducts: Amount spent on **sweets** in last 2 years
- MntGoldProds: Amount spent on **gold** in last 2 years

### Promotion (프로모션)

- NumDealsPurchases: Number of purchases made with a discount (할인 받아 구매한 수)
- AcceptedCmp1: 1 if customer accepted the offer in the 1st campaign, 0 otherwise
- AcceptedCmp2: 1 if customer accepted the offer in the 2nd campaign, 0 otherwise
- AcceptedCmp3: 1 if customer accepted the offer in the 3rd campaign, 0 otherwise
- AcceptedCmp4: 1 if customer accepted the offer in the 4th campaign, 0 otherwise
- AcceptedCmp5: 1 if customer accepted the offer in the 5th campaign, 0 otherwise
- Response: 1 if customer accepted the offer in the last campaign, 0 otherwise

### Place (구매 장소)

- NumWebPurchases: Number of purchases made through the company’s web site
- NumCatalogPurchases: Number of purchases made using a catalogue
- NumStorePurchases: Number of purchases made directly in stores
- NumWebVisitsMonth: Number of visits to company’s web site in the last month

## 👣 데이터 정제하기: Cleaning

- 결측치 (Missing Values)와 이상치 (Outliers) 를 제거하자
- 데이터를 바로 사용할 수 있으면 좋겠지만, 바로 사용할 수 있는 경우가 많지 않다.
    - 결측치가 있을 수 있고, 컴퓨터가 이해할 수 있는 형태 (숫자형)으로 바꿔주어야 하는 경우도 있다.
    - Outlier 가 너무 크다면, 이상치로 인해 모델이 왜곡될 수 있다.
    
아래 데이터를 살펴보자
```
# .info() 함수는 데이터에 대한 전반적인 정보를 나타냅니다. 
# df를 구성하는 행과 열의 크기, 컬럼명, 컬럼을 구성하는 값의 자료형 등을 출력해줍니다.
df.info()

=>

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2240 entries, 0 to 2239
Data columns (total 29 columns):
 #   Column               Non-Null Count  Dtype  
---  ------               --------------  -----  
 0   ID                   2240 non-null   int64  
 1   Year_Birth           2240 non-null   int64  
 2   Education            2240 non-null   object 
 3   Marital_Status       2240 non-null   object 
 4   Income               2216 non-null   float64
 5   Kidhome              2240 non-null   int64  
 6   Teenhome             2240 non-null   int64  
 7   Dt_Customer          2240 non-null   object 
 8   Recency              2240 non-null   int64  
 9   MntWines             2240 non-null   int64  
 10  MntFruits            2240 non-null   int64  
 11  MntMeatProducts      2240 non-null   int64  
 12  MntFishProducts      2240 non-null   int64  
 13  MntSweetProducts     2240 non-null   int64  
 14  MntGoldProds         2240 non-null   int64  
 15  NumDealsPurchases    2240 non-null   int64  
 16  NumWebPurchases      2240 non-null   int64  
 17  NumCatalogPurchases  2240 non-null   int64  
 18  NumStorePurchases    2240 non-null   int64  
 19  NumWebVisitsMonth    2240 non-null   int64  
 20  AcceptedCmp3         2240 non-null   int64  
 21  AcceptedCmp4         2240 non-null   int64  
 22  AcceptedCmp5         2240 non-null   int64  
 23  AcceptedCmp1         2240 non-null   int64  
 24  AcceptedCmp2         2240 non-null   int64  
 25  Complain             2240 non-null   int64  
 26  Z_CostContact        2240 non-null   int64  
 27  Z_Revenue            2240 non-null   int64  
 28  Response             2240 non-null   int64  
dtypes: float64(1), int64(25), object(3)
memory usage: 507.6+ KB
```

- 4 Income : non-null인 row의 수는 2216 개 - 비어있는 값이 24개 정도 있다.
- 7 Dt_Customer : 날짜형이지만 날짜가 아닌 String(object) 로 표시되어 있다.

**결측치 제거하기**
- 결측치를 제거하는 방법은 크게 3가지이다.
    - 결측치의 비중이 작다면 제거한다.
    - 결측치를 빈도가 가장 높은 값이나 평균으로 채운다.
    - 결측치를 예측하는 모델을 만들어 예측값으로 채운다.
- 해당 데이터에서는 24개로 결측치가 많지 않기 때문에 제거한다.

```py
# missing value 가 있는 row 를 제거한다.
# 결측치가 있는 경우 1. 평균치로 채우거나 2. 예측하거나 3. 데이터가 많지 않으면 제거한다.
df = df.dropna()
len(df)

=>

2216
```
**날짜 데이터 정제하기**
- 가입한지 얼마 되지 않은 고객과 가장 오래된 고객을 구해보자

```py
# string으로 된 date 를 datetime 함수를 쓰기 위해 datetime 형태로 바꾼다.
# Dt_Customer: 가입한 날짜
df["Dt_Customer"] = pd.to_datetime(df["Dt_Customer"])
```
```py
for i in df["Dt_Customer"][:5]:
  print(i.date())
  
=>

2012-04-09
2014-08-03
2013-08-21
2014-10-02
2014-01-19
```
```py
dates = []
for i in df["Dt_Customer"]:
    i = i.date()
    dates.append(i)  
    
print(dates)
print(max(dates))
print(min(dates))

=>

[datetime.date(2012, 4, 9), datetime.date(2014, 8, 3), datetime.date(2013, 8, 21), ... , datetime.date(2014, 1, 25), datetime.date(2014, 1, 24), datetime.date(2012, 10, 15)]
2014-12-06
2012-01-08
```

* 날짜를 숫자로! 가입 날짜(date)를 가입한 후 지난 일수(int)로 바꿔준다. 

```py
days = [] # 데이터를 담을 수 있는 빈 list
recent_date = max(dates) # 2014-12-06, 
# 가장 최근 가입일 (해당 날짜를 기준으로 기존 날짜의 값을 빼준다.)
for date in dates:
    day_difference = recent_date - date
    days.append(day_difference)
df["Customer_For"] = days


df["Customer_For"].head()

=>

0   971 days
1   125 days
2   472 days
3    65 days
4   321 days
Name: Customer_For, dtype: timedelta64[ns]
```

```py
# 날짜를 숫자로 type 을 변경해준다
df["Customer_For"] = pd.to_numeric(df["Customer_For"], errors="coerce")

# errors: error는 총 3개의 옵션이 존재합니다.
# errors = 'ignore' -> 만약 숫자로 변경할 수 없는 데이터라면 숫자로 변경하지 않고 원본 데이터를 그대로 반환합니다.
# errors = 'coerce' -> 만약 숫자로 변경할 수 없는 데이터라면 기존 데이터를 지우고 NaN으로 설정하여 반환합니다.
# errors = 'raise' -> 만약 숫자로 변경할 수 없는 데이터라면 에러를 일으키며 코드를 중단합니다.
```

**카테고리 데이터 정리하기**
- 앞에서 본 데이터 중 카테고리 형태의 데이터는 Education 과 Marital_Status :)

```py
df[['Education', 'Marital_Status']].head(10)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Education</th>
      <th>Marital_Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Graduation</td>
      <td>Single</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Graduation</td>
      <td>Single</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Graduation</td>
      <td>Together</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Graduation</td>
      <td>Together</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PhD</td>
      <td>Married</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Master</td>
      <td>Together</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Graduation</td>
      <td>Divorced</td>
    </tr>
    <tr>
      <th>7</th>
      <td>PhD</td>
      <td>Married</td>
    </tr>
    <tr>
      <th>8</th>
      <td>PhD</td>
      <td>Together</td>
    </tr>
    <tr>
      <th>9</th>
      <td>PhD</td>
      <td>Together</td>
    </tr>
  </tbody>
</table>

```py
df["Marital_Status"].value_counts()

=>

Married     864
Together    580
Single      480
Divorced    232
Widow        77
Alone         3
Absurd        2
YOLO          2
Name: Marital_Status, dtype: int64
```

```py
df["Education"].value_counts()

=>

Graduation    1127
PhD            486
Master         370
2n Cycle       203
Basic           54
Name: Education, dtype: int64
```
```py
# Marital_Status 정리하기, 파트너와 같이 사는지, 혼자사는지 여부
df["Living_With"]=(
    df["Marital_Status"]
    .replace(
      {"Married":"Partner", 
       "Together":"Partner", 
       "Absurd":"Alone", 
       "Widow":"Alone", 
       "YOLO":"Alone", 
       "Divorced":"Alone", 
       "Single":"Alone"
    })
)

# the number of children, Kidhome 과 Teenhome 을 분리하지 않고 합쳐준다.
df["Children"]=df["Kidhome"]+df["Teenhome"]

# 위의 데이터를 통해 가족 사이즈도 구할 수 있다.
df["Family_Size"] = (
    df["Living_With"].replace({"Alone": 1, "Partner":2})
    + df["Children"]
)

# 아이가 있는지, 없는지
df["Is_Parent"] = np.where(df.Children> 0, 1, 0)
```
```py
# 교육 상태 정리하기
df["Education"]=df["Education"].replace({"Basic":"Undergraduate","2n Cycle":"Undergraduate", "Graduation":"Graduate", "Master":"Postgraduate", "PhD":"Postgraduate"})
```
```py
# 생년 월일을 통해 나이를 구할 수 있다.
df["Age"] = 2021-df["Year_Birth"]

# 다양한 잡화 구매를 더해서 총 사용한 비용 Spent 를 구한다.
df["Spent"] = df["MntWines"]+ df["MntFruits"]+ df["MntMeatProducts"]+ df["MntFishProducts"]+ df["MntSweetProducts"]+ df["MntGoldProds"]
```
```py
# 모두 동일한 값, 필요없는 컬럼
df.Z_CostContact.value_counts()
df.Z_Revenue.value_counts()

=>

11    2216
Name: Z_Revenue, dtype: int64
```
```py
# 컬럼명 짧게 변경
df=df.rename(columns={"MntWines": "Wines","MntFruits":"Fruits","MntMeatProducts":"Meat","MntFishProducts":"Fish","MntSweetProducts":"Sweets","MntGoldProds":"Gold"})

# 중복되거나 필요없는 컬럼 제거
to_drop = ["Marital_Status", "Dt_Customer", "Z_CostContact", "Z_Revenue", "Year_Birth", "ID"]
df = df.drop(to_drop, axis=1)


df.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Education</th>
      <th>Income</th>
      <th>Kidhome</th>
      <th>Teenhome</th>
      <th>Recency</th>
      <th>Wines</th>
      <th>Fruits</th>
      <th>Meat</th>
      <th>Fish</th>
      <th>Sweets</th>
      <th>...</th>
      <th>AcceptedCmp2</th>
      <th>Complain</th>
      <th>Response</th>
      <th>Customer_For</th>
      <th>Living_With</th>
      <th>Children</th>
      <th>Family_Size</th>
      <th>Is_Parent</th>
      <th>Age</th>
      <th>Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Graduate</td>
      <td>58138.0</td>
      <td>0</td>
      <td>0</td>
      <td>58</td>
      <td>635</td>
      <td>88</td>
      <td>546</td>
      <td>172</td>
      <td>88</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>83894400000000000</td>
      <td>Alone</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>64</td>
      <td>1617</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Graduate</td>
      <td>46344.0</td>
      <td>1</td>
      <td>1</td>
      <td>38</td>
      <td>11</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10800000000000000</td>
      <td>Alone</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>67</td>
      <td>27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Graduate</td>
      <td>71613.0</td>
      <td>0</td>
      <td>0</td>
      <td>26</td>
      <td>426</td>
      <td>49</td>
      <td>127</td>
      <td>111</td>
      <td>21</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>40780800000000000</td>
      <td>Partner</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>56</td>
      <td>776</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Graduate</td>
      <td>26646.0</td>
      <td>1</td>
      <td>0</td>
      <td>26</td>
      <td>11</td>
      <td>4</td>
      <td>20</td>
      <td>10</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5616000000000000</td>
      <td>Partner</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>37</td>
      <td>53</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Postgraduate</td>
      <td>58293.0</td>
      <td>1</td>
      <td>0</td>
      <td>94</td>
      <td>173</td>
      <td>43</td>
      <td>118</td>
      <td>46</td>
      <td>27</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>27734400000000000</td>
      <td>Partner</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>40</td>
      <td>422</td>
    </tr>
  </tbody>
</table>

 **Outlier 제거하기**
 우리가 정리한 데이터를 다시 한 번 살펴보자
 
 ```py
 df.describe()
 ```
 <table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Kidhome</th>
      <th>Teenhome</th>
      <th>Recency</th>
      <th>Wines</th>
      <th>Fruits</th>
      <th>Meat</th>
      <th>Fish</th>
      <th>Sweets</th>
      <th>Gold</th>
      <th>...</th>
      <th>AcceptedCmp1</th>
      <th>AcceptedCmp2</th>
      <th>Complain</th>
      <th>Response</th>
      <th>Customer_For</th>
      <th>Children</th>
      <th>Family_Size</th>
      <th>Is_Parent</th>
      <th>Age</th>
      <th>Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>...</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2.216000e+03</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
      <td>2216.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>52247.251354</td>
      <td>0.441787</td>
      <td>0.505415</td>
      <td>49.012635</td>
      <td>305.091606</td>
      <td>26.356047</td>
      <td>166.995939</td>
      <td>37.637635</td>
      <td>27.028881</td>
      <td>43.965253</td>
      <td>...</td>
      <td>0.064079</td>
      <td>0.013538</td>
      <td>0.009477</td>
      <td>0.150271</td>
      <td>4.423735e+16</td>
      <td>0.947202</td>
      <td>2.592509</td>
      <td>0.714350</td>
      <td>52.179603</td>
      <td>607.075361</td>
    </tr>
    <tr>
      <th>std</th>
      <td>25173.076661</td>
      <td>0.536896</td>
      <td>0.544181</td>
      <td>28.948352</td>
      <td>337.327920</td>
      <td>39.793917</td>
      <td>224.283273</td>
      <td>54.752082</td>
      <td>41.072046</td>
      <td>51.815414</td>
      <td>...</td>
      <td>0.244950</td>
      <td>0.115588</td>
      <td>0.096907</td>
      <td>0.357417</td>
      <td>2.008532e+16</td>
      <td>0.749062</td>
      <td>0.905722</td>
      <td>0.451825</td>
      <td>11.985554</td>
      <td>602.900476</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1730.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000e+00</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>25.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>35303.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>24.000000</td>
      <td>24.000000</td>
      <td>2.000000</td>
      <td>16.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>9.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.937600e+16</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>44.000000</td>
      <td>69.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>51381.500000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>49.000000</td>
      <td>174.500000</td>
      <td>8.000000</td>
      <td>68.000000</td>
      <td>12.000000</td>
      <td>8.000000</td>
      <td>24.500000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>4.432320e+16</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>51.000000</td>
      <td>396.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>68522.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>74.000000</td>
      <td>505.000000</td>
      <td>33.000000</td>
      <td>232.250000</td>
      <td>50.000000</td>
      <td>33.000000</td>
      <td>56.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>5.927040e+16</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>62.000000</td>
      <td>1048.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>666666.000000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>99.000000</td>
      <td>1493.000000</td>
      <td>199.000000</td>
      <td>1725.000000</td>
      <td>259.000000</td>
      <td>262.000000</td>
      <td>321.000000</td>
      <td>...</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>9.184320e+16</td>
      <td>3.000000</td>
      <td>5.000000</td>
      <td>1.000000</td>
      <td>128.000000</td>
      <td>2525.000000</td>
    </tr>
  </tbody>
</table>

-> Income 과 Age 의 Max 를 보자. Outlier 가 숨어있는 것 같다!

```py
# 색상 지정
sns.set(rc={"axes.facecolor":"#FFF9ED","figure.facecolor":"#FFF9ED"})
pallet = ["#682F2F", "#9E726F", "#D6B2B1", "#B9C0C9", "#9F8A78", "#F3AB60"]
cmap = colors.ListedColormap(["#682F2F", "#9E726F", "#D6B2B1", "#B9C0C9", "#9F8A78", "#F3AB60"])

# 아래의 정해진 컬럼들 사이의 상관관계를 그래프로 확인해 본다.
To_Plot = [ "Income", "Recency", "Customer_For", "Age", "Spent", "Is_Parent"]
print("Reletive Plot Of Some Selected Features: A Data Subset")
sns.pairplot(df[To_Plot], hue= "Is_Parent", palette=(["#682F2F","#F3AB60"]))
```



```py
# Outlier (Age, Income) 삭제
df = df[(df["Age"]< 90)]
df = df[(df["Income"] < 600000)]
print("Outlier 제거 후 데이터 row 수:", len(df))


# box plot 이나 분포를 보고도 알 수 있음.

=>

Outlier 제거 후 데이터 row 수: 2212
```

### 💡 Correlation Coefficients

- correlation (상관성)이란?
  - 상관성은 두 변수간의 “선형적” 관계의 정도를 의미
  - -1 ~ 1 사이를 가지며 1에 가까울 수록 양의 선형관계, -1에 가까울 수록 음의 선형관계가 강하다는 것을 의미
  
![](https://velog.velcdn.com/images/yy2hi/post/b8e521b9-0202-4d2f-ae12-09845888f741/image.png)
  
![](https://velog.velcdn.com/images/yy2hi/post/cb466b6b-3e21-48b5-bd7e-a0f57f9a12fd/image.png)

```py
df.corr()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Income</th>
      <th>Kidhome</th>
      <th>Teenhome</th>
      <th>Recency</th>
      <th>Wines</th>
      <th>Fruits</th>
      <th>Meat</th>
      <th>Fish</th>
      <th>Sweets</th>
      <th>Gold</th>
      <th>...</th>
      <th>AcceptedCmp1</th>
      <th>AcceptedCmp2</th>
      <th>Complain</th>
      <th>Response</th>
      <th>Customer_For</th>
      <th>Children</th>
      <th>Family_Size</th>
      <th>Is_Parent</th>
      <th>Age</th>
      <th>Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Income</th>
      <td>1.000000</td>
      <td>-0.514523</td>
      <td>0.034565</td>
      <td>0.007965</td>
      <td>0.688209</td>
      <td>0.507354</td>
      <td>0.692279</td>
      <td>0.520040</td>
      <td>0.523599</td>
      <td>0.388299</td>
      <td>...</td>
      <td>0.327524</td>
      <td>0.104036</td>
      <td>-0.027900</td>
      <td>0.161387</td>
      <td>-0.027892</td>
      <td>-0.343529</td>
      <td>-0.286638</td>
      <td>-0.403132</td>
      <td>0.199977</td>
      <td>0.792740</td>
    </tr>
    <tr>
      <th>Kidhome</th>
      <td>-0.514523</td>
      <td>1.000000</td>
      <td>-0.039066</td>
      <td>0.010623</td>
      <td>-0.497203</td>
      <td>-0.373258</td>
      <td>-0.439031</td>
      <td>-0.388643</td>
      <td>-0.377843</td>
      <td>-0.354922</td>
      <td>...</td>
      <td>-0.174261</td>
      <td>-0.081911</td>
      <td>0.037067</td>
      <td>-0.077901</td>
      <td>-0.057731</td>
      <td>0.688081</td>
      <td>0.583250</td>
      <td>0.520355</td>
      <td>-0.237497</td>
      <td>-0.557949</td>
    </tr>
    <tr>
      <th>Teenhome</th>
      <td>0.034565</td>
      <td>-0.039066</td>
      <td>1.000000</td>
      <td>0.014392</td>
      <td>0.003945</td>
      <td>-0.175905</td>
      <td>-0.261134</td>
      <td>-0.205235</td>
      <td>-0.163107</td>
      <td>-0.018579</td>
      <td>...</td>
      <td>-0.145198</td>
      <td>-0.015633</td>
      <td>0.007746</td>
      <td>-0.154402</td>
      <td>0.008986</td>
      <td>0.698199</td>
      <td>0.594481</td>
      <td>0.587993</td>
      <td>0.361932</td>
      <td>-0.137964</td>
    </tr>
    <tr>
      <th>Recency</th>
      <td>0.007965</td>
      <td>0.010623</td>
      <td>0.014392</td>
      <td>1.000000</td>
      <td>0.015981</td>
      <td>-0.005257</td>
      <td>0.022914</td>
      <td>0.000788</td>
      <td>0.025244</td>
      <td>0.018148</td>
      <td>...</td>
      <td>-0.021147</td>
      <td>-0.001429</td>
      <td>0.005713</td>
      <td>-0.200114</td>
      <td>0.030748</td>
      <td>0.018062</td>
      <td>0.014717</td>
      <td>0.002189</td>
      <td>0.015694</td>
      <td>0.020479</td>
    </tr>
    <tr>
      <th>Wines</th>
      <td>0.688209</td>
      <td>-0.497203</td>
      <td>0.003945</td>
      <td>0.015981</td>
      <td>1.000000</td>
      <td>0.385844</td>
      <td>0.568081</td>
      <td>0.396915</td>
      <td>0.389583</td>
      <td>0.391461</td>
      <td>...</td>
      <td>0.351610</td>
      <td>0.206309</td>
      <td>-0.036420</td>
      <td>0.246320</td>
      <td>0.148745</td>
      <td>-0.353356</td>
      <td>-0.296702</td>
      <td>-0.341994</td>
      <td>0.164615</td>
      <td>0.892996</td>
    </tr>
    <tr>
      <th>Fruits</th>
      <td>0.507354</td>
      <td>-0.373258</td>
      <td>-0.175905</td>
      <td>-0.005257</td>
      <td>0.385844</td>
      <td>1.000000</td>
      <td>0.546740</td>
      <td>0.593038</td>
      <td>0.571474</td>
      <td>0.393459</td>
      <td>...</td>
      <td>0.192417</td>
      <td>-0.009924</td>
      <td>-0.002956</td>
      <td>0.123007</td>
      <td>0.059828</td>
      <td>-0.395161</td>
      <td>-0.341414</td>
      <td>-0.410657</td>
      <td>0.013447</td>
      <td>0.612129</td>
    </tr>
    <tr>
      <th>Meat</th>
      <td>0.692279</td>
      <td>-0.439031</td>
      <td>-0.261134</td>
      <td>0.022914</td>
      <td>0.568081</td>
      <td>0.546740</td>
      <td>1.000000</td>
      <td>0.572986</td>
      <td>0.534624</td>
      <td>0.357556</td>
      <td>...</td>
      <td>0.313379</td>
      <td>0.043549</td>
      <td>-0.021017</td>
      <td>0.237966</td>
      <td>0.071381</td>
      <td>-0.504176</td>
      <td>-0.429948</td>
      <td>-0.574147</td>
      <td>0.033622</td>
      <td>0.845543</td>
    </tr>
    <tr>
      <th>Fish</th>
      <td>0.520040</td>
      <td>-0.388643</td>
      <td>-0.205235</td>
      <td>0.000788</td>
      <td>0.396915</td>
      <td>0.593038</td>
      <td>0.572986</td>
      <td>1.000000</td>
      <td>0.583484</td>
      <td>0.426299</td>
      <td>...</td>
      <td>0.261712</td>
      <td>0.002322</td>
      <td>-0.019098</td>
      <td>0.108135</td>
      <td>0.078042</td>
      <td>-0.427482</td>
      <td>-0.363522</td>
      <td>-0.449596</td>
      <td>0.041154</td>
      <td>0.641884</td>
    </tr>
    <tr>
      <th>Sweets</th>
      <td>0.523599</td>
      <td>-0.377843</td>
      <td>-0.163107</td>
      <td>0.025244</td>
      <td>0.389583</td>
      <td>0.571474</td>
      <td>0.534624</td>
      <td>0.583484</td>
      <td>1.000000</td>
      <td>0.356754</td>
      <td>...</td>
      <td>0.245113</td>
      <td>0.010142</td>
      <td>-0.020569</td>
      <td>0.116059</td>
      <td>0.076345</td>
      <td>-0.389152</td>
      <td>-0.330705</td>
      <td>-0.402064</td>
      <td>0.021516</td>
      <td>0.606652</td>
    </tr>
    <tr>
      <th>Gold</th>
      <td>0.388299</td>
      <td>-0.354922</td>
      <td>-0.018579</td>
      <td>0.018148</td>
      <td>0.391461</td>
      <td>0.393459</td>
      <td>0.357556</td>
      <td>0.426299</td>
      <td>0.356754</td>
      <td>1.000000</td>
      <td>...</td>
      <td>0.170853</td>
      <td>0.050976</td>
      <td>-0.030166</td>
      <td>0.141096</td>
      <td>0.145632</td>
      <td>-0.267776</td>
      <td>-0.235826</td>
      <td>-0.245380</td>
      <td>0.059779</td>
      <td>0.527101</td>
    </tr>
    <tr>
      <th>NumDealsPurchases</th>
      <td>-0.108207</td>
      <td>0.216594</td>
      <td>0.386805</td>
      <td>0.002591</td>
      <td>0.009117</td>
      <td>-0.134191</td>
      <td>-0.121128</td>
      <td>-0.143147</td>
      <td>-0.121395</td>
      <td>0.053047</td>
      <td>...</td>
      <td>-0.127586</td>
      <td>-0.038064</td>
      <td>0.003744</td>
      <td>0.003226</td>
      <td>0.199994</td>
      <td>0.436072</td>
      <td>0.373986</td>
      <td>0.388593</td>
      <td>0.066156</td>
      <td>-0.065571</td>
    </tr>
    <tr>
      <th>NumWebPurchases</th>
      <td>0.459265</td>
      <td>-0.372327</td>
      <td>0.162239</td>
      <td>-0.005680</td>
      <td>0.553663</td>
      <td>0.302301</td>
      <td>0.306841</td>
      <td>0.299428</td>
      <td>0.333608</td>
      <td>0.407873</td>
      <td>...</td>
      <td>0.159100</td>
      <td>0.034722</td>
      <td>-0.013524</td>
      <td>0.151084</td>
      <td>0.171834</td>
      <td>-0.148938</td>
      <td>-0.121879</td>
      <td>-0.073473</td>
      <td>0.162265</td>
      <td>0.529095</td>
    </tr>
    <tr>
      <th>NumCatalogPurchases</th>
      <td>0.696589</td>
      <td>-0.504598</td>
      <td>-0.112477</td>
      <td>0.024197</td>
      <td>0.634237</td>
      <td>0.485611</td>
      <td>0.733787</td>
      <td>0.532241</td>
      <td>0.494623</td>
      <td>0.441656</td>
      <td>...</td>
      <td>0.309130</td>
      <td>0.099931</td>
      <td>-0.018675</td>
      <td>0.219912</td>
      <td>0.091391</td>
      <td>-0.443199</td>
      <td>-0.372319</td>
      <td>-0.452734</td>
      <td>0.125856</td>
      <td>0.780250</td>
    </tr>
    <tr>
      <th>NumStorePurchases</th>
      <td>0.631424</td>
      <td>-0.501863</td>
      <td>0.049212</td>
      <td>-0.000460</td>
      <td>0.640219</td>
      <td>0.459875</td>
      <td>0.486349</td>
      <td>0.457885</td>
      <td>0.455150</td>
      <td>0.390693</td>
      <td>...</td>
      <td>0.178462</td>
      <td>0.085146</td>
      <td>-0.011947</td>
      <td>0.035563</td>
      <td>0.104245</td>
      <td>-0.323823</td>
      <td>-0.265916</td>
      <td>-0.284891</td>
      <td>0.138998</td>
      <td>0.675981</td>
    </tr>
    <tr>
      <th>NumWebVisitsMonth</th>
      <td>-0.650257</td>
      <td>0.447258</td>
      <td>0.130985</td>
      <td>-0.018965</td>
      <td>-0.321616</td>
      <td>-0.417741</td>
      <td>-0.539194</td>
      <td>-0.446151</td>
      <td>-0.422289</td>
      <td>-0.245973</td>
      <td>...</td>
      <td>-0.195200</td>
      <td>-0.007483</td>
      <td>0.020820</td>
      <td>-0.002625</td>
      <td>0.255436</td>
      <td>0.415558</td>
      <td>0.345316</td>
      <td>0.475856</td>
      <td>-0.120282</td>
      <td>-0.498769</td>
    </tr>
    <tr>
      <th>AcceptedCmp3</th>
      <td>-0.015152</td>
      <td>0.016135</td>
      <td>-0.042797</td>
      <td>-0.032361</td>
      <td>0.061360</td>
      <td>0.014644</td>
      <td>0.018416</td>
      <td>-0.000276</td>
      <td>0.001660</td>
      <td>0.125557</td>
      <td>...</td>
      <td>0.095562</td>
      <td>0.071649</td>
      <td>0.009620</td>
      <td>0.253849</td>
      <td>-0.006858</td>
      <td>-0.019518</td>
      <td>-0.026126</td>
      <td>-0.005472</td>
      <td>-0.061097</td>
      <td>0.053037</td>
    </tr>
    <tr>
      <th>AcceptedCmp4</th>
      <td>0.219633</td>
      <td>-0.162111</td>
      <td>0.038168</td>
      <td>0.017520</td>
      <td>0.373349</td>
      <td>0.006598</td>
      <td>0.091677</td>
      <td>0.016058</td>
      <td>0.029206</td>
      <td>0.024305</td>
      <td>...</td>
      <td>0.242681</td>
      <td>0.295015</td>
      <td>-0.027030</td>
      <td>0.180032</td>
      <td>0.013873</td>
      <td>-0.088427</td>
      <td>-0.076698</td>
      <td>-0.076936</td>
      <td>0.070035</td>
      <td>0.249118</td>
    </tr>
    <tr>
      <th>AcceptedCmp5</th>
      <td>0.395569</td>
      <td>-0.204582</td>
      <td>-0.190119</td>
      <td>0.000233</td>
      <td>0.472889</td>
      <td>0.208990</td>
      <td>0.375252</td>
      <td>0.194793</td>
      <td>0.258417</td>
      <td>0.176628</td>
      <td>...</td>
      <td>0.409420</td>
      <td>0.222918</td>
      <td>-0.008378</td>
      <td>0.324891</td>
      <td>-0.023459</td>
      <td>-0.284635</td>
      <td>-0.225671</td>
      <td>-0.346693</td>
      <td>-0.019025</td>
      <td>0.468695</td>
    </tr>
    <tr>
      <th>AcceptedCmp1</th>
      <td>0.327524</td>
      <td>-0.174261</td>
      <td>-0.145198</td>
      <td>-0.021147</td>
      <td>0.351610</td>
      <td>0.192417</td>
      <td>0.313379</td>
      <td>0.261712</td>
      <td>0.245113</td>
      <td>0.170853</td>
      <td>...</td>
      <td>1.000000</td>
      <td>0.176595</td>
      <td>-0.025018</td>
      <td>0.297212</td>
      <td>-0.037171</td>
      <td>-0.230291</td>
      <td>-0.185711</td>
      <td>-0.279387</td>
      <td>0.011941</td>
      <td>0.381354</td>
    </tr>
    <tr>
      <th>AcceptedCmp2</th>
      <td>0.104036</td>
      <td>-0.081911</td>
      <td>-0.015633</td>
      <td>-0.001429</td>
      <td>0.206309</td>
      <td>-0.009924</td>
      <td>0.043549</td>
      <td>0.002322</td>
      <td>0.010142</td>
      <td>0.050976</td>
      <td>...</td>
      <td>0.176595</td>
      <td>1.000000</td>
      <td>-0.011200</td>
      <td>0.169234</td>
      <td>0.006030</td>
      <td>-0.070037</td>
      <td>-0.059505</td>
      <td>-0.081575</td>
      <td>0.007821</td>
      <td>0.136336</td>
    </tr>
    <tr>
      <th>Complain</th>
      <td>-0.027900</td>
      <td>0.037067</td>
      <td>0.007746</td>
      <td>0.005713</td>
      <td>-0.036420</td>
      <td>-0.002956</td>
      <td>-0.021017</td>
      <td>-0.019098</td>
      <td>-0.020569</td>
      <td>-0.030166</td>
      <td>...</td>
      <td>-0.025018</td>
      <td>-0.011200</td>
      <td>1.000000</td>
      <td>-0.000145</td>
      <td>0.041662</td>
      <td>0.032181</td>
      <td>0.027081</td>
      <td>0.018124</td>
      <td>0.004602</td>
      <td>-0.034135</td>
    </tr>
    <tr>
      <th>Response</th>
      <td>0.161387</td>
      <td>-0.077901</td>
      <td>-0.154402</td>
      <td>-0.200114</td>
      <td>0.246320</td>
      <td>0.123007</td>
      <td>0.237966</td>
      <td>0.108135</td>
      <td>0.116059</td>
      <td>0.141096</td>
      <td>...</td>
      <td>0.297212</td>
      <td>0.169234</td>
      <td>-0.000145</td>
      <td>1.000000</td>
      <td>0.175743</td>
      <td>-0.167937</td>
      <td>-0.218383</td>
      <td>-0.203885</td>
      <td>-0.020937</td>
      <td>0.264443</td>
    </tr>
    <tr>
      <th>Customer_For</th>
      <td>-0.027892</td>
      <td>-0.057731</td>
      <td>0.008986</td>
      <td>0.030748</td>
      <td>0.148745</td>
      <td>0.059828</td>
      <td>0.071381</td>
      <td>0.078042</td>
      <td>0.076345</td>
      <td>0.145632</td>
      <td>...</td>
      <td>-0.037171</td>
      <td>0.006030</td>
      <td>0.041662</td>
      <td>0.175743</td>
      <td>1.000000</td>
      <td>-0.034836</td>
      <td>-0.027803</td>
      <td>-0.005195</td>
      <td>-0.021057</td>
      <td>0.138590</td>
    </tr>
    <tr>
      <th>Children</th>
      <td>-0.343529</td>
      <td>0.688081</td>
      <td>0.698199</td>
      <td>0.018062</td>
      <td>-0.353356</td>
      <td>-0.395161</td>
      <td>-0.504176</td>
      <td>-0.427482</td>
      <td>-0.389152</td>
      <td>-0.267776</td>
      <td>...</td>
      <td>-0.230291</td>
      <td>-0.070037</td>
      <td>0.032181</td>
      <td>-0.167937</td>
      <td>-0.034836</td>
      <td>1.000000</td>
      <td>0.849574</td>
      <td>0.799802</td>
      <td>0.092676</td>
      <td>-0.499931</td>
    </tr>
    <tr>
      <th>Family_Size</th>
      <td>-0.286638</td>
      <td>0.583250</td>
      <td>0.594481</td>
      <td>0.014717</td>
      <td>-0.296702</td>
      <td>-0.341414</td>
      <td>-0.429948</td>
      <td>-0.363522</td>
      <td>-0.330705</td>
      <td>-0.235826</td>
      <td>...</td>
      <td>-0.185711</td>
      <td>-0.059505</td>
      <td>0.027081</td>
      <td>-0.218383</td>
      <td>-0.027803</td>
      <td>0.849574</td>
      <td>1.000000</td>
      <td>0.692370</td>
      <td>0.078593</td>
      <td>-0.424497</td>
    </tr>
    <tr>
      <th>Is_Parent</th>
      <td>-0.403132</td>
      <td>0.520355</td>
      <td>0.587993</td>
      <td>0.002189</td>
      <td>-0.341994</td>
      <td>-0.410657</td>
      <td>-0.574147</td>
      <td>-0.449596</td>
      <td>-0.402064</td>
      <td>-0.245380</td>
      <td>...</td>
      <td>-0.279387</td>
      <td>-0.081575</td>
      <td>0.018124</td>
      <td>-0.203885</td>
      <td>-0.005195</td>
      <td>0.799802</td>
      <td>0.692370</td>
      <td>1.000000</td>
      <td>-0.011841</td>
      <td>-0.521603</td>
    </tr>
    <tr>
      <th>Age</th>
      <td>0.199977</td>
      <td>-0.237497</td>
      <td>0.361932</td>
      <td>0.015694</td>
      <td>0.164615</td>
      <td>0.013447</td>
      <td>0.033622</td>
      <td>0.041154</td>
      <td>0.021516</td>
      <td>0.059779</td>
      <td>...</td>
      <td>0.011941</td>
      <td>0.007821</td>
      <td>0.004602</td>
      <td>-0.020937</td>
      <td>-0.021057</td>
      <td>0.092676</td>
      <td>0.078593</td>
      <td>-0.011841</td>
      <td>1.000000</td>
      <td>0.115901</td>
    </tr>
    <tr>
      <th>Spent</th>
      <td>0.792740</td>
      <td>-0.557949</td>
      <td>-0.137964</td>
      <td>0.020479</td>
      <td>0.892996</td>
      <td>0.612129</td>
      <td>0.845543</td>
      <td>0.641884</td>
      <td>0.606652</td>
      <td>0.527101</td>
      <td>...</td>
      <td>0.381354</td>
      <td>0.136336</td>
      <td>-0.034135</td>
      <td>0.264443</td>
      <td>0.138590</td>
      <td>-0.499931</td>
      <td>-0.424497</td>
      <td>-0.521603</td>
      <td>0.115901</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>

```py
# correlation matrix 히트맵으로 표현하기
corrmat= df.corr()
plt.figure(figsize=(20,20))  
sns.heatmap(corrmat, annot=True, cmap="Spectral", center=0)
```

![](https://velog.velcdn.com/images/yy2hi/post/12c70d73-8a50-4a3a-b825-669716d848b9/image.png)


## 👣 데이터 전처리하기: Preprocessing
* 카테고리 형 데이터를 숫자형으로 바꾸기
* 피쳐 간 규모를 맞추기 위해 Scaling 하기
* 모델에 넣을 피쳐 - 차원 축소하기

카테고리 데이터 전처리하기

```py
df.dtypes

=>

Education               object
Income                 float64
Kidhome                  int64
Teenhome                 int64
Recency                  int64
Wines                    int64
Fruits                   int64
Meat                     int64
Fish                     int64
Sweets                   int64
Gold                     int64
NumDealsPurchases        int64
NumWebPurchases          int64
NumCatalogPurchases      int64
NumStorePurchases        int64
NumWebVisitsMonth        int64
AcceptedCmp3             int64
AcceptedCmp4             int64
AcceptedCmp5             int64
AcceptedCmp1             int64
AcceptedCmp2             int64
Complain                 int64
Response                 int64
Customer_For             int64
Living_With             object
Children                 int64
Family_Size              int64
Is_Parent                int64
Age                      int64
Spent                    int64
dtype: object
```
```py
# 데이터 중에서 object 의 데이터 형태를 가지고 있는 컬럼을 가져와라
s = (df.dtypes == 'object')
s[s.values == True].index
s[s].index 

=>

Index(['Education', 'Living_With'], dtype='object')
```

```py
# 카테고리 형태의 데이터
s = (df.dtypes == 'object')
object_cols = list(s[s].index)

print("카테고리 데이터 종류:", object_cols)

=>

카테고리 데이터 종류: ['Education', 'Living_With']
```

```py
# 카테고리 데이터에 라벨 인코더 사용하기
# 라벨 인코더란?
   # 카테고리형 데이터에 숫자를 매핑하여 바꿔준다. 
from sklearn.preprocessing import LabelEncoder

# Education 바꿔주기
LE=LabelEncoder()
df['Education']=df[['Education']].apply(LE.fit_transform)
```

```py
df['Education'].head()

=>

0    0
1    0
2    0
3    0
4    1
Name: Education, dtype: int64
```

```py
print(LE.classes_)
print(LE.inverse_transform([1, 2, 2]))

=>

['Graduate' 'Postgraduate' 'Undergraduate']
['Postgraduate' 'Undergraduate' 'Undergraduate']
```

```py
# Living_With 카테고리 -> 숫자로 바꿔주기
df['Living_With']=df[['Living_With']].apply(LE.fit_transform)
df['Living_With'].head()

=>

0    0
1    0
2    1
3    1
4    1
Name: Living_With, dtype: int64
```

피쳐 간 규모를 맞추기 위해 Scaling 하기

![](https://velog.velcdn.com/images/yy2hi/post/746d9c21-7a80-46ff-bfb7-5c101b7a4bf5/image.png)

```py
from sklearn.preprocessing import StandardScaler

ds = df.copy()

# 나중에 클러스터 별 캠페인 반응률을 살펴보기 위해 사용
# 클러스터를 구성하기 위한 피쳐에서는 제거한다.
cols_del = ['AcceptedCmp3', 'AcceptedCmp4', 'AcceptedCmp5', 'AcceptedCmp1','AcceptedCmp2', 'Complain', 'Response']
ds = ds.drop(cols_del, axis=1)
```

```py
ds.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Education</th>
      <th>Income</th>
      <th>Kidhome</th>
      <th>Teenhome</th>
      <th>Recency</th>
      <th>Wines</th>
      <th>Fruits</th>
      <th>Meat</th>
      <th>Fish</th>
      <th>Sweets</th>
      <th>...</th>
      <th>NumCatalogPurchases</th>
      <th>NumStorePurchases</th>
      <th>NumWebVisitsMonth</th>
      <th>Customer_For</th>
      <th>Living_With</th>
      <th>Children</th>
      <th>Family_Size</th>
      <th>Is_Parent</th>
      <th>Age</th>
      <th>Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>58138.0</td>
      <td>0</td>
      <td>0</td>
      <td>58</td>
      <td>635</td>
      <td>88</td>
      <td>546</td>
      <td>172</td>
      <td>88</td>
      <td>...</td>
      <td>10</td>
      <td>4</td>
      <td>7</td>
      <td>83894400000000000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>64</td>
      <td>1617</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>46344.0</td>
      <td>1</td>
      <td>1</td>
      <td>38</td>
      <td>11</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>10800000000000000</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>67</td>
      <td>27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>71613.0</td>
      <td>0</td>
      <td>0</td>
      <td>26</td>
      <td>426</td>
      <td>49</td>
      <td>127</td>
      <td>111</td>
      <td>21</td>
      <td>...</td>
      <td>2</td>
      <td>10</td>
      <td>4</td>
      <td>40780800000000000</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>56</td>
      <td>776</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>26646.0</td>
      <td>1</td>
      <td>0</td>
      <td>26</td>
      <td>11</td>
      <td>4</td>
      <td>20</td>
      <td>10</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>4</td>
      <td>6</td>
      <td>5616000000000000</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>37</td>
      <td>53</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>58293.0</td>
      <td>1</td>
      <td>0</td>
      <td>94</td>
      <td>173</td>
      <td>43</td>
      <td>118</td>
      <td>46</td>
      <td>27</td>
      <td>...</td>
      <td>3</td>
      <td>6</td>
      <td>5</td>
      <td>27734400000000000</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>40</td>
      <td>422</td>
    </tr>
  </tbody>
</table>

```py
#Scaling
scaler = StandardScaler()
scaler.fit(ds) # mean, variance 계산

# 적용
scaled_ds = pd.DataFrame(scaler.transform(ds), columns= ds.columns )
print("All features are now scaled")

=>

All features are now scaled
```

```py
#Scaled data to be used for reducing the dimensionality
print("Dataframe to be used for further modelling:")
scaled_ds.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Education</th>
      <th>Income</th>
      <th>Kidhome</th>
      <th>Teenhome</th>
      <th>Recency</th>
      <th>Wines</th>
      <th>Fruits</th>
      <th>Meat</th>
      <th>Fish</th>
      <th>Sweets</th>
      <th>...</th>
      <th>NumCatalogPurchases</th>
      <th>NumStorePurchases</th>
      <th>NumWebVisitsMonth</th>
      <th>Customer_For</th>
      <th>Living_With</th>
      <th>Children</th>
      <th>Family_Size</th>
      <th>Is_Parent</th>
      <th>Age</th>
      <th>Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.893586</td>
      <td>0.287105</td>
      <td>-0.822754</td>
      <td>-0.929699</td>
      <td>0.310353</td>
      <td>0.977660</td>
      <td>1.552041</td>
      <td>1.690293</td>
      <td>2.453472</td>
      <td>1.483713</td>
      <td>...</td>
      <td>2.503607</td>
      <td>-0.555814</td>
      <td>0.692181</td>
      <td>1.973583</td>
      <td>-1.349603</td>
      <td>-1.264598</td>
      <td>-1.758359</td>
      <td>-1.581139</td>
      <td>1.018352</td>
      <td>1.676245</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.893586</td>
      <td>-0.260882</td>
      <td>1.040021</td>
      <td>0.908097</td>
      <td>-0.380813</td>
      <td>-0.872618</td>
      <td>-0.637461</td>
      <td>-0.718230</td>
      <td>-0.651004</td>
      <td>-0.634019</td>
      <td>...</td>
      <td>-0.571340</td>
      <td>-1.171160</td>
      <td>-0.132545</td>
      <td>-1.665144</td>
      <td>-1.349603</td>
      <td>1.404572</td>
      <td>0.449070</td>
      <td>0.632456</td>
      <td>1.274785</td>
      <td>-0.963297</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.893586</td>
      <td>0.913196</td>
      <td>-0.822754</td>
      <td>-0.929699</td>
      <td>-0.795514</td>
      <td>0.357935</td>
      <td>0.570540</td>
      <td>-0.178542</td>
      <td>1.339513</td>
      <td>-0.147184</td>
      <td>...</td>
      <td>-0.229679</td>
      <td>1.290224</td>
      <td>-0.544908</td>
      <td>-0.172664</td>
      <td>0.740959</td>
      <td>-1.264598</td>
      <td>-0.654644</td>
      <td>-1.581139</td>
      <td>0.334530</td>
      <td>0.280110</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.893586</td>
      <td>-1.176114</td>
      <td>1.040021</td>
      <td>-0.929699</td>
      <td>-0.795514</td>
      <td>-0.872618</td>
      <td>-0.561961</td>
      <td>-0.655787</td>
      <td>-0.504911</td>
      <td>-0.585335</td>
      <td>...</td>
      <td>-0.913000</td>
      <td>-0.555814</td>
      <td>0.279818</td>
      <td>-1.923210</td>
      <td>0.740959</td>
      <td>0.069987</td>
      <td>0.449070</td>
      <td>0.632456</td>
      <td>-1.289547</td>
      <td>-0.920135</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.571657</td>
      <td>0.294307</td>
      <td>1.040021</td>
      <td>-0.929699</td>
      <td>1.554453</td>
      <td>-0.392257</td>
      <td>0.419540</td>
      <td>-0.218684</td>
      <td>0.152508</td>
      <td>-0.001133</td>
      <td>...</td>
      <td>0.111982</td>
      <td>0.059532</td>
      <td>-0.132545</td>
      <td>-0.822130</td>
      <td>0.740959</td>
      <td>0.069987</td>
      <td>0.449070</td>
      <td>0.632456</td>
      <td>-1.033114</td>
      <td>-0.307562</td>
    </tr>
  </tbody>
</table>

- 여기까지 카테고리 데이터를 LabelEncoder 를 통해 숫자형으로 변환
- Scaler 를 사용하여 데이터를 정규화

### 💡 Dimension Reduction

- 앞의 상관계수 행렬에서 봤듯이, 서로 관련있는 피쳐들이 많음.
- 변수가 너무 많으면, 모델링이 적절하게 되지 않을 수 있음. 

-> 우리 데이터에 상관성이 많고, 피쳐가 너무 많은데 클러스터를 수행하려다 보니
PCA(차원 축소 기법)을 사용해서 차원을 축소 시켜 보자.

```py
# PCA 로 3차원으로 데이터를 줄인다.
# sklearn 에서 PCA 모듈을 불러온다.
from sklearn.decomposition import PCA

pca = PCA(n_components=3)
pca.fit(scaled_ds)
PCA_ds = pd.DataFrame(pca.transform(scaled_ds), columns=(["col1","col2", "col3"]))

PCA_ds.describe().T
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>col1</th>
      <td>2212.0</td>
      <td>-2.569775e-17</td>
      <td>2.878377</td>
      <td>-5.969394</td>
      <td>-2.538494</td>
      <td>-0.780421</td>
      <td>2.383290</td>
      <td>7.444305</td>
    </tr>
    <tr>
      <th>col2</th>
      <td>2212.0</td>
      <td>-8.994212e-17</td>
      <td>1.706839</td>
      <td>-4.312106</td>
      <td>-1.328321</td>
      <td>-0.158197</td>
      <td>1.242240</td>
      <td>6.142664</td>
    </tr>
    <tr>
      <th>col3</th>
      <td>2212.0</td>
      <td>7.388103e-17</td>
      <td>1.221956</td>
      <td>-3.530975</td>
      <td>-0.828811</td>
      <td>-0.021838</td>
      <td>0.800092</td>
      <td>6.615874</td>
    </tr>
  </tbody>
</table>

```py
PCA_ds.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>col1</th>
      <th>col2</th>
      <th>col3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4.994347</td>
      <td>-0.151233</td>
      <td>2.647706</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.884455</td>
      <td>-0.006707</td>
      <td>-1.863811</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.617864</td>
      <td>-0.720806</td>
      <td>-0.254408</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-2.676036</td>
      <td>-1.541954</td>
      <td>-0.922951</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.649591</td>
      <td>0.209833</td>
      <td>-0.021460</td>
    </tr>
  </tbody>
</table>

```py
#A 3D Projection Of Data In The Reduced Dimension
x =PCA_ds["col1"]
y =PCA_ds["col2"]
z =PCA_ds["col3"]

#3D 플랏 그리기
# https://www.python-graph-gallery.com/370-3d-scatterplot
fig = plt.figure(figsize=(10,8))
ax = fig.add_subplot(111, projection="3d")
ax.scatter(x,y,z, c="maroon", marker="o" )
ax.set_title("A 3D Projection Of Data In The Reduced Dimension")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/a43a2837-7263-4978-85eb-eb694973027f/image.png)

## 👣 유저 세그먼트 나누기: Clustering

### 💡 Elbow Method 

데이터를 몇 개로 나눠야 할까?

* Elbow Method 

- WSS(군집 내 분산)은 작을 수록 군집의 중심에 많이 모여있는 것이므로 WSS(군집 내 분산)이 작을 수록 좋다. 
- 하지만 클러스터를 늘려서 더 이상 작아지지 않는 한계점이 있다면 더 이상 클러스터 수를 증가시키지 않아도 좋다.

![](https://velog.velcdn.com/images/yy2hi/post/30e2462d-adb0-427a-b402-ca2adf8c4a0e/image.png)

```py
# 이미지 출처: https://heung-bae-lee.github.io/2020/05/30/machine_learning_19/
```

``py
from sklearn.cluster import KMeans
```
```py
# Quick examination of elbow method to find numbers of clusters to make.
print('Elbow Method to determine the number of clusters to be formed:')

distortions = []

# 클러스터 개수 1 ~ 10 까지 늘려보면서
# 클러스터 내 거리합을 저장한다.
K = range(1,10)
for k in K:
    kmeanModel = KMeans(n_clusters=k)
    kmeanModel.fit(PCA_ds)
    distortions.append(kmeanModel.inertia_) # within-cluster sum-of-squares criterion
```
```py
distortions

=>

[28060.968908252147,
 13841.123695080303,
 9625.179269985394,
 7482.577127923258,
 6590.895912178157,
 5885.488242066771,
 5317.942980809662,
 4867.552990155307,
 4545.679730812983]
```

```py
plt.figure(figsize=(8,6))
plt.plot(K, distortions)
plt.xlabel('k')
plt.ylabel('Distortion')
plt.title('The Elbow Method showing the optimal k')
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/22ad5527-0c66-49b2-9ec8-c982855efbd6/image.png)

클러스터링을 해보자! 
- 여기서는 Hierarchical Clustering 을 먼저 적용해 보았다.

```py
from sklearn.cluster import AgglomerativeClustering


# 유사도 기준: affinity = euclidean
AC = AgglomerativeClustering(n_clusters=4)

# fit model and predict clusters
yhat_AC = AC.fit_predict(PCA_ds)
PCA_ds["Clusters"] = yhat_AC

# 기존 데이터에도 넣는다.
df["Clusters"]= yhat_AC
```

```py
# Plotting the clusters
fig = plt.figure(figsize=(10,8))
ax = plt.subplot(111, projection='3d', label="bla")
ax.scatter(x, y, z, s=40, c=PCA_ds["Clusters"], marker='o', cmap = cmap )
ax.set_title("The Plot Of The Clusters")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/2940d356-4b2d-415c-9a1d-aeaa6a9edffe/image.png)

## 👣 모델 평가하기: Evaluation
EDA 를 통해 만들어진 클러스터와 그 특성 파악하기
그룹이 골고루 분포되어 있을까?

```py
# Plotting countplot of clusters
pal = ["#682F2F","#B9C0C9", "#9F8A78","#F3AB60"]
pl = sns.countplot(x=df["Clusters"], palette=pal)
pl.set_title("Distribution Of The Clusters")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/1fbaf96a-d867-4c45-a0da-79d6db48c8de/image.png)

클러스터 별 구매금액과 연간 수입을 알아보자

```py
pl = sns.scatterplot(data = df,x=df["Spent"], y=df["Income"],hue=df["Clusters"], palette= pal)
pl.set_title("Cluster's Profile Based On Income And Spending")
plt.legend()
plt.show()


# group 3: high spending & low income (3)
# group 2: low spending & low income (2)
# group 0: high spending & average income (0)
# group 1: high spending & high income (1)
```

![](https://velog.velcdn.com/images/yy2hi/post/e6313143-e7bc-40f0-b413-5471efefeb8e/image.png)

구매를 많이하는 고객층은?
```py
plt.figure()
pl=sns.swarmplot(x=df["Clusters"], y=df["Spent"], color= "#CBEDDD", alpha=0.5 )
pl=sns.boxenplot(x=df["Clusters"], y=df["Spent"], palette=pal)
```

![](https://velog.velcdn.com/images/yy2hi/post/d9bd5c10-9927-47e9-8111-4e90969f925a/image.png)

어떤 클러스터에서 캠페인 반응이 높을까?

```py
# Creating a feature to get a sum of accepted promotions 
df["Total_Promos"] = df["AcceptedCmp1"]+ df["AcceptedCmp2"]+ df["AcceptedCmp3"]+ df["AcceptedCmp4"]+ df["AcceptedCmp5"]

# Plotting count of total campaign accepted.
plt.figure()
pl = sns.countplot(x=df["Total_Promos"],hue=df["Clusters"], palette= pal)
pl.set_title("Count Of Promotion Accepted")
pl.set_xlabel("Number Of Total Accepted Promotions")
plt.show()

# 대부분 반응을 하지 않았다. 프로모션이 더 잘 디자인 될 필요가 있다.
```

![](https://velog.velcdn.com/images/yy2hi/post/807370d2-5fc6-4fee-9aa2-4aac32154ae5/image.png)

할인에 잘 반응한 그룹은?

```py
# Plotting the number of deals purchased
plt.figure()
pl=sns.boxenplot(y=df["NumDealsPurchases"],x=df["Clusters"], palette= pal)
pl.set_title("Number of Deals Purchased")
plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/1448db97-0710-4a4b-a76c-0d0587e8cf77/image.png)

가족 구성원과 나이대는 어떻게 될까?

```py
Personal = [ "Income","Kidhome","Teenhome","Customer_For", "Age", "Children", "Family_Size", "Is_Parent", "Education","Living_With"]

for i in Personal:
    plt.figure()
    sns.boxplot(y=df[i], x=df["Clusters"], palette=pal)
    plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/eb396c81-c395-40da-8fab-9be950783d97/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/e248bc4d-ebcd-4833-b510-1a46e86008da/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/b2974746-9ba0-4dd4-b62c-7b9e05f50f54/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/ddbe0cf4-4697-4b77-a7eb-05128016b7bf/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/8dbd3dec-8f37-4928-a064-7d3b72bc6b49/image.png)

- 부모이며, 가족 구성원이 2 -4, 한부모 가정, 10대, 비교적 나이가 있음
- 부모가 아니며, 많아도 2명정도의 가족 구성원, 전 연령에 골고루 있음, 연간 수입이 높음
- 대부분 부모이며, 최대 4명의 가족 구성원, 10대가 아닌 1명의 어린 아이를 두고 있음, 상대적으로 어림
- 부모이며, 최대 5명까지의 가족 구성원, 상대적으로 나이가 있고, 수입이 낮음

## 👣 다양한 클러스터링 모델 사용해보기

### 💡Density Based Clustering
```py
from sklearn.cluster import DBSCAN
# Initiating the DBSCAN Clustering model 
DP = DBSCAN(eps=0.30, min_samples=9)

# fit model and predict clusters
DP_df = DP.fit_predict(PCA_ds)
PCA_ds["DBSCAN_Clusters"] = DP_df

# Adding the Clusters feature to the orignal dataframe.
df["DBSCAN_Clusters"]= DP_df
```
```py
# Plotting the clusters
fig = plt.figure(figsize=(10,8))
ax = plt.subplot(111, projection='3d', label="bla")
ax.scatter(x, y, z, s=40, c=PCA_ds["DBSCAN_Clusters"], marker='o', cmap = 'viridis' )
ax.set_title("The Plot Of The Clusters")
plt.show()
```
![](https://velog.velcdn.com/images/yy2hi/post/67480ade-520a-4bbc-aa35-29992ed93446/image.png)

```py
# Plotting countplot of clusters
pl = sns.countplot(x=df["DBSCAN_Clusters"])
pl.set_title("Distribution Of The Clusters")
plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/5877bb2c-5e39-4d9a-be4f-f7116d561834/image.png)

```py
pl = sns.scatterplot(data = df,x=df["Spent"], y=df["Income"],hue=df["DBSCAN_Clusters"])
pl.set_title("Cluster's Profile Based On Income And Spending")
plt.legend()
plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/5c8176e0-1e33-4017-80ac-a935623cea1b/image.png)

```py
plt.figure()
pl=sns.swarmplot(x=df["DBSCAN_Clusters"], y=df["Spent"], color= "#CBEDDD", alpha=0.5 )
pl=sns.boxenplot(x=df["DBSCAN_Clusters"], y=df["Spent"])
plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/6169ee79-4ca3-48aa-bc6e-1aadfac421e6/image.png)


### 💡Partition Based Clustering
```py
Kmeans = KMeans(n_clusters=4)
# fit model and predict clusters
Kmeans_df = Kmeans.fit_predict(PCA_ds)
PCA_ds["Kmeans_Clusters"] = Kmeans_df
#Adding the Clusters feature to the orignal dataframe.
df["Kmeans_Clusters"]= Kmeans_df
```
```py
#Plotting countplot of clusters
pl = sns.countplot(x=df["Kmeans_Clusters"])
pl.set_title("Distribution Of The Clusters")
plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/0d4eb129-f2f9-4420-9953-28d660610a0d/image.png)

```py
pl = sns.scatterplot(data = df,x=df["Spent"], y=df["Income"],hue=df["Kmeans_Clusters"])
pl.set_title("Cluster's Profile Based On Income And Spending")
plt.legend()
plt.show()
```

![](https://velog.velcdn.com/images/yy2hi/post/b2937793-0e24-4b6f-b2ba-f0dc57f68c01/image.png)

### 💡 Silhouette Coefficient
```py
from sklearn.metrics import silhouette_samples

# Hierachical
sample_silhouette_values = silhouette_samples(PCA_ds, PCA_ds["Clusters"])
print("Hierachical",sample_silhouette_values.mean())

# Density - DBSCAN
sample_silhouette_values = silhouette_samples(PCA_ds, PCA_ds["DBSCAN_Clusters"])
print("Density",sample_silhouette_values.mean())


# Kmeans
sample_silhouette_values = silhouette_samples(PCA_ds, PCA_ds["Kmeans_Clusters"])
print("Kmeans",sample_silhouette_values.mean())

=>

Hierachical 0.45273806967143565
Density 0.05700586237988258
Kmeans 0.46554372154673307
```
