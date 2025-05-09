# 🧵 왜 멀티스레딩을 사용할까?

📅 작성일: 2025-04-03  
📂 분류: OS / Multithreading  
🔖 태그: #멀티스레딩 #멀티태스킹 #스레드 #병렬처리 #응답성 #성능

---

## 🔍 요약

멀티스레딩은 **하나의 프로그램이 동시에 여러 작업을 처리할 수 있게 해주는 방식**이다.

이를 통해 **성능 향상**, **자원 효율**, **빠른 반응**을 얻을 수 있다.

---

## ✅ 멀티스레딩을 사용하는 주요 이유

### 1. ✅ 응답성 향상

- UI는 그대로 유지하면서 백그라운드 작업 처리 가능
- ex)
    - 게임에서 캐릭터는 움직이고, 동시에 채팅 가능

### 2. ✅ CPU 활용도 극대화

- CPU는 놀고 있는 시간이 아까움
- 스레드를 여러개로 돌리면 **멀티코어 CPU**를 더 효율적으로 사용 가능

### 3. ✅ 작업 병렬 처리

- ex)
    - 서버가 동시에 여러 요청을 처리할 수 있음
- 스레드마다 독립된 작업 -> 빠른 처리 가능

### 4. ✅ 자원 절약

- 스레드는 같은 메모리 공간을 공유하므로 프로세스를 여러개 띄우는 것보다 가볍다
- 프로세스보다 **생성 비용도 낮고, 전환 속도도 빠름**

---

## ⚠️ 주의할 점

- 자원을 공유하기 때문에 **동기화 문제**가 발생할 수 있음
- 예: 두 스레드가 같은 데이터를 동시에 수정하려 하면 **충돌(race condition)** 발생

