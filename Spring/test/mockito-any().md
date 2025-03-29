# 📘 Mockito any() 사용 기준 & 테스트 설계 요령

📅 작성일: 2025-03-30  
📂 분류: spring/test
🔖 태그: #mockito #unit-test #any #stub #테스트전략

---

## 🔍 요약

단위 테스트에서 any()는 인자의 **정확성보다 호출 여부에 초점을 둘 때 사용**한다.
테스트 대상이 로직의 주체인지, 혹은 단순 위임자인지에 따라 `any()` 사용 여부가 결정된다.

---

## 🧠 상세 내용

### 1. any() 사용 기준 요약

- any()는 mock 객체의 메서드가 **어떤 인자든 호출되면** stub 동작하도록 만드는 도구
- 테스트하고자 하는 메서드의 인자 값이 **결과에 영향을 주는 경우**에는 `any()`를 사용하면 안됨

### 2. 계층별 사용 예시 (내가 커스텀한 계층에 대한 내용)

비지니스 계층

```java
// 비지니스 계층은 단순 위임하므로 any() 사용해도 무방
when(authService.login(any(UserRequest.class))).thenReturn(mockLoginInfo);
```

서비스 계층

```java
// 서비스 로직에 따라 이메일 / 비밀번호 분기되므로 any()는 위험
when(userRepository.findByEmail("test@test.com")).thenReturn(user);
```

### 3. 잘못된 사용 예시 & 주의점

- `any()`와 실제 값 혼용시 `InvalidOfMathchersException` 발생 가능
    - 해결법 : 모든 인자에 `any()` 또는 `eq()`를 일관되게 사용

```java
when(service.doSomething(eq("value"), anyInt())).thenReturn(...);  // OK
when(service.doSomething("value", anyInt())).thenReturn(...);  // Error
```

### 4. 헷갈린다면 아래 내용을 기억하자

- any()를 언제 쓰나?
    - 인자 내용이 결과에 영향을 주지 않을때
- 언제 쓰면 안되나?
    - 인자가 로직 분기, 예외 처리 등 테스트 핵심일때