---
layout: single
title: "SQL - Subquery"
categories: SQL
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# SQL Subquery
- 하나의 SQL문 안에 포함되어 있는 또 다른 SQL문을 말한다.
- 메인쿼리가 서브쿼리르 포함하는 종속적인 관계이다.
  
  
### 사용시 주의사항
- Subquery는 괄호로 묶어서 사용
- 단일 행 혹은 복수 행 비교 연산자와 함께 사용 가능
- Subquery에서는 order by 사용 X
- 서브쿼리는 메인쿼리의 컬럼 사용 가능
- 메인쿼리는 서브쿼리의 컬럼 사용 불가

### 종류
- 스칼라 서브쿼리(Scalar Subquery) - SELECT절에 사용
- 인라인 뷰(Inline View) - FROM절에 사용
- 중첩 서브쿼리(Nested Subquery) - WHERE절에 사용


# 🔴 스칼라 서브쿼리 (Scalar Subquery)
- SELECT절에서 사용하는 서브쿼리로 결과는 하나의 Column이어야 한다.
```sql
SELECT column1, (SELECT column2 FROM table2 WHERE condition)
FROM table1
WHERE condition;
```

### 예제
서울은평경찰서의 강도 검거 건수와 서울시 경찰서 전체의 평균 강도 검거 건수를 조회

```
mysql> select case_number, (select avg(case_number) from crime_statuswhere crime_type like '강도' and status_type like '검거') avg
from crime_status 
where police_station like '은평' and crime_type like '강도' and status_type like '검거';
+-------------+--------+
| case_number | avg    |
+-------------+--------+
|           1 | 4.1935 |
+-------------+--------+
```

# 🔴 인라인 뷰 (Inline View)
- FROM절에 사용하는 서브쿼리로 메인쿼리에서는 인라인 뷰에서 조회한 Column만 사용 가능
```sql
SELECT a.column, b.column
FROM table1 a, (SELECT column1, column2 FROM table2) b
WHERE condition;
```

### 예제
경찰서별로 가장 많이 발생한 범죄 건수와 범죄 유형을 조회
```
mysql> select c.police_station, c.crime_type, c.case_number 
    -> from crime_status c, (select police_station, max(case_number) count from crime_status where status_type like '발생' group by police_station) m
    -> where c.police_station = m.police_station and c.case_number = m.count;
+----------------+------------+-------------+
| police_station | crime_type | case_number |
+----------------+------------+-------------+
| 중부           | 폭력       |         997 |
| 종로           | 폭력       |         964 |
| 남대문         | 절도       |         699 |
| 서대문         | 폭력       |        1292 |
| 혜화           | 폭력       |         747 |
| 용산           | 폭력       |        1617 |
| 성북           | 폭력       |         672 |
| 동대문         | 폭력       |        1784 |
| 마포           | 폭력       |        1844 |
| 영등포         | 폭력       |        2701 |
| 성동           | 폭력       |        1223 |
| 동작           | 폭력       |        1631 |
| 광진           | 폭력       |        1676 |
| 서부           | 폭력       |         748 |
| 강북           | 폭력       |        1817 |
| 금천           | 폭력       |        1471 |
| 중랑           | 폭력       |        2022 |
| 강남           | 폭력       |        2283 |
| 관악           | 폭력       |        2614 |
| 강서           | 폭력       |        2445 |
| 강동           | 폭력       |        1942 |
| 종암           | 폭력       |         758 |
| 구로           | 폭력       |        2204 |
| 서초           | 폭력       |        1750 |
| 양천           | 폭력       |        1582 |
| 송파           | 폭력       |        2675 |
| 노원           | 폭력       |        2163 |
| 방배           | 폭력       |         423 |
| 은평           | 폭력       |        1092 |
| 도봉           | 폭력       |        1234 |
| 수서           | 폭력       |        1394 |
+----------------+------------+-------------+
```

# 🔴 중첩 서브쿼리 (Nested Subquery)
WHERE절에서 사용하는 서브쿼리
- Single Row : 하나의 열을 검색하는 서브쿼리
- Multiple Row : 하나 이상의 열을 검색하는 서브쿼리
- Multiple Column : 하나 이상의 행을 검색하는 서브쿼리

## ⭕ Single Row Subquery
- 서브쿼리가 비교연산자(=, >, >=, <, <=, <>, !=)와 사용되는 경우
- 서브쿼리의 검색 결과는 한 개의 결과값을 가져야 한다. (두 개 이상인 경우 에러)
```sql
SELECT column_names
FROM table_name
WHERE column_name = (SELECT column_name
					 FROM table_name
                     WHERE condition)
ORDER BY column_name;                     
```

### 예제
```
mysql> select name from celeb where name = select host from snl_show;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'select host from snl_show' at line 1

mysql> select name from celeb where name = (select host from snl_show);
ERROR 1242 (21000): Subquery returns more than 1 row

mysql> select name from celeb where name = (select host from snl_show where id = 1);
+-----------+
| name      |
+-----------+
| 강동원    |
+-----------+
```

## ⭕ Multiple Row - IN
서브쿼리 결과중에 포함 될 때
```sql
SELCT column_names
FROM table_name
WHERE column_name IN (SELECT column_name
					  FROM table_name
                      WHERE condition)
ORDER BY column_names;
```

### 예제
SNL에 출연한 영화배우를 조회
```
mysql> select host
    -> from snl_show
    -> where host in (select name from celeb where job_title like '%영화배우%');
+-----------+
| host      |
+-----------+
| 강동원    |
| 차승원    |
+-----------+
```

## ⭕ Multiple Row - EXISTS
서브쿼리 결과에 값이 있으면 반환
```sql
SELECT column_names
FROM table_name
WHERE EXISTS (SELECT column_name
			  FROM table_name
              WHERE condition)
ORDER BY column_names;              
```

### 예제
범죄 검거 혹은 발생 건수가 2000건 보다 큰 경찰서 조회
```
mysql> select name
    -> from police_station p
    -> where exists (select police_station from crime_status c where p.name = c.reference and case_number > 2000);
+--------------------------+
| name                     |
+--------------------------+
| 서울강남경찰서           |
| 서울강서경찰서           |
| 서울관악경찰서           |
| 서울구로경찰서           |
| 서울노원경찰서           |
| 서울송파경찰서           |
| 서울영등포경찰서         |
| 서울중랑경찰서           |
+--------------------------+
```

## ⭕ Multiple Row - ANY
서브쿼리 결과중에 최소한 하나라도 만족시(비교연산자 사용)
```sql
SELECT column_names
FROM table_name
WHERE column_name = ANY (SELECT column_name
						 FROM table_name
                         WHERE condition)
ORDER BY column_names;                         
```

### 예제
SNL에 출연한적이 있는 연예인 이름 조회
```
mysql> select name
    -> from celeb
    -> where name = any (select host from snl_show);
+-----------+
| name      |
+-----------+
| 강동원    |
| 유재석    |
| 차승원    |
| 이수현    |
+-----------+
```

## ⭕ Multiple Row - ALL
서브쿼리 결과 모두 만족(비교 연산자 사용)
```sql
SELECT column_names
FROM table_name
WHERE column_name = ALL (SELECT column_name
						 FROM table_name
                         WHERE condition)
ORDER BY column_names;  
```

### 예제
```
mysql> select name
    -> from celeb
    -> where name = all (select host from snl_show where id = 1);
+-----------+
| name      |
+-----------+
| 강동원    |
+-----------+
```

## ⭕ Multi Column Subquery - IN
서브쿼리 내에 메인쿼리 컬럼이 같이 사용되는 경우
```sql
SELECT column_names
FROM tablename a
WHERE (a.column1, a.column2,...) IN (SELECT b.column1, b.column2, ...
									 FROM tablename b
                                     WHERE a.column_name = b.column_name)
ORDER BY column_names;                                     
```

### 예제
강동원과 성별, 소속사가 같은 연예인의 이름, 성별, 소속사를 조회
```
mysql> select name, sex, agency
    -> from celeb
    -> where (sex, agency) in (select sex, agency from celeb where name='강동원');
+-----------+------+----------------------+
| name      | sex  | agency               |
+-----------+------+----------------------+
| 강동원    | M    | YG엔터테인먼트       |
| 차승원    | M    | YG엔터테인먼트       |
+-----------+------+----------------------+
```


