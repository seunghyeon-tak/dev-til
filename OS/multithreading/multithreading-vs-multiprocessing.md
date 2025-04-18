# ⚔️ 멀티스레딩 vs 멀티프로세싱

📅 작성일: 2025-04-03  
📂 분류: OS / Multithreading  
🔖 태그: #멀티스레딩 #멀티프로세싱 #성능비교 #운영체제 #병렬처리

---

## 🔍 요약

**멀티스레딩**과 **멀티프로세싱**은 모두 **작업을 동시에 처리**하기 위한 방법이다.

하지만 구조와 성능, 안정성, 사용 목적이 다르다.

---

## ✅ 핵심 개념 요약

| 항목           | 멀티스레딩                           | 멀티프로세싱                           |
|----------------|---------------------------------------|-----------------------------------------|
| 실행 단위       | 하나의 프로세스 내 여러 스레드         | 여러 개의 프로세스                     |
| 메모리 구조     | 메모리 공간 공유                     | 메모리 공간 독립                       |
| 통신 방식       | 빠름 (자원 공유)                     | 느림 (IPC 필요)                        |
| 안정성         | 하나가 죽으면 전체 영향 받을 수 있음   | 하나 죽어도 다른 프로세스는 영향 없음 |
| 자원 소비       | 적음                                 | 많음                                   |
| 사용 예시       | 웹 서버, 채팅 앱, 게임 클라이언트 등   | 대용량 병렬 연산, 머신러닝, 파이썬 연산 등 |

---

## 🧠 예시로 이해하기

- 멀티스레딩 :
    - 하나의 앱 안에서 동시에 여러 작업 처리
    - ex) 게임에서 동시에 애니메이션, 입력 처리, 소리 출력

- 멀티프로세싱 : 
    - 서로 독립된 프로그램들이 병렬로 작업
    - ex) CPU 코어별로 이미지를 병렬로 처리하는 프로그램

---

## ⚠️ 주의할 점

- 멀티스레딩은 공유 자원 충돌(동기화 문제)에 주의 해야함
- 멀티프로세싱은 자원 소비 많고 통신 비용이 크다
