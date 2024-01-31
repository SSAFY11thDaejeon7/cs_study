# Process Synchronization (동기화)
- 다중 프로그래밍 시스템
  - 여러개의 프로세스들이 존재
  - 프로세스들은 서로 독립적으로 동작
  - 공유 자원 또는 데이터가 있을 때, 문제 발생 가능
 
- 동기화 (Synchronization)
  - 프로세스들이 서로 동작을 맞추는 것
  - 프로세스들이 서로 정보를 공유하는 것

- 비동기적 (Asynchronous)
  - 프로세스들이 서로에 대해 모름
- 병행적 (Concurrent)
  - 여러개의 프로세스들이 동시에 시스템에 존재
- 병행 수행적인 비동기적 프로세스들이 공유 자원에 동시 접근할 때 문제가 발생할 수 있음

- Shared data(공유 데이터): 여러 프로세스들이 공유하는 데이터
- Critical section(임계 영역): 공유 데이터를 접근하는 코드 영역
- Mutual exclusion(상호배제): 둘 이상의 프로세스가 동시에 임계 영역에 진입하는 것을 막는 것

- 기계어 명령의 특성
  - 원자성, 분리불가능
  - 한 기계어 명령의 실행 도중에 interrupt(방해) 받지 않음
 
- Race condition: 프로세스의 실행 순서에 따라 실행 결과가 달라질 수 있다.
- Race condition을 막아 원하는 결과를 보장하기 위한 것: **상호 배제**

## Mutual Exclusion (상호 배제)
### Methods
- Mutual exclusion primitives (기본 연산)
  - enterCS() primitive
    - Critical section 진입 전 검사
    - 다른 프로세스가 critical section 안에 있는지 검사
  - exitCS() primitive
    - Critical section을 벗어날 때의 후처리 과정
    - Critical section을 벗어남을 시스템이 알림
- 요구사항
  - 상호배제: 임계 영역에 프로세스가 있으면 다른 프로세스의 진입을 금지
  - 진행: 임계 영역 안에 있는 프로세스 외에는 다른 프로세스가 임계 영역에 진입하는 것을 방해하면 안됨
  - 한정 대기: 프로세스의 CS 진입은 유한시간 내에 허용되어야 함

- Dekker's Algorithm: Two process 상호 배제를 보장하는 최초의 알고리즘
  - turn과 flag를 통해 상호 배제를 보장함
- Peterson's Algorithm
  - Dekker 알고리즘보다 간단하게 구현됨

### N-Process 상호 배제
- 다익스트라 알고리즘
  - 최초로 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결
  - 실행 시간이 가장 짧은 프로세스에 프로세서를 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공

- SW solution들의 문제점
  - 속도가 느림
  - 구현이 복잡함
  - 상호배제 primitive 실행 중에 선점될 수 있음
    - 공유 데이터 수정중에 interrupt를 억제함으로써 해결 가능 -> overhead 발생
  - Busy Waiting 발생 (프로세스를 시작하지 못하는 상태)
<br>

# Hardware Solution

### TestAndSet(TAS) instruction
- Test와 Set을 한번에 수행하는 기계어
- 실행중 interrupt를 받지 않음
- 3개 이상의 프로세스의 경우, Bounded waiting 조건 위배
  - 이 경우, waiting 조건을 추가해서 대기 프로세스의 순서를 정리

- 하드웨어 솔루션의 장점: 구현이 간단하다
- 하드웨어 솔루션의 단점: Busy waiting 발생 가능
  - 이 문제를 해결하기 위해 OS의 solution 기법 사용

# OS Solution
## Spinlock
- 정수 변수
- 초기화 , P(), V() 연산으로만 접근 가능
  - 위 연산들은 indivisible ( or atomic) 연산
  - 중간에 선점되지않고 OS가 끝까지 실행될 수 있게 보장해줌
  - 전체가 한 instruction cycle에 수행됨
    
![7일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/aeb180d8-ea57-4ae6-a08d-42bf1324ea84)

- S: 물건의 개수 / P: 물건을 꺼내는 것 / V: 물건을 넣는 것
- 멀티 프로세서 시스템에서만 사용 가능
- 단점: Busy waiting 발생 가능

## Semaphore
- 1965sus 다익스트라가 제안
- Busy waiting 문제 해결
- 음이 아닌 정수형 변수(S)
  - 초기화 연산, P(), V()로만 접근 가능
    - P: Probern (검사)
    - V: Verhogen (증가)
- 임의의 s 변수 하나가 ready queue 하나가 할당됨

- Binary semaphore
  - S가 0과 1 두 종류의 값만 갖는 경우
  - 상호배제나 프로세스 동기화의 목적으로 사용
- Counting semaphore
  - S가 0이상의 정수값을 가질 수 있는 경우
  - 생산자-소비자 문제등을 해결하기 위해 사용

- 초기화 연산: S 변수에 초기값을 부여하는 연산
- P() 연산: 자물쇠를 거는 연산
- V() 연산: 자물쇠를 푸는 연산
- 물건이 없다면 대기실에서 대기하고, 일이 끝나면 깨워줌

![7일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/f0bfdf69-5b4a-421a-a43b-c50cffa69fd9)

- 모두 indivisible 연산
- OS support
- 전체가 한 instruction cycle에 수행됨

- Semaphore로 해결 가능한 동기화 문제들
  - 상효배제 문제
  - 프로세스 동기화 문제
  - 생산자-소비자 문제
  - Reader-writer 문제

- Reader-Writer preblem
  - Reader: 데이터에 대해 읽기 연산만 수행
  - Writer: 데이터에 대해 갱신 연산을 수행
  - 데이터 무결성 보장 필요
    - Reader들은 동시에 데이터 접근 가능
    - Writer들이 동시 데이터 접근시, 상호배제 필요
  - reader / writer에 대한 우선권 부여

- 장점: No busy waiting -> 기다려야하는 프로세스는 block(asleep)상태가 됨
- 단점: 큐에 대한 wake-up 순서는 비결정적이므로 기아 현상이 발생할 수 있음
  
## Eventcount / Sequencer
> 은행의 번호표와 비슷한 개념

- Semaphore의 기아 현상 문제를 해결할 수 있음
- Sequeancer (은행 번호표 기계)
  - 정수형 변수
  - 생성시 0으로 초기화, 감소하지 않음
  - 발생 사건들의 순서 유지
  - ticket() 연산으로만 접근 가능
- ticket(S)
  - 현재까지 ticket() 연산이 호출된 횟수를 반환
  - indivisible operation
- Eventcount (은행 번호 전광판)
  - 정수형 변수
  - 생성시 0으로 초기화, 감소하지 않음
  - 특정 사건의 발생 횟수를 기록
  - read(E), advance(E), await(E, v) 연산으로만 접근 가능
- read(E): 현재 Eventcount 값 반환
- advance(E): E <- E + 1, E를 기다리고 있는 프로세스를 깨움 (wake-up)
- await(E v)
  - v는 정수형 변수 (내 번호표)
  - if (E < v)이면 E에 연결된 Q에 프로세스 전달(PUSH) 및 CPUT scheduler 호출 (E는 은행 전광판에 있는 번호)
 
- Mutual exclusion
![8일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/e814da49-75a1-4c24-9162-3de79975577b)

- 처음에 v에 0을 반환, S는 0 -> 1
- E, v 둘 다 0이므로 기다리지않고 바로 임계영역에 감
- 다음에 v에 1을 반환, S는 1 -> 2
- E < v 이므로 대기실에서 기다리다가 앞 프로세스가 일을 끝내고 나와서 advance()하면 E는 1이 되고 E < v가 아니게 되어서 임계영역에 감

- No busy waiting
- No starvation
  - FIFO scheduling for Q
- Semaphore보다 더 low-level control이 가능

## High-level Mechanism
- Language-level constructs
- 사용이 쉬움
- Object-Oriented Concept와 유사함

### Monitor
- 공유 데이터와 Critical section의 집합 (최대 1개의 프로세스만 진입 가능)
- Conditional variable
  - wait(), signal() operations
![8일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/698abd60-f988-42be-8d55-a20dece972b9)

- Monitor의 구조
  - 진입 큐: 모니터 내의 procedure 수만큼 존재
  - Mutual exclusion
    - 모니터 내에는 항상 하나의 프로세스만 진입 가능
  - 정보 은폐
    - 공유 데이터는 모니터 내의 프로세스만 접근 가능
  - 조건 큐
    - 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
  - 신호제공자 큐 (전화 부스 같은 것)
    - 모니터에 항상 하나의 신호제공자 큐가 존재
    - signal() 명령을 실행한 프로세스가 임시 대기

- 자원 할당 문제
![8일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/6181610e-87e9-46c6-b322-4ceffc784c88)

- 책방과 같음
- releaseR: 반납하는 영역
- requestQ: 대출하는 영역
- R_Free: 책을 빌릴 수 있을 때 / codition queue: 책을 빌리기 위해 대기하는 공간
- signaler queue: 신호제공자 큐

- 자원 할당 시나리오
  - 자원 R 사용 가능
  - 프로세스Pj가 모니터 안에서 자원 R을 요청
  - 자원 R이 프로세스Pj에게 할당됨
  - 프로세스Pk, Pm이 condition_queue에서 R 요청
  - Pj가 큐의 앞에 있는 프로세스 Pk를 signaler 큐에서 awake()한 뒤, releaseR에서 남은 일을 하고 밖으로 나감
 
- Dining philosopher problem
  - 5명의 철학자가 원형 테이블에 앉아 있음
  - 철학자들은 생각하는 일, 스파게티 먹는 일만 반복함
  - 공유 자원: 스파게티, 포크
  - 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야 함
  - P:프로세스, 포크: 공유 데이터
![8일차-5](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/d101f4e8-f771-4635-a974-d6ebab1b074e)

![8일차-4](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/b64e33d4-dc78-4fae-b89b-16107c67d867)

- numForks: 철학자들이 사용 가능한 포크 수
- 음식을 다 먹으면 포크를 내리고 양쪽에 포크를 대기하고 있었으면 시그널을 주고 나옴

- Monitor의 장점
  - 사용이 쉽다
  - Deadlock등 error 발생 가능성이 낮음
- Monitor의 단점
  - 지원하는 언어에서만 사용 가능
  - 컴파일러가 OS를 이해하고 있어야 함
    - 임계 영역 접근을 위한 코드 생성
