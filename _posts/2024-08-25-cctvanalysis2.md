---
layout: single
title: "Project 1 - 서울시 CCTV 현황 데이터 분석 (2)"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---


## 두 데이터 합치기
#### merge를 이용한 데이터 병합
![](https://velog.velcdn.com/images/yy2hi/post/b30a15a4-ff91-4b97-a199-6210dc0e39b7/image.png)

---

![](https://velog.velcdn.com/images/yy2hi/post/9ea50617-6ff8-42a5-8235-d4dfdf55b339/image.png)

- key 컬럼을 기준으로 병합

---

![](https://velog.velcdn.com/images/yy2hi/post/299ff756-e8d1-45b1-b92a-42bb08aa9d1b/image.png)

- left에 키를 기준으로 right 병합

---

![](https://velog.velcdn.com/images/yy2hi/post/735f49a0-cbef-486c-8444-8ad4b4056528/image.png)

- key를 기준으로 합집합 병합

---

![](https://velog.velcdn.com/images/yy2hi/post/cc204afd-a2ee-4372-9b85-964b0ddd3782/image.png)

- key 컬럼에서 교집합 병합

---

### 데이터 병합 및 정리
#### 데이터 병합
![](https://velog.velcdn.com/images/yy2hi/post/bc3f05ca-ec8f-4802-b21b-a3455491c82e/image.png)

---

#### 필요 없는 컬럼 제거
![](https://velog.velcdn.com/images/yy2hi/post/5d14f999-122d-44c7-8023-b11dc20c2260/image.png)

---

#### 인덱스 재지정
![](https://velog.velcdn.com/images/yy2hi/post/1115173e-068e-4c99-8e35-6b3621e2a203/image.png)

- 재지정 명령어 : set_index

---

### 상관관계(Correlation)
![](https://velog.velcdn.com/images/yy2hi/post/ee217a35-7988-41a0-8f73-54dd4ffef1ef/image.png)

---

#### corr()
![](https://velog.velcdn.com/images/yy2hi/post/bac442dc-624b-4c2f-ab15-c45a396e1027/image.png)

- 데이터의 관계를 찾을 때, 최소한의 근거가 있어야 해당 데이터를 비교하는 의미가 존재
- 상관계수를 조사해서 0.2 이상의 데이터를 비교하는 것은 유의미

![](https://velog.velcdn.com/images/yy2hi/post/e300d49d-53e3-42d0-a3c7-d2f0e22d89af/image.png)

- CCTV 전체 수(소계)와 가장 상관관계가 있는 데이터 → 인구수
- ∴ 구별 인구대비 CCTV 현황을 분석해서 상대적으로 CCTV가 적거나 많은 구를 찾는 것이 의미를 가짐

---

#### CCTV 비율
![](https://velog.velcdn.com/images/yy2hi/post/8a810798-48f0-441d-b3ea-15c40300a3b4/image.png)

- 인구대비 CCTV 비율이 높은 구

---

![](https://velog.velcdn.com/images/yy2hi/post/0afe0648-c2e4-4785-97b8-64b3dcd1bf8d/image.png)

- 인구대비 CCTV 비율이 낮은 구

---

## Matplotlib
- 파이썬 대표 시각화 도구
- Jupyter Notebook의 경우 matplotlib의 결과가 out session에 나타나는 것이 유리하므로 %matplotlib inline 옵션 사용

---

### matplotlib 호출
![](https://velog.velcdn.com/images/yy2hi/post/c55b31c2-2b4e-4195-b05a-1adb5307c879/image.png)

---

#### 삼각함수 그리기
![](https://velog.velcdn.com/images/yy2hi/post/5a67317a-340a-4425-8caf-5be2d5c0a3fa/image.png)

![](https://velog.velcdn.com/images/yy2hi/post/34c02836-0819-4523-9acd-a66868fa544f/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/62efa723-a009-49a6-a3df-3a21e78d923c/image.png)

---

#### scatter()
![](https://velog.velcdn.com/images/yy2hi/post/ca39868b-146a-44dc-a2cd-311c68d0c035/image.png)

![](https://velog.velcdn.com/images/yy2hi/post/1c5de3c8-04fd-4843-ac3d-7d84d1bdf8c0/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/ec2852c9-fc93-4f83-bd98-258ef1436b04/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/093c2643-eb86-4a36-a230-d68630fa9a70/image.png)

---

## 데이터 시각화
![](https://velog.velcdn.com/images/yy2hi/post/bb3ec83a-33cb-4149-841c-9ac3914a886d/image.png)
- 한글 폰트 적용 및 마이너스 기호 적용 (window: "malgun gothic")

---

![](https://velog.velcdn.com/images/yy2hi/post/93085a1d-7b7a-4d9d-bc4c-d14aeb9cc4fd/image.png)
- Pandas DataFrame은 데이터 변수에서 plot() 사용 가능
- 데이터(컬럼)가 많은 경우 정렬한 후 그리는 것이 효과적

![](https://velog.velcdn.com/images/yy2hi/post/d1476506-9636-45ff-abbb-d5808e551d5c/image.png)

---

![](https://velog.velcdn.com/images/yy2hi/post/24242bbe-5dc3-4491-af17-8458b25c29fd/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/c4e40d31-0ed5-4921-a32c-d2e1fc110a0a/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/bcb772ff-10b3-4d21-a59e-b4e3a2db8d2f/image.png)

---

## 경향 파악
- 단순 CCTV 많은 구 : 강남, 양천, 서초, 관악, 은평, 용산
- CCTV 비율 높은 구 : 종로, 용산, 중구
- 전체 경향과 함께 보지 않으면 제대로 이해시키기 어려움
![](https://velog.velcdn.com/images/yy2hi/post/9d568179-b03c-42be-9ef9-d3443bc91cdc/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/f6df4a4c-402d-4410-8079-20c49a613981/image.png)

---

### 선형회귀(Linear Regression) Trend 파악
#### Numpy를 이용한 1차 직선 만들기
- np.polyfit : 직선을 구성하기 위한 계수 계산
- np.poly1d : polyfit으로 찾은 계수로 python에서 사용할 함수로 만들어 줌
![](https://velog.velcdn.com/images/yy2hi/post/c32872cf-4c93-4e75-947c-4f97edd20f2d/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/50b44dbf-b028-4a62-86d3-c110900a1cf7/image.png)
- plyfit에서 찾은 계수를 넣어 함수 완성

---


#### 인구 400000인 구에서 서울시의 전체 경향에 맞는 적당한 CCTV 수?
![](https://velog.velcdn.com/images/yy2hi/post/e1e6cb25-c357-4e8c-99c0-efb063dd7c02/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/692fbd2b-2af5-4315-a74b-324fd7231125/image.png)
- 경향선을 그리기 위해 X 데이터 생성
- np.linspace(a, b ,n) : a부터 b까지 n개의 등간격 데이터 생성
![](https://velog.velcdn.com/images/yy2hi/post/74f0e43e-7c93-47be-9277-e7aa568f9d90/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/1a3c54e2-7ede-4185-a02f-4e73b9af3c62/image.png)

---

## 경향에서 벗어난 데이터 강조
### 그래프 다듬기
data_result['오차'] = data_result['소계']-f1(data_result['인구수])

- 경향(trend)과의 오차 만들기
- 경향은 f1 함수에 해당 인구를 입력 : f1(data_result['인구수'])
- 현재값 : data_result['소계']
![](https://velog.velcdn.com/images/yy2hi/post/3405a69d-2f14-468f-ab1d-f878de3ae5c3/image.png)

---

#### 경향 대비 CCTV를 많이 가진 구
![](https://velog.velcdn.com/images/yy2hi/post/8659b758-69c4-49c1-a728-2e92e47dee76/image.png)

---

#### 경향 대비 CCTV를 적게 가진 구
![](https://velog.velcdn.com/images/yy2hi/post/c8254774-a61b-4545-8ec1-1a9502e6d472/image.png)

---

#### 강조하고 싶은 데이터 시각화

![](https://velog.velcdn.com/images/yy2hi/post/897065b2-e9d5-4e2f-9199-470a578bbf57/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/5ed53a8b-9327-4ca4-9238-4f6376abc8f5/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/4c7ba5a7-2782-40bd-9c3d-0ddf863b1247/image.png)
- s : 마커의 크기
- c : color 세팅에 방금 계산한 경향과의 오차 적용
- cmap : 사용자 정의한 맵 적용


![](https://velog.velcdn.com/images/yy2hi/post/190f8cfc-210f-43e5-8346-771175a3c009/image.png)
- 오차가 큰 데이터 아래 위로 5개만 마커 옆에 구 이름 명시
![](https://velog.velcdn.com/images/yy2hi/post/1d3d8a0e-5f6c-4c9d-8dc8-7c4dd536e5e4/image.png)
- text : 그래프에 글자를 그리는 명령
- plt.text(x, y, text, 설정)
- x, y 데이터에 1.02, 0.98을 곱해 구 이름이 마커에 겹치지 않도록 설정
![](https://velog.velcdn.com/images/yy2hi/post/808494aa-cc94-4d77-9263-25e45852b747/image.png)

---
#### 데이터 저장
![](https://velog.velcdn.com/images/yy2hi/post/ad28c5c3-9bd7-4225-8022-612a091a5cc4/image.png)


---

#### 출처
서울시 자치구 년도별 CCTV 설치 현황, https://data.seoul.go.kr/dataList/OA-2734/F/1/datasetView.do
서울시 주민등록인구 통계, https://data.seoul.go.kr/dataList/419/S/2/datasetView.do