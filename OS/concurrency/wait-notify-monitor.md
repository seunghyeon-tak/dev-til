# 🌀 wait(), notify(), 그리고 Monitor란?

📅 작성일: 2025-04-10  
📂 분류: OS / Concurrency  
🔖 태그: #운영체제 #스레드동기화 #wait #notify #모니터 #스레드제어

---

## 🔍 개요

Java의 `wait()` / `notify()`는 스레드 간 협업을 위한 동기화 도구이다.

운영체제 이론에서 나오는 **모니터(Monitor)** 개념을 기반으로 한다.

- `synchronized` 블록 내부에서만 사용할 수 있으며
- 하나의 공유 자원을 기준으로 **여러 스레드가 순차적으로 작업을 수행할 수 있도록 조율**한다.

---

## 🧠 Monitor란?

> 한 번에 하나의 스레드만 접근할 수 있도록 보호된 공유 자원 공간

Java의 `synchronized` 블록은 해당 객체에 대해 **모니터** 락을 획득한 스레드만 진입 가능하도록한다.

- 모니터는 내부에 락 + 대기 큐 를 갖는다.
- `wait()`을 호출하면 스레드는 락을 반납하고 wait set에서 대기
- `notify()`는 대기 중인 스레드 하나를 깨워 락 획득을 시도하게 함

---

## 🔄 상태 변화 흐름

```text
[Thread A] 진입 (락 획득) ──┐
                         ↓
        [wait()] 호출 → 락 반납 → Wait Set으로 이동
                         ↓
        [Thread B] 진입 → 작업 완료 후 [notify()]
                         ↓
        [Thread A] 다시 깨어남 → 락 재획득 후 실행 재개 (새로운 작업을 하는게 아니고 기존 작업을 재개하는걸로 이해하기)
```

---

## ✅ 주요 메서드 정리

| 메서드 | 	설명 |
|------|------|
|wait() |	현재 스레드를 Wait Set으로 이동시키고, 락 반납|
|notify() |	대기 중인 스레드 하나를 깨움|
|notifyAll() |	모든 대기 스레드를 깨움|

⚠️ 모두 synchronized 블록 안에서만 호출 가능

---

## ⚙️ 간단한 예제

```java
class SharedData {
    private boolean ready = false;

    public synchronized void produce() {
        ready = true;
        notify();  // 대기 중인 소비자 스레드 깨우기
    }

    public synchronized void consume() {
        while (!ready) {
            try {
                wait();  // 락 반납 + 대기
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        System.out.println("");
    }

}
```

---

## 🗒️ 메모

**실 운영 서비스에서는 쓰는 경우가 드물다고 함**

이유
- 락 - 대기 - 조건 흐름을 개발자가 다 직접 짜야해서 실수 위험이 높음
- 대체제가 많다. (wait(), notify()를 추상화한 고수준 API가 많음)
- 예외 처리 디버깅이 까다롭다. (잘못쓰면 스레드가 영원히 안깨어나 데드락 발생)

**대체제**

- 비동기 큐 : BlockingQueue, 메시지 큐 (kafka, RabbitMQ 등)
- 백그라운드 작업 : ThreadPoolExecutor, @Async, 배치작업
- 자원 접근 제어 : Semaphore, Redis Lock, DB 트랜잭션
- 상태 동기화 : CountDownLatch, CompletableFuture 등