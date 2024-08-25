---
layout: single
title: "SQL 기초 - Table"
categories: SQL
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# Table
- 데이터베이스 안에서 실제 데이터가 저장되는 형태이고, 행(Row)과 열(Column)로 구성된 데이터 모음
![](https://velog.velcdn.com/images/yy2hi/post/76c4a693-6b97-43c5-b0cc-906366369ef0/image.png)

### 실습할 데이터베이스 생성
- zerobase라는 이름의 데이터베이스 생성
```sql
CREATE DATABASE zerobase DEFAULT CHARACTER SET utf8mb4;
```
```
mysql> create database zerobase default character set utf8mb4;
Query OK, 1 rows affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| zerobase           |
+--------------------+
```
#### ❗ utf8mb4 : 다국적 언어, 이모지 포함

---

## Table 생성
```sql
CREATE TABLE tablename
(
	columnname datatype,
    columnname datatype,
    ...
);   
```
#### id(int), name(varchar(16) 칼럼을 가지는 mytable이라는 이름의 테이블 생성
```
mysql> create table mytable
    -> (
    ->  id int,
    ->  name varchar(16)
    -> );
Query OK, 0 rows affected (0.01 sec)
```
### Table 목록 확인
```sql
SHOW TABLES;
```
```
mysql> show tables;
+--------------------+
| Tables_in_zerobase |
+--------------------+
| mytable            |
+--------------------+
```

### Table 정보 확인
```sql
DESC tablename;
```

#### mytable 테이블 정보 확인
```sql
DESC mytable;
```
```
mysql> desc mytable;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(16) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

---

## Table 변경
### Table 이름 변경
```sql
ALTER TABLE tablename
RENAME new_tablename
```
#### mytable 이름 person으로 변경
```sql
ALTER TABLE mytable RENAME person;
```
```
mysql> alter table mytable rename person;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+--------------------+
| Tables_in_zerobase |
+--------------------+
| person             |
+--------------------+
```

### Table Column 추가
```sql
ALTER TABLE tablename
ADD COLUMN columnname datatype;
```
#### person 테이블에 agee(double) 컬럼 추가
```sql
ALTER TABLE person ADD COLUMN agee double;
```
```
mysql> alter table person add column agee double;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc person;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(16) | YES  |     | NULL    |       |
| agee  | double      | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

### Table Column 변경 - DataType
```sql
ALTER TABLE tablename
MODIFY COLUMN columnname datatype;
```
#### person 테이블의 agee 컬럼의 데이터 타입 int로 변경
```sql
ALTER TABLE person
MODIFY COLUMN agee int;
```
```
mysql> alter table person modify column agee int;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc person;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(16) | YES  |     | NULL    |       |
| agee  | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

### Table Column 변경 - Name
```sql
ALTER TABLE tablename
CHANGE COLUMN old_columnname new_columnname new_datatype;
```

#### person 테이블의 agee 컬럼 이름 age로 변경
```sql
ALTER TABLE person
CHANGE COLUMN agee age int;
```
```
mysql> alter table person change column agee age int;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc person;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(16) | YES  |     | NULL    |       |
| age   | int         | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

### Table Column 삭제 문법
```sql
ALTER TABLE tablename
DROP COLUMN columnname;
```

#### person 테이블의 age컬럼 삭제
```sql
ALTER TABLE person
DROP COLUMN age;
```
```
mysql> alter table person drop column age;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc person;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int         | YES  |     | NULL    |       |
| name  | varchar(16) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```

### Table 삭제
```sql
DROP TABLE tablename;
```
#### person 테이블 삭제
```SQL
DROP TABLE person;
```
```
mysql> drop table person;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
Empty set (0.00 sec)
```