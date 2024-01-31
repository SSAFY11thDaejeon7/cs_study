# 프로세스 동기화 & 상호배제
### Process Synchronization(동기화)
- 다중 프로그래밍 시스템
    - 여러 개의 프로세스들이 존재
    - 프로세스들은 서로 독립적으로 동작
    - 공유 자원 도는 데이터가 있을 때, 문제 발생 가능
- 동기화 (Synchronization)
    - 프로세스들이 서로 동작을 맞추는 것
    - 프로세스들이 정보를 공유하는 것

### Asynchronous and Concurrent
- 비동기적(Asynchronous)
    - 프로세스들이 서로에 대해 모름
- 병행적 (Concurrent)
    - 여러 개의 프로세스들이 동시에 시스템에 존재
- 병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근 할 때 문제가 발생 할 수 있음

### Terminologies
- Shared data(공유 데이터): 여러 프로세스들이 공유하는 데이터
- Critical section(임계 영역): 공유 데이터를 접근하는 코드 영역
- Mutual exclusion(상호배제): 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

### Critical Section (Example)

<img width="716" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/e05d4147-6b6f-49b2-9cb4-086aa1b92c0f">

<img width="689" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/dbdf1c8f-3375-46aa-bfd9-6616d4c91f00">

### Mutual Exclusion (상호배제)

<img width="712" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/3a746f77-b68b-49e2-bb7e-822a00f74808">

- Mutual Exclusion Primitives
    - enterCs() primitive
        - Critical Section 진입 전 검사
        - 다른 프로세스가 Critical Section 안에 있는지 검사
    - exitCS() primitive
        - Ciritical Section을 벗어날 때의 후처리 과정
        - Critical Section을 벗어남을 시스템이 알림
<img width="666" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/33e2dc6b-36e6-4619-ace5-b6c93ae12ff4">

### Requirements for ME primitives
- Mutual Exclusion (상호배제)
    - Critical Section(CS) 에 프로세스가 있으면, 다른 프로세스의 진입을 금지
- Progress (진행)
    - CS안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
- Bounded Wating (한정대기)
    - 프로세스의 CS진입은 유한시간 내에 허용되어야 함

### Two Process Mutual Exclusion
- ME Primitives V1
<img width="693" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/ab2b3e18-b61f-4f4d-a72e-b097acb36061">

- Progress 조건 위배
    - Why?
        - P0이 Critical Section에 진입 하지 않는 경우
        - 한 Process가 두 번 연속 CS에 진입 불가

- ME Primitives V2
<img width="747" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/44831151-cce3-47a0-b73c-d4f6ce2b9dd0">

- Mutual Exclusion 조건 위배
    - Why?
        - P0가 실행되기 위해 P1의 flag 확인
        - P0가 flag를 최신화 하기 전에 Preemption 될 경우 P1은 그 사이에 CS에 들어갈 수 있음
        - 뒤늦게 P0가 flag를 변경하지만 CS에는 두 프로세스가 존재

- ME Primitives V3
 <img width="748" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/f1838c31-ed15-41da-bbe6-59c528f8ecc2">

- Progress, Bounded waiting 조건 위배
    - Why?
        - P0이 flag를 최신화 후에 Preemption이 된 경우 P1은 flag를 최신화 후 while문 실행
        - P0이 다시 실행이 되지만 P1의 flag가 변경되어 두 프로세스 모두 CS에 접근 불가

# Mutual Exclusion Solution
- SW Solutions
    - Dekker’s algorithm (Peterson’s algorithm)
    - Dijkstra’s algorithm, Knuth’s algorithm, Eisenberg and McGuire’s algorithm, Lamport’s algorithm
- HW Solutions
    - TestAndSet(TAS) instruction
- OS supported SW Solution
    - Spinlock
    - Semaphore
    - Eventcount/sequencer
- Language-Level Solution
    - Monitor

### Dekker’s Algorithm
- Two Process ME을 보장하는 최초의 알고리즘
<img width="1174" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/14fe456e-e123-4086-8acb-fa014e15a5a0">

### Peterson’s Algorithm
- Dekker’s algorithm 보다 간단하게 구현
<img width="699" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/3fe20333-5e84-4a47-a63d-078b039a7704">


### Dijkstra’s Algorithm
- Dijkstra 알고리즘의 flag[] 변수
    - idle: 프로세스가 임게 지역 진입을 시도하고 있지 않을 때
    - want-in: 프로세스의 임게 지역 진입 시도 1단계 일 때
    - in-CS: 프로세스의 임게 지역 진입 시도 2단계 및 임계 지역 내에 있을 때
<img width="1268" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/6ecd029f-3d63-4a1b-b4be-adbf403702c5">

### SW Solutions들의 문제점
- 속도가 느림
- 구현이 복잡함
- ME primitve 실행 중 preemption될 수 있음
    - 공유 데이터 수정 중은 interrupt를 억제함으로서 해결 가능
    - Overhead 발생
- Busy waiting → Inefficient

# HW Solution
### TestAndSet (TAS) instruction

- Test와 Set을 한번에 수행하는 기계어
- Machine instruction
    - 실행 중, interrupt를 받지 않음 (preemption 되지 않음)
- Busy wating → inefficient

<img width="643" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/799e563a-8994-4da4-9d55-e8f2d6913d27">

### ME with TAS instruction
<img width="719" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/5f41ba66-c383-4c5b-8f4e-8ea505c0bc64">

- 3개 이상의 프로세스의 경우, Bounded wating 조건 위배
    - 대기 중인 프로세스의 순서대로 임계영역에 접근하는 것이 아니기 때문에 무한히 대기하게 될 수 있음

### N-Process Mutual Exclusion
 <img width="1057" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/78aae6f8-c81b-4f7f-8fb3-aaa444f7c454">

### HW Solution
- 장점: 구현이 간단
- 단점: Busy waiting
- Busy waiting 문제를 해소한 상호배제 기법: Semaphore

# OS Supported SW Solution
### Spinlock
- 정수형 변수
- 초기화, P(), V() 연산으로만 접근이 가능 (P: 자물쇠를 거는 것, V: 자물쇠를 푸는 것)
    - 위 연산들은 indivisible(or atomic) 연산
        - OS Support(보장)
        - 전체가 한 instruction cycle에 수행
- 단점: 멀티 프로세서 시스템에서만 사용이 가능, Busy waiting 문제 발생

<img width="688" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/a941f940-e2ee-4328-8e8f-30a801541713">

### Semaphore의 특징
- 1965년 Dijkstra 가 제안
- Busy waiting 문제 해결
- 음이 아닌 정수형 변수(S)
- 초기화 연산, P(), V()로만 접근 가능
- 임의의 S 변수 하나에 ready queue 하나가 할당 됨

### Binary Semaphore
- S가 0과 1 두 종류의 값만 갖는 경우
- 상호배제나 프로세스 동기화의 목적으로 사용

### Counting Semaphore
- S가 0 이상의 정수값을 가질 수 있는 경우
- Producer-Consumer 문제 등을 해결하기 위해 사용
    - 생산자 - 소비자 문제

### Semaphore
- 초기화 연산
    - S 변수에 초기값을 부여하는 연산
- P()연산, V()연산
- 모두 indivisible 연산
    - OS support
    - 전체가 한 instruction cycle 에 수행 됨

<img width="1080" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/c0d1a213-d43f-464a-b5d8-efd79a846200">

### Semaphore로 해결 가능한 동기화 문제들
- 상호배제 문제(Mutual Exclusion)
- 프로세스 동기화 문제(Process Synchronization Problem)
- 생산자-소비자 문제(Producer-Consumer Problem)
- Reader-Writer 문제
- Dining Philosopher Problem
- 기타

### Sempahore - Mutual Exclusion
<img width="689" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/920d2000-4620-44c9-95bd-6e9f0f4e442c">

### Semapohre - Process Synchronization
- Process들의 실행 순서 맞추기
    - 프로세스들은 병행적이며, 비동기적으로 수행

<img width="632" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/5a25accc-3ced-4ba4-885d-d004c32b43d0">

<img width="664" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/603c71a6-9d8d-4d09-8176-1bd7690e12f2">

### Semaphore - Producer-Consumer Problem
- 생산자(Producer) 프로세스: 메시지를 생성하는 프로세스 그룹
- 소비자(Consumer) 프로세스: 메시지를 전달받는 프로세스 그룹

<img width="1087" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/98e88870-ba07-4db8-81e9-50495789e43a">

### Producer-Consumer Problem with single buffer
<img width="567" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/3123e886-e817-4eb4-9a92-efffa1271338">

### Producer-Consumer Problem with N-buffers
<img width="1026" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/d057115e-2c26-489e-b706-0a28d963802a">

### Reader-Writer Problem
- Reader: 데이터에 대해 읽기 연산만 수행
- Writer: 데이터에 대해 갱신 연산을 수행
- 데이터 무결성 보장 필요
    - Reader들은 동시에 데이터 접근 가능
    - Writer들(또는 Reader와 Wrter)이 동시 데이터 접근 시 상호배제(동기화) 필요
- 해결법
    - Reader / Writer에 대해 우선권 부여
        - Reader Preference Solution
        - Writer Preference Solution

### Reader-Writer Problem (Reader Preference Solution)
<img width="1018" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/673f7466-ff85-4e4a-84e4-319a639dca0f">

### Semaphore의 장/단점
- 장점: No Busy waiting → 기다려야 하는 프로세스는 block(asleep) 상태가 됨
- 단점: Semaphore Queue에 대한 wake-up 순서가 비결정적 → Starvation Problem 발생 가능

# Eventcount / Sequencer
### Sequencer
- 정수형 변수
- 생성시 0으로 초기화, 감소하지 않음
- 발생 사건들의 순서 유지
- ticket() 연산으로만 접근 가능
    - ticket(S)
        - 현재까지 ticket()연산이 호출 된 횟수를 반환
        - Indivisible operation

### Eventcount
- 정수형 변수
- 생성시 0으로 초기화, 감소하지 않음
- 특정 사건의 발생 횟수를 기록
- read(E), advance(E), await(E, v) 연산으로만 접근 가능
    - read(E)
        - 현재 Eventcount 값 반환
    - advance(E)
        - E ← E + 1
        - E를 기다리고 있는 프로세스를 깨움
    - await(E, v)
        - V는 정수형 변수
        - if(E < v) 이면 E에 연결된 QE에 프로세스 전달(push) 및 CPU Scheduler 호출

<img width="647" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/b8f72b9a-db01-43d5-8d6e-6a8643fa328e">

### Producer-Consumer Problem
<img width="1218" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/380a61ad-e548-460f-98fd-171dfad8ab8f">

### Eventcount / Sequencer 특징
- No Busy waiting
- No Starvation
- Semaphore 보다 더 low-level control이 가능

# Language-Level Solution
### High-Level Mechanism
- Language-level Constructs
- Object-Oriented Concept 와 유사
- 사용이 쉬움

### Monitor
- 공유 데이터와 Critical Section의 집합
- Conditional Variable
    - wait(), signal() operations

<img width="969" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/bad3e57f-64be-49e7-822a-e4ba9dc0ce15">

### Monitor의 구조
- Entry Queue(진입 큐)
    - 모니터 내의 procedure 수만큼 존재
- Mutual Exclusion
    - 모니터 내에는 항상 하나의 프로세스만 진입 가능
- Information Hiding(정보 은폐)
    - 공유 데이터는 모니터 내의 프로세스만 접근 가능
- Condition Queue(조건 큐)
    - 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
- Signaler Queue(신호제공자 큐)
    - 모니터에 항상 하나의 신호제공자 큐가 존재
    - signal() 명령을 실행한 프로세스가 임시 대기

### 자원 할당 문제
<img width="991" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/d2d27949-6234-48c6-8676-36d2e0b61890">

<img width="407" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/fed61665-6fe4-44fe-b617-52e12a676695">

### Monitor - Producer-Consumer Problem
<img width="611" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/754cbc87-3782-42f3-bbeb-956e069067d9">

### Monitor - Reader-Writer Problem
- reader/writer 프로세스들간의 데이터 무결성 보장 기법
- writer 프로세스에 의한 데이터 접근 시에만 상호 배제 및 동기화 필요함
- 모니터 구성
    - 변수 2개
        - 현재 읽기 작업을 진행하고 있는 reader 프로세스의 수
        - 현재 writer 프로세스가 쓰기 작업을 진행 중인지 표시
    - 조건 큐 2개
        - reader/writer 프로세스가 대기해야 할 경우에 사용
    - 프로시져 4개
        - reader(writer) 프로세스가 읽기(쓰기) 작업을 원할 경우에 호출, 읽기(쓰기) 작업을 마쳤을 때 호출

<img width="998" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/9b343c13-750e-48c5-b10f-ef07d9168cbb">

### Dining Philosopher Problem
- 5명의 철학자
- 철학자들은 생각하는 일, 스파게티 먹는 일만 반복함
- 공유 자원: 스파게티, 포크
- 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야함

<img width="706" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/56572c7e-4d08-4626-8674-917bf1724bf0">

### Monitor 장/단점
- 장점
    - 사용이 쉽다
    - Deadlock 등의 error 발생 가능성이 낮음
- 단점
    - 지원하는 언어에서만 사용 가능
    - 컴파일러가 OS를 이해하고 있어야 함
        - Critical Section 접근을 위한 코드 생성
