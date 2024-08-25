---
layout: single
title: "Project 1 - 서울시 CCTV 현황 데이터 분석 (1)"
categories: DataAnalysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# Project 01. Analysis Seoul CCTV
## 프로젝트 개요
![](https://velog.velcdn.com/images/yy2hi/post/3616de05-5ac7-451e-9a08-f4c2e3348e23/image.png)

## 목표
- 서울시 구별 CCTV 현황 데이터 확보
- 인구 현황 데이터 확보
- CCTV 데이터와 인구 현황 데이터 합치기
- 데이터 정리 및 정렬
- 그래프로 시각화
- 전체적인 경향 파악
- 경향에서 벗어난 데이터 강조

## 데이터 읽기
### Pandas로 CSV, 엑셀 파일 읽기
![](https://velog.velcdn.com/images/yy2hi/post/f10200a1-6e1b-4d5d-834f-22f415707635/image.png)
- R만큼의 강력한 데이터 핸들링 성능을 제공하는 모듈
- 단일 프로세스에서는 최대 효율
- 코딩 가능하고 응용 가능한 엑셀
![](https://velog.velcdn.com/images/yy2hi/post/c8721df6-ea98-4139-8916-d2158d5ce7b7/image.png)

---
#### Pandas DataFrame 구조
![](https://velog.velcdn.com/images/yy2hi/post/8185d8ec-628b-46f6-95a5-3cad8aeb67c8/image.png)

---
#### column 이름으로 조회
![](https://velog.velcdn.com/images/yy2hi/post/4ad15318-8818-4093-9293-aafd98101344/image.png)

---
#### 서울 CCTV 수 column 이름 변경
![](https://velog.velcdn.com/images/yy2hi/post/83a5445d-37c0-4a38-a55d-756f4f67a035/image.png)

---
#### 엑셀 설정
![](https://velog.velcdn.com/images/yy2hi/post/79c3153e-b4c3-4ed5-b329-d34b3a24dfe0/image.png)

- 읽기 시작할 행(header)과 컬럼 지정(usecols)

---
#### 서울시 인구수 column 이름 변경
![](https://velog.velcdn.com/images/yy2hi/post/243801e9-744e-4570-8e23-b7576630b7ca/image.png)

---

### Pandas Basic
![](https://velog.velcdn.com/images/yy2hi/post/8351c5d4-6d37-4154-b7c8-71897b98cc7a/image.png)

- pandas는 통상 pd로 import
- 수치해석적 함수가 많은 numpy는 통상 np로 import

---

#### Pandas의 데이터형을 구성하는 기본 Series
![](https://velog.velcdn.com/images/yy2hi/post/39f202ea-8360-4680-bd45-0130ee6a2cd8/image.png)

---

#### 날짜(시간) 이용
![](https://velog.velcdn.com/images/yy2hi/post/14872710-6556-44f4-ab39-30f4a6b68645/image.png)

---

#### 가장 많이 사용되는 데이터형 DataFrame
![](https://velog.velcdn.com/images/yy2hi/post/c7d07f01-96d7-4336-803d-7a2e12751bf3/image.png)
- index와 columns를 지정

---

#### DataFrame의 기본 정보 확인
![](https://velog.velcdn.com/images/yy2hi/post/1a33680a-df3d-436f-9a9e-b31c86f21f95/image.png)

- 각 컬럼의 크기와 데이터형태 확인

---

#### DataFrame의 통계적 기본 정보 확인
![](https://velog.velcdn.com/images/yy2hi/post/84534c44-4631-4e72-9803-753f741c8d1e/image.png)

---

#### 데이터 정렬
![](https://velog.velcdn.com/images/yy2hi/post/329c4195-8745-4b44-ad99-e50eb6615258/image.png)

---

#### 특정 컬럼 읽기
![](https://velog.velcdn.com/images/yy2hi/post/5e3d0987-4af2-457c-ba72-f15287542b76/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/c358c8bd-3c16-41fe-94c4-7b5f45ff7414/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/e40b9542-1624-44f1-9f5c-dd6db2769252/image.png)

- iloc 옵션을 이용해 번호로만 접근

---

#### Pandas Slice under condition
![](https://velog.velcdn.com/images/yy2hi/post/df08cf37-d0dd-4ee9-abc3-ba1f9e45892b/image.png)

- df[condition]과 같이 사용하는 것이 일반적
- 버전에 따라 문법이 다르므로, 인터넷에서 확보한 소스코드는 Pandas의 버전 확인이 필요
![](https://velog.velcdn.com/images/yy2hi/post/431f875a-b6ac-492f-a1b3-e69b1caf2a66/image.png)

---

#### 특정 요소 확인
![](https://velog.velcdn.com/images/yy2hi/post/46960809-3791-4e1b-a0bd-11981b273838/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/5cf2ec12-95b8-4be1-8e0a-fca73b1bbbb5/image.png)

---
#### 특정 칼럼 제거
![](https://velog.velcdn.com/images/yy2hi/post/9e306eee-3443-479f-b4ce-cd9477931f6c/image.png)

---

#### apply 메소드
![](https://velog.velcdn.com/images/yy2hi/post/0535d600-3a60-4435-8b07-4fd41e37fe6a/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/9312a98e-6561-49e6-abdf-fd14693e347c/image.png)

- 함수를 만들어서 적용하거나 람다 함수 적용 가능

---

## CCTV 데이터 훑어보기

#### CCTV를 가장 적게 보유한 구
![](https://velog.velcdn.com/images/yy2hi/post/2052b47d-618a-4deb-8002-4bef4790a3c6/image.png)

---


#### CCTV를 가장 많이 보유한 구
![](https://velog.velcdn.com/images/yy2hi/post/1bc731fd-7f19-457a-8603-72a6a29c5698/image.png)

---

#### 전에 보유한 갯수 대비 최근 3년간 CCTV를 많이 설치한 구
![](https://velog.velcdn.com/images/yy2hi/post/f10d2955-ca3e-43a8-8a01-344e11131fbb/image.png)

---

## 인구현황 데이터 훑어보기
#### 서울시 인구 데이터 확인
![](https://velog.velcdn.com/images/yy2hi/post/41dd0ea2-dd89-4e49-b592-677862669519/image.png)
![](https://velog.velcdn.com/images/yy2hi/post/3ede9e67-94e6-47e9-9ddf-a6ea7e47009a/image.png)

---

#### 데이터 초반 검증
![](https://velog.velcdn.com/images/yy2hi/post/34cedfd9-4f10-4962-8d68-a14d3c96ec9c/image.png)

---

#### 외국인, 고령자 비율 만들기
![](https://velog.velcdn.com/images/yy2hi/post/893459fb-756c-4e96-a577-e5f2fbdfaa0e/image.png)

---

#### 인구수가 많은 구 확인
![](https://velog.velcdn.com/images/yy2hi/post/c421f38f-3709-4e03-8da7-929148e679f2/image.png)

---

#### 고령자비율 확인
![](https://velog.velcdn.com/images/yy2hi/post/a2539557-7100-4566-abba-0ba5e0cb598f/image.png)

---

**출처**
서울시 자치구 년도별 CCTV 설치 현황, https://data.seoul.go.kr/dataList/OA-2734/F/1/datasetView.do
서울시 주민등록인구 통계, https://data.seoul.go.kr/dataList/419/S/2/datasetView.do