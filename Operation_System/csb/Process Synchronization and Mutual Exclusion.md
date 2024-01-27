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
