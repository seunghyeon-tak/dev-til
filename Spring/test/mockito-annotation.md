# 📘 Mockito any() 사용 기준 & 테스트 설계 요령

📅 작성일: 2025-03-30  
📂 분류: spring/test
🔖 태그: #mockito #unit-test #assertThrows #stub #mock

---

## 🔍 요약

단위 테스트에서 `@Mock`, `@InjectMocks` 어노테이션을 통해 테스트 대상 객체를 구성하고
`assertThrows`, `verify()`를 통해 예외와 호출 여부를 정확하게 검증한다.

---

## 🧠 상세 내용

### 1. Mockito 어노테이션 정리

- @Mock
    - 가짜 의존성 객체 생성
- @InjectMocks
    - 테스트 대상 객체 생성 + @Mock 필드를 주입
- @ExtendWith(MockitoExtension.class)
    - Mockito JUnit5 연동


### 2. 예외 검증 방법

**권장 방식 : assertThrows()**

```java
ApiException ex = assertThrows(ApiException.class, () -> {
    service.login(invalidRequest);
});

assertThat(ex.getErrorDescription())
    .isEqualTo(UserResponseCode.EMAIL_NOT_EXIST.getDescription());
```

| 장점 : 예외 객체를 변수로 받아 추가 검증 가능

참고 : **assertThatThrowBy() - AssertJ**

```java
assertThatThrownBy(() -> service.login(invalidRequest))
    .isInstanceOf(ApiException.class)
    .hasMessageContaining("존재하지 않는 이메일");
```

| 단점 : 예외 객체에 대한 직접 접근 어려움

### 3. void 메서드 stub & 예외

정상 호출 허용 : doNothing().when(mock).method();

예외 발생 유도 : doThrow(new ApiException(...)).when(mock).method();

```java
doThrow(new ApiException(...)).when(Service).save(any());
```