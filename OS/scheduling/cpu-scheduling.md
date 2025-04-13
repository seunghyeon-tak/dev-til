# ⚙️ CPU 스케줄링 (CPU Scheduling)

📅 작성일: 2025-04-13  
📂 분류: OS / Scheduling  
🔖 태그: #스케줄링 #cpu-scheduling #운영체제 #프로세스관리 #알고리즘

---

## 🔍 요약

CPU 스케줄링은 **여러 프로세스 중 어떤 프로세스에 CPU를 할당할지 결정하는 정책**이다.

운영체제는 스케줄러를 통해 프로세스 상태를 관리하고, **CPU 효율을 극대화 하며 응답 시간과 대기 시간을 최소화** 하려한다.

---

## 💥 왜 중요한가요?

- 웹 서버나 백엔드 시스템은 여러 작업(요청)을 효율적으로 처리해야한다.
- 비효율적인 스케줄링은 **성능 저하**, **응답 지연**, **자원 낭비**로 이어짐

---

## ✅ 쉽게 예를 들어보기

- 은행에서 고객이 줄을 서고, 창구 직원이 한 명씩 처리하는 것과 유사
- 어떤 고객(프로세스)을 먼저 처리하느냐에 따라 전체 대기 시간과 만족도(성능)가 달라짐

---


## ✅ 주요 스케줄링 알고리즘

| 알고리즘 | 설명 | 장점 | 단점 |
|----------|------|------|------|
| FCFS (First-Come, First-Served) | 도착 순서대로 처리 | 구현 간단 | Convoy Effect 발생 |
| SJF (Shortest Job First) | 수행 시간이 짧은 작업 우선 | 평균 대기시간 최소화 | 실제 수행 시간 예측 어려움 |
| Priority Scheduling | 우선순위 높은 작업 우선 | 중요 작업 빠르게 처리 | Starvation 발생 가능 |
| Round Robin (RR) | 일정 시간 할당 후 순환 | 응답 시간 보장 | Context Switching 오버헤드 |
| MLFQ (Multi-Level Feedback Queue) | 다단계 큐 + 피드백 적용 | 다양한 프로세스 유형에 유연 | 설정 복잡 |

---

## 🧪 코드/흐름 예시 (Round Robin 간단 의사 코드)

```java
public class RoundRobin {
    public static void main(String[] args) {
        Queue<Process> queue = new LinkedList<>();
        queue.add(new Process("P1", 8));
        queue.add(new Process("P2", 4));
        queue.add(new Process("P3", 9));

        int timeQuantum = 4;

        while (!queue.isEmpty()) {
            Process p = queue.poll();
            p.run(timeQuantum);
            if (!p.isComplete()) {
                queue.add(p);
            } else {
                System.out.println(p.name + " 완료");
            }
        }
    }
}

class Process {
    String name;
    int remainingTime;

    public Process(String name, int remainingTime) {
        this.name = name;
        this.remainingTime = remainingTime;
    }

    public void run(int timeSlice) {
        int actualRunTime = Math.min(timeSlice, remainingTime);
        System.out.println(name + " 실행 중 (" + actualRunTime + "초)");
        remainingTime -= actualRunTime;
    }

    public boolean isComplete() {
        return remainingTime <= 0;
    }
}
```

---

## ⚠️ 주의할 점

- 어떤 환경에 어떤 알고리즘이 적합한지 판단할 수 있어야 함
    - 실시간 시스템 : Priority 기반
    - 서버 처리 : RoundRobin 또는 MLFQ
- Context Switching이 많은 경우 -> 성능 저하
- Starvation, Convoy Effect 등의 문제 상황도 함께 고려해야함