# 🛠 데이터베이스 조작 기본 (Database Operation Basic)

📅 작성일: 2025-05-24
📂 분류: Database
🔖 태그: #SQL기초 #데이터베이스조작 #MySQL명령어

---

## 📌 상세 내용

### 1. SQL 조작을 위한 준비와 문법

|항목 |	설명|
|----|----|
|클라이언트 접속 |	터미널이나 GUI 툴(MySQL Workbench 등)로 DB 접속|
|SQL 문법 기본 |	대소문자 구분 X, 문장 끝에 ; 필수, 키워드는 보통 대문자|
|실행 구조 |	명령어 + 대상 + 조건 (예: SELECT * FROM users WHERE age > 20;)|

### 2. 데이터베이스와 테이블 다루기

|명령어 |	설명|
|-----|-------|
|CREATE DATABASE |	새로운 DB 생성|
|DROP DATABASE |	DB 삭제 (주의 필요)|
|SHOW DATABASES |	전체 DB 목록 조회|
|USE <DB> |	사용할 DB 선택|
|CREATE TABLE |	테이블 구조 정의 (열, 자료형 등 포함)|
|DROP TABLE |	테이블 삭제|

### 3. 레코드 조작하기

|동작 |	SQL 예시 |	설명|
|----|---------|-----|
|추가 |	INSERT INTO 테이블명 VALUES(...) |	레코드 삽입|
|조회 |	SELECT * FROM 테이블명 |	전체 레코드 조회|
|조건검색 |	SELECT * FROM 테이블명 WHERE 조건 |	조건을 만족하는 데이터만 조회|

### 4. 조건 검색과 연산자 활용

**WHERE 조건 연산**

|연산자 |	의미 |	예시|
|-----|-------|-----|
|= / != |	같다 / 다르다 |	WHERE age != 30|
|BETWEEN A AND B |	A 이상 B 이하 |	WHERE price BETWEEN 1000 AND 5000|
|NOT BETWEEN |	범위 외 |	WHERE score NOT BETWEEN 60 AND 90|

**포함 여부 및 NULL 판별**

|연산자 |	설명 |	예시|
|-----|-------|-----|
|IN (값1, 값2) |	여러 값 중 포함 여부 |	WHERE grade IN ('A', 'B')|
|NOT IN (...) |	포함되지 않은 경우 |	WHERE id NOT IN (1,2)|
|IS NULL |	값이 비어 있는 경우 |	WHERE phone IS NULL|
|IS NOT NULL |	값이 존재하는 경우 |	WHERE email IS NOT NULL|

---

## 5. 흐름 요약 예시

실제로 DB를 다룰 땐 다음과 같은 흐름으로 진행된다:

사용할 DB 선택: **USE shop;**

테이블 생성: **CREATE TABLE products (...)**

레코드 추가: **INSERT INTO products VALUES (...)**

특정 조건 검색: **SELECT * FROM products WHERE price < 10000;**

---

## 정리

SQL의 기본 조작 명령은 데이터를 어떻게 만들고, 다루고, 필터링할 것인가에 초점이 맞춰져 있다.

특히 `SELECT + WHERE` 조합은 실무에서 가장 많이 사용되는 형태이므로

조건절, 연산자, NULL 처리 등을 정확히 이해하고 익숙해지는 것이 중요하다.
