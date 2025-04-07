# 🧵 스레드(Thread)란?

📅 작성일: 2025-04-03  
📂 분류: OS / Multithreading  
🔖 태그: #스레드 #Thread #운영체제 #멀티스레딩 #실행흐름

---

## 🔍 요약

**스레드(Thread)**는 프로그램(=프로세스) 안에서 **작업을 실행하는 가장 작은 작업단위**이다.

쉽게 말해, 하나의 프로그램이 동시에 여러 일을 할 수 있게 해주는 실행 흐름이다.

---

## 💡 왜 스레드가 필요할까?

- 하나의 프로그램이 여러 작업을 동시에 처리하려면 필요함
- ex)
    - 영상 플레이어에서 영상 재생 + 자막 출력 + 사용자 입력 처리
    - 웹 서버가 동시에 여러 요청 처리

---

## ✨ 비유로 이해하기

- 프로세스: 하나의 회사
- 스레드: 회사 안의 직원들
  - 같은 자원(건물, 컴퓨터 등)을 공유하면서 각자 다른 일을 함

---

## 📝 정리 메모

- 스레드는 가볍고 빠르지만, 공유 자원 때문에 충돌 주의 해야한다.
- 멀티스레딩은 성능 향상을 위한 중요한 개념이지만, **동기화 문제도 함께 따라온다.**

---

## 멀티 스레딩 실습

### 기본구조

```java
public class MultiThreadExample {
    public static void main(String[] args) {
        Runnable task = () -> {
            String name = Thread.currentThread().getName();
            for (int i = 0; i < 5; i++) {
                System.out.println(name + " - count : " + i);
            }
        };

        Thread t1 = new Thread(task, "Worker-1");
        Thread t2 = new Thread(task, "Worker-2");
        Thread t3 = new Thread(task, "Worker-3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```

- Worker-1, Worker-2, Worker-3이 **동시에 실행**되며 로그가 섞여서 출력
- 실행 순서는 매번 달라짐 -> 컨텍스트 스위칭 존재

---

## ✅ 멀티스레딩이 사용되는 대표 사례

### 1. 🔄 HTTP 요청 처리 (웹 서버 동작 구조)

- Spring Boot는 내부적으로 Tomcat 같은 웹 서버를 사용하며,
- **클라이언트의 요청을 하나의 스레드로 처리**
- 동시에 여러 요청이 들어와도 **스레드 풀로 각각 처리됨**

📌 관련 구성:
- 'ThreadPoolExecutor'
- 톰캣의 스레드 풀 설정 ('server.tomcat.thread.max' 등)

### 2. 📨 비동기 작업 처리

- 오래 걸리는 작업(이메일 전송, 알림, Slack 메시지 등)을 별로 스레드로 분리
- **응답 속도 향상**, **사용자 경험 개선**

📌 스프링에서는 이렇게 사용:
```java
@Async
public void sendEmail(String to) {
  // 별도 스레드에서 처리됨
}
```

`@Async`를 붙이면 해당 메서드는 별도의 스레드 풀에서 실행되며,

메인 요청 흐름과 분리되어 비동기로 처리된다.

즉, 메서드 자체가 다른 스레드에서 실행되는 구조이다.

### 3. 🕐 배치 & 스케줄링 작업

- 시간에 따라 반복적으로 수행되는 작업도 스레드 기반으로 실행됨

```java
@Scheduled(cron = "0 0 1 * * *")
public void cleanUpOldData() {
  // 매일 새벽 1시에 실행
}
```

### 4. ⚙️ 병렬 처리 / 멀티코어 활용

- 대량의 데이터를 여러 스레드로 동시에 처리해서 성능 향상
- 이미지 변환, 파일 업로드 처리 등에서 사용

```java
ExecutorService executor = Executors.newFisedThreadPool(4);
executor.submit(() -> processPart(dataChunk));
```

### 5. 🔒 동시성 제어 (락, 세마포어 등)

- 여러 스레드가 동시에 DB 또는 캐시에 접근하면 데이터 무결성 문제 발생
- 락을 걸거나, Redis 분산락으로 제어


📌 실무 적용 예:

- synchronized, ReentrantLock
- Redis SETNX, Redisson