# ⚙️ XML

📅 작성일: 2025-10-23 
📂 분류: ComputerStructure  
🔖 태그: #XML #데이터전송

---

## XML이란 무엇인가?

- 데이터를 저장하고 전송하기위한 형식이다. (데이터를 구조적으로 표현하는 마크업)

## XML 기본 구조

```xml
<?xml version="1.0" encoding="UTF-8"?>
<student>
  <name>홍길동</name>
  <age>21</age>
  <major>Computer Science</major>
</student>
```

- XML의 구성
    - 프롤로그 버전을 나타냄
    - 하나의 루트 태그 (단 하나만 있어야함)
    - 열린 태그와 닫힌 태그가 반드시 쌍으로 존재해야함
    - 하위 요소들

## HTML vs XML

- HTML은 웹 페이지 `표시` XML은 데이터 `저장/전송`
- HTML은 미리 정해진 태그만 사용가능 XML은 사용자가 태그를 직접 정의 가능
- HTML은 비교적 느슨함 (닫는 태그 없이도 동작) XML은 모든 태그가 쌍으로 존재해야함

## JSON vs XML

- JSON은 `키-값` 기반 XML은 `태그` 기반
- JSON은 간결하고 직관적이고 XML은 사람이 보기엔 복잡함
- 데이터 크기는 XML이 더 크다 (태그 반복때문에)
- JSON은 요즘 API 통신의 표준이며, XML은 문서 규격, 시스템간 연동, 표준화된 데이터 교환에서 사용됨
