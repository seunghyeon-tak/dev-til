# 🗂️ 데이터베이스 모델링 기초 정리

📅 작성일: 2025-06-07  
📂 분류: Database / 모델링  
🔖 태그: #Entity #ERD #개념모델 #논리모델 #물리모델 #관계형데이터베이스

---

## 🔍 주요 개념 정리

### ✅ 엔티티 (Entity)

- 정의 : 현실 세계의 객체 또는 개념을 표현한 것(사람이나 물건으로 표현함). DB에서는 테이블로 구현된다.
- 예시 : 사용자(User), 상품(Product), 주문(Order)

---

### ✅ 속성 (Attirbute)

- 정의 : 엔티티가 가지는 구체적인 정보, DB에서는 컬럼에 해당 (더 상세한 항목에 해당하는 속성)
- 예시 : User의 이름(name), 이메일(email), 생성일(created_at)

---

### ✅ 릴레이션십 (Relationship)

- 정의 : 엔티티 간의 연관성, 관계형 DB에서는 외래 키(foreigh key)로 구현됨.
- 종류
    - 1:1 (일대일) 하나의 A가 하나의 B와만 연결
        - ex. 사용자 <-> 사용자 상세정보
    - 1:N (일대다) 하나의 A가 여러 B와 연결됨
        - ex. 사용자 <-> 주문 (한명의 사용자가 여러 주문 가능)
    - N:M (다대다) 여러 A가 여러 B와 연결됨 (중간 테이블 필요)
        - ex. 학생 <-> 수업 (학생은 여러 수업, 수업은 여러 학생 수강)

---

### ✅ ER 다이어그램 (Entity-Relationship Diagram)

- 정의 : 엔티티, 속성, 릴레이션십을 도식화한 그림. 설계 초기 단계에서 사용
- 역할 : 비개발자, 개발자, 기획자 모두가 이해 가능한 데이터 구조 시각화 도구

---

### ✅ 모델링 3단계

**1. 📘 개념 모델 (Conceptual Model)**

- 비지니스 요구사항을 반영한 추상적인 수준
- ERD로 표현
- 기술 독립적 (DBMS 미고려)

**2. 📗 논리 모델 (Logical Model)**

- 개념 모델을 관계형 구조로 구체화
- 엔티티 -> 테이블, 속성 -> 컬럼
- 정규화 수행, 키 제약 조건 등 명시

**3. 📙 물리 모델 (Physical Model)**

- 논리 모델을 실제 DB에 맞춰 최적화
- 인덱스, 데이터 타입, 제약 조건, 파티셔닝 등 포함
- DBMS에 따라 구현 방식 달라짐

---

## 🧾 요약

| 구분           | 설명                  | 예시 또는 특징               |
| ------------ | ------------------- | ---------------------- |
| Entity       | 테이블이 될 대상           | User, Product          |
| Attribute    | 테이블의 컬럼 정보          | name, email            |
| Relationship | 엔티티 간의 연결관계         | 1:1, 1\:N, N\:M        |
| ER Diagram   | 위 개념들을 시각화한 설계도     | ERD 툴로 표현              |
| 개념 모델        | 추상화된 구조             | 비즈니스 요구 기반, ERD 중심     |
| 논리 모델        | 정규화, 관계형 테이블 구조화    | PK, FK 명시              |
| 물리 모델        | DBMS 기반의 최적화된 구현 모델 | MySQL, PostgreSQL 등 고려 |

---

## 🧩 핵심 정리

- ERD는 설계의 출발점이며, 모델링은 개념 -> 논리 -> 물리 단계로 발전
- 릴레이션십은 DB설계에서 가장 실수하기 쉬운 부분이며, 비지니스 로직에 맞게 정확히 정의하는 것이 중요
- 모델링을 잘하면 유지보수, 확장성, 성능 모두가 좋아진다.