# 📘 컨텍스트 스위칭 (Context Switching)

📅 작성일: 2025-03-31  
📂 분류: OS  
🔖 태그: #운영체제 #컨텍스트스위칭 #프로세스 #스레드 #CPU

---

## 🔍 요약

**컨텍스트 스위칭**은 CPU가 **현재 작업중인 프로세스(or 스레드)의 상태를 저장**하고

**다른 프로세스(or 스레드)의 상태를 불러와서 실행을 전환**하는 과정이다.

---

## 💡 왜 필요한가요?

- 컴퓨터는 보통 CPU가 1개 또는 몇개뿐인데,
- 여러개의 작업 (프로세스, 스레드)을 **동시에 실행하는 것처럼** 보이게 하려면
- CPU가 **빠르게 작업을 전환하며 실행** 해야한다.

-> 이러한 전환 과정이 컨텍스트 스위칭이다.

---

## 🧠 상세 설명

### ✅ "Context"란?

- 프로세스의 실행 상태를 말함
- 예) CPU 레지스터, 프로그램 카운터, 메모리 상태 등

### ✅ Context Switching 과정 요약

1. 현재 실행 중인 작업의 상태 저장
    - 이걸 PCB(Process Control Block)나 TCB(Thread Control Block)에 저장함
2. 다음 실행할 작업의 상태를 불러오기
3. CPU가 다음 작업으로 실행 전환

### 🌀 이 과정은 운영체제가 수행

- 정확히는 **CPU 스케줄러**가 어떤 작업으로 바꿀지 결정하고,
- **커널**이 전환 작업을 처리한다.

---

## 📦 예시로 쉽게 이해하기

예를 들어 스마트폰에서

유튜브를 보다가 카카오톡 메시지가 오면 잠깐 전환

그리고 다시 유튜브 복귀

-> **앱 자체가 종료되는게 아니라**, 운영체제가 앱의 상태를 기억해두고 **필요할 때 다시 불러옴**

---

## 참고: Context Switching의 단점

- **오버헤드(추가 비용)**가 발생함
    -> 저장 & 불러오는 작업 시간이 걸림
- 너무 자주 발생하면 오히려 비효율적
    -> 실제 일보다 작업 바꾸는 일에 시간이 들어감

---

## 📘 정리 요약

| 항목         | 설명 |
|--------------|------|
| 정의         | 작업 상태 저장 & 전환 |
| 왜 필요한가? | 여러 작업을 동시에 실행하는 것처럼 보이게 하려고 |
| 누가 처리하나 | 운영체제 (커널, 스케줄러) |
| 단점         | 느려질 수 있음 (오버헤드) |