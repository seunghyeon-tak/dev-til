# ⚠️ Race Condition (경쟁 상태)

📅 작성일: 2025-04-04  
📂 분류: OS / Synchronization  
🔖 태그: #race-condition #경쟁상태 #동기화문제 #스레드충돌 #운영체제

---

## 🔍 요약

Race Condition(경쟁 상태)은 
여러 스레드나 프로세스가 **동시에 같은 자원에 접근해서 예기치 않은 결과가 나오는 상황**이다.

---

## 🎯 쉽게 설명하면

- 두명이 동시에 **같은 문서 수정**하면 충돌나는 것처럼
- **동시에 실행되는 스레드가 같은 변수나 자원을 변경하려고 할 때 문제가 생김**

---

## 💥 예시 상황

### 예: 은행 계좌에서 출금하는 코드

```java
if (balance >= 10000) {
    balance -= 10000;
}
```

- 두 스레드가 동시에 위 조건을 만족하고 실행한다면?
    - 둘다 balance가 10000이상이라고 판단하고 출금
    - 실제로는 20000이 빠짐 -> **잘못된 결과**

---

## 🧠 왜 발생할까?

- 멀티스레딩 환경에서 스레드들은 CPU 시간을 나눠 씀
- 코드 실행 순서가 매번 달라질 수 있음
- 동시에 변수에 접근하면 순서가 꼬일 수 있음

-> 이런 상황을 **비동기적 충돌**이라고함

---

## ✅ 방지 방법

- **임계영역(Critical Section)** 설정
- **락(Lock), 뮤텍스(Mutex), 세마포어(Semaphore)** 같은 동기화 기법 사용
- 자원에 접근할 때 **한번에 하나만 접근**하도록 제한

---

## 📦 정리 요약

|  항목 |	내용  |
|------|--------|
|정의	| 여러 스레드가 같은 자원에 동시에 접근해 잘못된 결과가 나오는 현상|
|왜 위험한가? |	데이터 무결성 파괴, 예기치 않은 버그 발생|
|방지 방법 |	락, 뮤텍스, 임계 영역 설정 등 동기화 처리|
|실무 예시 |	은행 앱 출금, 쇼핑몰 재고 수량 처리, 게시글 좋아요 수 증가 등|

---

## ✅ 실습 예제: 동기화 없는 경우

```java
public class RaceConditionExample {
    static int count = 0;

    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 0; i < 1_000_000; i++) {
                count++;  // 경쟁 상태 발생 가능
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start(); 
        t2.start();

        t1.join(); 
        t2.join();

        System.out.println("최종 count 값 : " + count);
    }
}
```

count를 출력하면 값이 매번 달라지는걸 확인 할 수 있다.

-> 여러 스레드가 동시에 count 값을 증가시키면서 값이 꼬임

## ✅ 해결 방법: 동기화 적용

### synchronized 블록 사용

```java
public class RaceConditionExample {
    static int count = 0;
    static final Object lock = new Object();  // 공유 락 객체

    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 0; i < 1_000_000; i++) {
                synchronized (lock) {
                    count++;  // 공유 자원 보호
                }
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("최종 count 값 : " + count);
    }
}
```

### AtomicInteger 사용 (간단한 방법)

```java
public class RaceConditionExample {
    static AtomicInteger count = new AtomicInteger(0);

    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            for (int i = 0; i < 1_000_000; i++) {
                count.incrementAndGet();  // 원자적 연산
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("최종 count 값 : " + count);
    }
}
```

---

## ✅ 현실에서 자주 발생하는 예시

### 재고 감소

> 동시에 100명이 상품 구매 -> 재고가 1인데 2명이 동시에 주문
> -> 둘다 재고 확인 했고 -> 재고 -1 -> 재고가 음수가 되어버림

### 실습 플로우

- 가상의 상품 재고를 1로 설정
- 동시에 10개의 스레드로 "상품 구매 시도"
- if (stock > 0) stock--; 같은 로직 -> 경쟁 상태 발생
- `synchronized` 또는 `AtomicInteger`, `ReentrantLock`으로 해결

### 실습 코드

```java
public class Stock {
    static int stock = 1;

    public static void main(String[] args) throws InterruptedException {
        Runnable buyTask = () -> {
            if (stock > 0) {
                try {
                    Thread.sleep(10); // 일부러 delay
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                stock--;  // 동기화 안됨 -> 경쟁상태 발생 가능
                System.out.println(Thread.currentThread().getName() + " 구매 완료. 남은 재고 : " + stock);
            } else {
                System.out.println(Thread.currentThread().getName() + " 구매 실패. 재고 없음");
            }
        };

        for (int i = 0; i < 10; i++) {
            new Thread(buyTask, "사용자-" + i).start();
        }
    }
}
```

**위 코드 결과**

- 재고는 1개인데, 여러 스레드가 동시에 `stock > 0` 을 통과
- 결국 stock이 0 이하로 떨어질 수 있음

```text
사용자-0 구매 완료. 남은 재고 : -5
사용자-4 구매 완료. 남은 재고 : -3
사용자-8 구매 완료. 남은 재고 : -8
사용자-7 구매 완료. 남은 재고 : -7
사용자-9 구매 완료. 남은 재고 : -9
사용자-2 구매 완료. 남은 재고 : -3
사용자-3 구매 완료. 남은 재고 : -3
사용자-6 구매 완료. 남은 재고 : -4
사용자-1 구매 완료. 남은 재고 : -3
사용자-5 구매 완료. 남은 재고 : -6
```

### 해결 방법

**synchronized 사용**

```java
public class Stock {
    static int stock = 1;

    public static void main(String[] args) throws InterruptedException {
        Runnable buyTask = () -> {
            synchronized (Stock.class) {
                if (stock > 0) {
                    stock--;
                    System.out.println(Thread.currentThread().getName() + " 구매 완료. 남은 재고 : " + stock);
                } else {
                    System.out.println(Thread.currentThread().getName() + " 구매 실패. 재고 없음");
                }
            }
        };

        for (int i = 0; i < 10; i++) {
            new Thread(buyTask, "사용자-" + i).start();
        }
    }
}
```

**위 코드 결과**

- 동시에 조건 검사와 감소가 일어나는 임계영역을 락으로 보호

```
사용자-0 구매 완료. 남은 재고 : 0
사용자-9 구매 실패. 재고 없음
사용자-8 구매 실패. 재고 없음
사용자-7 구매 실패. 재고 없음
사용자-6 구매 실패. 재고 없음
사용자-5 구매 실패. 재고 없음
사용자-4 구매 실패. 재고 없음
사용자-3 구매 실패. 재고 없음
사용자-2 구매 실패. 재고 없음
사용자-1 구매 실패. 재고 없음
```

**AtomicInteger 사용**

```java
import java.util.concurrent.atomic.AtomicInteger;

public class Stock {
    static AtomicInteger stock = new AtomicInteger(1);

    public static void main(String[] args) throws InterruptedException {
        Runnable buyTask = () -> {
            int currentStock = stock.get();
            if (currentStock > 0) {
                try {
                    Thread.sleep(1); // 일부러 delay → 경쟁상태 유도
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                boolean success = stock.compareAndSet(currentStock, currentStock - 1);
                if (success) {
                    System.out.println(Thread.currentThread().getName() + " 구매 성공! 남은 재고: " + stock.get());
                } else {
                    System.out.println(Thread.currentThread().getName() + " 구매 실패 (다른 스레드가 선점)");
                }
            } else {
                System.out.println(Thread.currentThread().getName() + " 구매 실패. 재고 없음");
            }
        };

        for (int i = 0; i < 10; i++) {
            new Thread(buyTask, "사용자-" + i).start();
        }
    }
}
```

**위 코드 결과**

- 오직 한 명만 재고를 가져가고, 나머지는 실패

```
사용자-5 구매 실패 (다른 스레드가 선점)
사용자-2 구매 실패 (다른 스레드가 선점)
사용자-9 구매 실패. 재고 없음
사용자-3 구매 실패 (다른 스레드가 선점)
사용자-4 구매 실패 (다른 스레드가 선점)
사용자-6 구매 실패 (다른 스레드가 선점)
사용자-0 구매 실패 (다른 스레드가 선점)
사용자-7 구매 실패 (다른 스레드가 선점)
사용자-8 구매 실패. 재고 없음
사용자-1 구매 성공! 남은 재고: 0
```

---

## ❓ 궁금한거

위의 방법은 DB 기반이 아닌 메모리 기반의 동기화로 이루어져 있는거 같은데 실무에 어떤식으로 쓰일까?

대부분 실무에선 DB기반으로 돌아가는데 이러한 방법들이 쓸모 있을까? -> `YES`

### 1️⃣ DB 쓰기 전에 한번 거르는 용도

**상황 예시**
> 쇼핑몰 서버에 1만명이 동시에 쿠폰 받기를 눌렀다고 가정한다
> 하지만 쿠폰 수량은 딱 100장만 있는 상태

예를 들어 이걸 그냥 다 DB에 insert 시도하게되면?
- 1만개 요청이 DB로 쏟아짐 -> DB 터짐
- 중복 지급 or 성능 저하 발생

**해결방법**

- 일단 메모리에서 먼저 카운팅하기

```java
AtomicInteger successCount = new AtomicInteger(0);

if (successCount.incrementAndGet() <= 100) {
    // 성공한 데이터만 DB에 저장
} else {
    // 실패 처리
}
```

### 2️⃣ 동시에 실행되면 안되는 내부 작업 보호

**상황 예시**
> 하루에 한번만 돌아야 하는 배치 작업

- 어떤 API나 스케줄러가 잘못 설정되면 같은 작업을 **2번 이상 동시에 실행**할 수 있음

**자바 메모리 락으로 보호**

```java
private final Lock lock = new ReentrantLock();

public void processBatch() {
    if (lock.tryLock()) {
        try {
            // 배치 작업 실행
        } finally {
            lock.unlock();
        }
    } else {
        // 이미 실행 중
    }
}
```

- 메모리 기반 락으로 동시 실행 방지
- 보통 내부 API 호출, 외부 연동 API에서 자주 사용됨 (중복 호출 되면 안되는 API에 해당)

### 3️⃣ 일시적인 상태를 메모리에서만 관리

**상황 예시**

> 관리자 페이지에서 잠깐만 락 걸기
> 어떤 항목 수정 시 다른 관리자가 동시에 수정 못하게
> DB까지 쓰기엔 오버스펙일때

```java
ConcurrentHashMap<Long, Boolean> editLocks = new ConcurrentHashMap<>();

if (editLocks.putIfAbsent(itemId, true) == null) {
    // 수정 가능
} else {
    // 누군가 수정 중
}
```

이런 잠깐 막기는 메모리에서 처리하는게 훨씬 빠르고 가볍고,

실무에서는 관리자 툴이나 백오피스에서 종종 등장한다.

---

## ⚠️ 참고할 내용

위와 같은 메모리 락은 단일 서버에서 사용하기 좋은 방법이므로 

멀티 서버에선 사용하면 안된다.

왜냐하면 서버간 공유가 안되기 때문이다.

간단한 예시로 생각하면 좋은 방법은 서버가 여러개라고 함은 프로세스가 여러개 있다고 생각하면 된다.

각 프로세스마다 스레드가 존재하며 각 프로세스에 있는 스레드가 공유가 안되는것과 같다고 생각하면 될 것같다.

**그럼 뭘 써야하나?**

보통 실무에선 같은 프로젝트를 여러개 띄워놓고 운영하는데 이땐 `Redis 락`의 동기화 방식을 사용하면 된다.
