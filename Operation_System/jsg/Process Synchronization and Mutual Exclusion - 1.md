# Process Synchronization

- 다중 프로그래밍 시스템
  - 여러 개의 프로세스들이 존재
  - 프로세스들은 서로 독립적으로 동작
  - 공유 자원 또는 데이터가 있을 때, 문제 발생 가능
- 동기화(Synchronization)
  - 프로세스들이 서로 동작을 맞추는 것
  - 프로세스들이 서로 정보를 공유하는 것

- 비동기적(Asynchronous)
  - 프로세스들이 서로에 대해 모름
- 병행적(Concurrent)
  - 여러 개의 프로세스들이 동시에 시스템에 존재
- **병행 수행중인 비동기적 프로세스들이 공유자원에 동시 접근할때 문제가 발생할 수 있음**

### Terminologies
- machin instruction(기계어 명령)의 특성
  - Atomicity(원자성), Indivisible(분리 불가능)
  - 한 기계어 명령의 실행 도중에 인터럽트 받지 않음(방해 불가)
#### Shared data(공유데이터)
- 여러 프로세스들이 공유하는 데이터
#### Critical section(임계영역)
- 공유 데이터를 접근하는 코드 영역(code segment)
<img width="667" alt="스크린샷 2024-01-26 오후 9 38 16" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/49d28ecd-68b3-4223-9045-7aa5f513dd36">

- 결과 값이 하나로 보장이 되지 않고 실행 순서에 따라 값이 달라지는 Race condition 현상
#### Mutual exclusion(상호배제)
<img width="665" alt="스크린샷 2024-01-26 오후 9 43 15" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/36a9e567-c9fa-40c3-b9f3-05e0b2add678">

- 둘 이상의 프로세스가 동시에 critical secton에 진입하는 것을 막는 것
##### Mutual Exclusion Methods
- Mutual exclusion primitives
  - enterCS() primitive **primitives : 가장 기본이 되는 연산**
    - Critical section 진입 전 검사
    - 다른 프로세스가 critical section 안에 있는지 검사
  - exitCS() primitive
    - Critical section을 벗어날 때의 후처리 과정
    - Critiacl section을 벗어남을 시스템이 알림
##### Requirements for ME primitives
- Mutual exclusion(상호배제)  
  - Critical section에 프로세스가 있으면, 다른 프로세스의 진입을 금지
- Progress(진행)
  - CS 안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
- Bounded waiting(한정대기)
  - 프로세스의 cs진입은 유한시간 내에 허용되어야 함

### Dekker's Algorithm
<img width="641" alt="스크린샷 2024-01-26 오후 10 06 17" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/2a520162-c078-4b0e-a9db-f7fc5e0e75f9">

- Two process ME를 보장하는 최초의 알고리즘
- turn과 flag를 모두 다 둔 케이스
- 서로의 깃발을 확인하고 추가로 turn 값을 통해 자신의 턴인지 확인한다.

### Peterson's Algorithm
<img width="604" alt="스크린샷 2024-01-26 오후 10 10 01" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/9a8a2c27-8c8b-4aad-b48d-8a703a61d0f5">

- Dekker's algorithm 보다 간단하게 구현
- 서로에게 턴을 양보하는 로직이 들어감

### Dijkstra's Algorithm(다익스트라 알고리즘)
<img width="627" alt="스크린샷 2024-01-26 오후 10 13 03" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/20783f8e-c030-4185-b59e-5ae246dd9bef">

- 최초의 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결
- 실행 시간이 가장 짧은 프로세스에 프로세서 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공
<img width="703" alt="스크린샷 2024-01-26 오후 10 12 21" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/c38991bd-5c8f-4830-b776-767a6fffada5">

### SW Solution
- Sw solution들의 문제점
  - 속도가 느림
  - 구현이 복잡함
  - ME primitive 실행 중 preemption 될 수 있음
  - Busy waiting 
