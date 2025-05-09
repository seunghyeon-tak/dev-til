# 🗒️ 메모리 관리 - 페이징(Paging) & 세그멘테이션(Segmentation)

📅 작성일: 2025-04-26
📂 분류: OS / memory-management
🔖 태그: #페이징 #세그멘테이션 #메모리관리 #운영체제 #가상메모리

---

## 🔍 왜 메모리를 잘 나눠야 할까?

컴퓨터는 여러 프로그램을 동시에 실행해야 한다.

그런데 메모리는 한정되어있어서, 운영체제가 어떻게 나눠줘야 효율적일까? 고민하게 된다.

그래서 나온 방식이 바로 페이징과 세그멘테이션이다.

---

## 📦 페이징(Paging): 같은 크기로 조각 내서 퍼즐처럼 끼워 넣기

### 📘 쉽게 말하면:

- 운영체제가 메모리를 일정한 크기로 잘라놓는다.
- 프로그램도 똑같이 자른다.
- 퍼즐 맞추듯이 빈칸에 끼워넣는다.

### ✅ 장점:

- 크기가 같아서 아무데나 끼워 넣을 수 있다.
    -> 메모리 낭비가 작다.

### ⚠️ 단점:

- 퍼즐 맞추려면 어디에 뒀는지 기억해야 하니깐
    -> 추가 정보(페이지 테이블)가 필요하다.

---

## 🧩 세그멘테이션(Segmentation): 프로그램 구조대로 덩어리 나누기

### 📘 쉽게 말하면:

- 코드, 데이터, 스택 같은 논리적인 단위로 나눠서 메모리에 올린다.
- 덩어리마다 크기가 다르다.

### ✅ 장점:

- 프로그램 구조랑 딱 맞아서 보기 쉽고 이해하기 편하다.

### ⚠️ 단점:

- 덩어리 크기가 다르다 보니
    -> 중간에 애매하게 남는 공간(외부 단편화)이 생길 수 있다.

---

## 🔄 페이징 vs 세그멘테이션 요약 비교

|구분|	페이징|	세그멘테이션|
|---|-------|---------|
|나누는 기준|	고정된 크기로 나눔|	의미 있는 덩어리로 나눔|
|크기|	똑같음 (예: 4KB씩)|	다 다름 (코드, 데이터 등)|
|장점|	아무 데나 끼워 넣기 가능|	이해하기 쉬움, 구조 반영|
|단점|	퍼즐 조각 관리 필요|	빈 공간이 생길 수 있음|

---

## 💬 현실에서 어떻게 쓰이나?

실제 운영체제에서는 두 가지를 섞어서 쓴다.
- 메모리를 퍼즐처럼 잘게 쪼개기도 하고(페이징)
- 논리 구조도 고려해서 구억 나누기도 한다. (세그멘테이션)

---

## 💡 핵심 정리

- 페이징 : 메모리를 똑같은 크기로 나눠서 퍼즐처럼 끼워 넣는 방식
- 세그멘테이션 : 코드/데이터/스택 처럼 의미 있는 단위로 메모리를 나누는 방식

-> 둘 다 효율적으로 메모리를 나누기 위한 운영체제 전략이다.