---
layout: single
title: "SQL 기초 - Database, SQL, MySQL"
categories: SQL
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# 📖DataBase
- 여러 사람이 공유하여 사용할 목적으로 체계화해 통합, 관리하는 데이터의 집합체

### DBMS (Database Management System)
- 사용자와 데이터베이스 사이에서 사용자의 요구에 따라 정보를 생성해주고 데이터베이스를 관리해주는 소프트웨어

### 관계형 데이터베이스 (RDB : Relational Database)
- 서로간에 관계가 있는 데이터 테이블들을 모아둔 데이터 저장공간

---

# 📖SQL (Structured Query Language)
- 데이터베이스에서 데이터를 정의, 조작, 제어하기 위해 사용하는 언어

### SQL 구성
- 데이터 정의 언어 (DDL : Data Definition Language)
  - CREATE, ALTER, DROP 등의 명령어
    
- 데이터 조작 언어 (DML : Data Manipulation Language)
  - INSERT, UPDATE, DELETE, SELECT 등의 명령어
  
- 데이터 제어 언어 (DCL : Data Control Language)
  - GRANT, REVOKE, COMMIT, ROLLBACK 등의 명령어
  
---

# 📖MySQL
## Database 관리
### root 계정으로 mySQL 접속
![](https://velog.velcdn.com/images/yy2hi/post/0850c6c2-9c55-4254-95b8-8f0cdd41890d/image.png)

### Database 확인
- 현재 database 목록 확인

```sql
SHOW DATABASES;
```
```
show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
+--------------------+
```

### Database 생성
- Database 이름을 지정하여 생성

```sql
CREATE DATABASE zerobase;
```
```
show databases;
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

### Database 사용
- 해당 데이터베이스로 이동 (사용)
```sql
USE zerobase;
```
```
Database changed
```

### Database 삭제
```sql
DROP DATABASE zerobase;
```
```
show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
+--------------------+
```

---

## User 관리
- 사용자 정보는 mysql에서 관리하므로 일단 mysql 데이터베이스로 이동후 조회
### User 조회
```sql
use mysql;
SELECT host, user FROM user;
```
```
mysql> use mysql;
Database changed
mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
```

### User 생성 - localhost
- 현재 PC에서만 접속 가능한 사용자를 비밀번호와 함께 생성
```sql
CREATE USER 'username'@'localhost' identified by 'password';
```
```
mysql> create user 'noma'@'localhost' identified by '1234';
Query OK, 0 rows affected (0.01 sec)

mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | noma             |
| localhost | root             |
+-----------+------------------+
```

### User 생성 - %
- 외부에서 접속 가능한 사용자를 비밀번호와 함께 생성
```SQL
CREATE USER 'username'@'%' identified by 'password';
```
```
mysql> create user 'noma'@'%' identified by 5678;
Query OK, 0 rows affected (0.01 sec)

mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | noma             |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | noma             |
| localhost | root             |
+-----------+------------------+
```

### User 삭제
- 접근 범위에 따라 같은 이름의 사용자여도 별도로 삭제
```sql
DROP USER 'username'@'localhost'
DROP USER 'username'@'%'
```
```
mysql> drop user 'noma'@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> drop user 'noma'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
```

---

## User 권한 관리
### 실습환경 만들기 1 - Database 만들기
- 권한 관리 실습을 위한 Database (testdb) 생성
```sql
CREATE DATABASE testdb;
```
```
mysql> create database testdb;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| testdb             |
+--------------------+
```

### 실습환경 만들기 2 - User 만들기
- 권한 관리 실습을 위한 사용자 (noma@localhost, 1234) 생성
```sql
CREATE USER 'noma'@'localhost' identified by '1234';
```
```
mysql> use mysql;
Database changed
mysql> create user 'noma'@'localhost' identified by '1234';
Query OK, 0 rows affected (0.00 sec)

mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | noma             |
| localhost | root             |
+-----------+------------------+
```

### User 권한 확인
- 사용자에게 부여된 모든 권한 목록을 확인

```sql
SHOW GRANTS FOR 'username'@'localhst';
```
```
mysql> show grants for 'noma'@'localhost';
+------------------------------------------+
| Grants for noma@localhost                |
+------------------------------------------+
| GRANT USAGE ON *.* TO `noma`@`localhost` |
+------------------------------------------+
```

### User 권한 부여
- 사용자에게 특정 데이터베이스의 모든 권한을 부여
```sql
GRANT ALL ON dbname.* to 'username'@'localhost';
```
```
mysql> grant all on testdb.* to 'noma'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql> show grants for 'noma'@'localhost';
+----------------------------------------------------------+
| Grants for noma@localhost                                |
+----------------------------------------------------------+
| GRANT USAGE ON *.* TO `noma`@`localhost`                 |
| GRANT ALL PRIVILEGES ON `testdb`.* TO `noma`@`localhost` |
+----------------------------------------------------------+
```

### User 권한 제거
- 사용자에게 특정 데이터베이스의 모든 권한을 삭제
```sql
REVOKE ALL ON dbname.* from 'username'@'localhost';
```
```
mysql> revoke all on testdb.* from 'noma'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql> show grants for 'noma'@'localhost';
+------------------------------------------+
| Grants for noma@localhost                |
+------------------------------------------+
| GRANT USAGE ON *.* TO `noma`@`localhost` |
+------------------------------------------+
```
#### ❗ 수정내용이 적용이 되지 않는 경우 새로고침
```sql
FLUSH PRIVILEGES;
```
