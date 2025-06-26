# 📊 RDS 기본 개념과 적용 방법

🗓️ TIL: 2025-06-26  
📌 분류: AWS / Database  
🏷️ 태그: RDS, MySQL, Spring, DB 연결

---

## ✅ RDS란?

> 관계형 데이터베이스 서비스

- 사용자는 DB 설치/패치/백업/복제 등을 직접 하지 않아도 됨
- MySQL, PostgreSQL, MariaDB, Oracle, SQL Server 등을 선택 가능

---

## ✅ 간단한 RDS 아키텍처

![Image](https://github.com/user-attachments/assets/c205722c-f8bf-4827-b7de-e2b734974663)

---

## ✅ RDS를 왜 사용해야 하나?

- 로컬 DB는 내 컴퓨터에서만 접근 가능
- 배포한 서버는 외부에 있으므로, RDS처럼 클라우드에 존재하는 DB가 필요
- 내구성/가용성/자동 백업/장애 복구 등의 혜택을 누릴 수 있음

---

## ✅ EC2에 MySQL 직접 설치 vs RDS

| 항목      | EC2 + 직접설치      | RDS                 |
| ------- | --------------- | ------------------- |
| 설치 관리   | 직접 구성해야 함       | AWS가 자동 관리          |
| 장애 복구   | EC2 장애 시 DB도 다운 | Multi-AZ 구성 시 자동 복구 |
| 백업      | 수동 설정 필요        | 자동 백업 지원            |
| 성능 모니터링 | 별도 설치           | CloudWatch 통합       |
| 비용      | 상대적으로 저렴        | 비싸지만 안정성↑           |

**📌 결론: 소규모 POC나 학습용은 EC2로도 가능, 실무 운영은 RDS가 일반적**

---

## ✅ RDS 사용 시 추가적으로 알아야 할 것

- 보안 그룹 설정 : 내 로컬 또는 EC2에서 RDS 접속하려면 보안 그룹에서 3306 포트 열어야 함
- 자동 백업 설정 가능
- 모니터링 지표 : CPU 사용률, 연결 수 등 -> CloudWatch에서 확인 가능
- Parameter Group : DB의 설정값들 관리
- Subnet Group : VPC 내에서 RDS의 가용 영역 설정

---

## ✅ Spring Boot 프로젝트에서 RDS 연결 방법

### 1. `application-prod.yml` or `application-dev.yml` 작성

```yaml
spring:
  datasource:
    url: jdbc:mysql://your-db-name.xxxxxxxxxxxx.ap-northeast-2.rds.amazonaws.com:3306/db_name?serverTimezone=Asia/Seoul&characterEncoding=UTF-8
    username: your_username
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: none # 또는 validate, update
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect
    show-sql: true
```

### 2. RDS 보안 그룹에서 접근 허용

- 3306 포트 열기
- 로컬 IP / EC2 IP / CIDR 대역(ex. 0.0.0.0/0) 등을 허용해야 접근 가능

### 3. RDS MySQL 접속 테스트

```bash
mysql -h your-db-name.xxxxx.rds.amazonaws.com -u your_username -p
```

### 4. Flyway 등 마이그레이션 도구 설정

```yaml
spring:
  flyway:
    enabled: true
    baseline-on-migrate: true
```