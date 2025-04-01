# 📘 Spring 페이징 처리 정리

📅 작성일: 2025-04-02  
📂 분류: spring/jpa  
🔖 태그: #페이징 #Pageable #Page

---

## 🔍 요약

Spring Boot + JPA 환경에서 어떠한 목록을 페이징 처리하여 조회하는 API를 구현 가능하다.

JPA에서 제공하는 Pageable, Page<T>, PageRequest.of()를 활용해서

page, size, sort 파라미터 기반의 유연한 페이징을 처리를 할 수 있다.

---

## ✅ 상세 내용

### 🔹 기본 구조

```java
Pageable pageable = PageRequest.of(page, size, Sort.by("createdAt").descending());
Page<ProductEntity> page = productRepository.findByIsActiveTrue(pageable);
```

### 🔹 핵심 개념

- Pageable : 페이징 요청 정보를 담는 객체 (page, size, sort 등)
- Page<T> : 페이징된 결과 데이터 (실제 데이터 + 전체 개수 등 포함)
- PageRequest.of(...) : Pageable 객체 생성용 메서드


### 🔹 API 요청 예시

```http
GET /api/v1/products?page=0&size=10&sort=createdAt,DESC
```

### 🔹 응답 예시 구조 (Page 내부)

```json
{
  "content": [...],             // 상품 데이터
  "totalElements": 200,         // 전체 상품 수
  "totalPages": 20,             // 전체 페이지 수
  "number": 0,                  // 현재 페이지 번호
  "size": 10,                   // 페이지당 개수
  "sort": { ... },              // 정렬 정보
  ...
}
```

### 🔹 DTO 변환 시 page.map() 사용

```java
Page<ProductResponse> result = page.map(this::toResponse);
```

-> 반복문 없이 한 줄로 엔티티 -> DTO 변환 가능

-> 페이징 정보도 자동 유지됨
