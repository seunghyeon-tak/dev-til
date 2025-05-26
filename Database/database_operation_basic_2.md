# 🧾 SQL 데이터 조작 명령어 (UPDATE ~ JOIN)

📅 작성일: 2025-05-27
📂 분류: Database
🔖 태그: #SQL #UPDATE #DELETE #GROUPBY #JOIN

---

## 📌 상세 내용

### 1. 레코드 수정과 삭제

|명령어 |	설명 |	예시 |
|-----|-------|------|
|UPDATE|	기존 레코드의 값을 수정할 때 사용. WHERE 없으면 전체 수정됨 |	UPDATE users SET age = 30 WHERE id = 1;|
|DELETE |	조건에 맞는 레코드를 삭제. WHERE 없으면 전부 삭제됨 |	DELETE FROM users WHERE age < 18;|

- 주의: WHERE 절이 없을 경우 전체 테이블이 변경될 수 있어 매우 위험하다.
- 트랜잭션 또는 백업 후 작업하는 것이 권장됨.

### 2. 정렬과 집계 함수

|명령어|	설명 |	예시 |
|----|--------|------|
|ORDER BY |	결과를 정렬할 때 사용 (기본: ASC, 역순: DESC) |	SELECT * FROM products ORDER BY price DESC;|
|COUNT(*) |	레코드 수 계산 |	SELECT COUNT(*) FROM orders WHERE status = 'complete';|
|MAX, MIN |	최댓값/최솟값 계산 |	SELECT MAX(price), MIN(price) FROM products;|
|SUM, AVG |	총합/평균 계산 |	SELECT SUM(amount), AVG(amount) FROM sales;|

- ORDER BY는 거의 모든 SELECT문과 함께 쓰일 수 있다.
- 집계 함수는 주로 GROUP BY와 같이 쓰면 유용하다.

### 3. 그룹화와 조건 집계

|명령어 |	설명 |	예시 |
|-----|-------|------|
|GROUP BY |	특정 컬럼을 기준으로 데이터를 묶음 |	SELECT category, COUNT(*) FROM products GROUP BY category;|
|HAVING |	그룹화된 데이터에 조건을 걸 때 사용 |	SELECT category, COUNT(*) FROM products GROUP BY category HAVING COUNT(*) > 5;|

- WHERE은 레코드 필터링, HAVING은 그룹 필터링에 사용된다.
- HAVING 없이 조건을 걸면 오류 발생 (집계 함수가 있는 조건은 무조건 HAVING 사용).

### 4. JOIN - 여러 테이블 연결

|명령어 |	설명 |	예시 |
|------|-------|-----|
|INNER JOIN |	양쪽 테이블에 모두 값이 있을 때만 결과 반환 |	SELECT u.name, o.amount FROM users u INNER JOIN orders o ON u.id = o.user_id;|
|LEFT JOIN |	왼쪽 테이블 기준, 오른쪽에 없어도 NULL로 포함 |	SELECT u.name, o.amount FROM users u LEFT JOIN orders o ON u.id = o.user_id;|
|RIGHT JOIN |	오른쪽 테이블 기준, 왼쪽에 없어도 포함 |	SELECT u.name, o.amount FROM users u RIGHT JOIN orders o ON u.id = o.user_id;|

- `JOIN`은 실무에서 필수: 거의 모든 복잡한 SQL문에 포함됨
- `LEFT JOIN`은 "없는 데이터를 포함해도 되는 경우" 매우 유용
- `ON` 뒤에는 테이블 간 연결 조건이 반드시 필요함

---

## 생각하기

- 조건 없이 UPDATE/DELETE하는 실수를 방지하려면 항상 WHERE 절부터 생각하자.
- 집계함수와 GROUP BY는 데이터 요약 분석에 정말 중요함.
- JOIN은 개념적으로 어렵지만, 다른 테이블의 데이터를 붙여주는 방식이라고 생각하면 된다.
- WHERE과 HAVING의 차이를 구분하는 연습 필요