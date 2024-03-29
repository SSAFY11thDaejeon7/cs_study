# 1. 동기화

### 동기화란?

- 다중 프로그래밍 시스템
    - 여러 개의 프로세스들이 존재한다.
    - 프로세스들은 서로 독립적으로 동작한다.
    - 공유 자원 또는 데이터가 있을 때 문제가 발생할 수 있다!
- 동기화
    - 프로세스들이 서로 동작을 맞추는 것
    - 프로세스들이 서로 정보를 공유 하는 것 (서로 합의 / 대화를 나누는 것)

### Asynchronous and Concurrent P’s

- 비동기적: 프로세스들이 서로에 대해 모름
- 병행적: 여러 개의 프로세스들이 동시에 시스템에 존재 (Concurrent)
- 병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근 할 때 문제가 발생 할 수 있음

### Terminologies

- Shared Data ( = Critical Data, 공유 데이터)
    - 여러 프로세스들이 공유하는 데이터
    - 동시에 건들이면 큰 문제가 되는 데이터
- Critical Section (임계 영역)
    - 공유 데이터를 접근하는 코드 영역
- Mutual Exclusion (상호 배제)
    - 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

> **기계어 멍령 (Machine instruction)의 특성** 
- Atomicity(원자성), indivisible(분리불가능)
- 한 기계어 명령의 실행 도중에 인터럽트 받지 않음
> 

CPU의 작업은 레지스터에서만 작업한다

메모리의 프로그램을 레지스터에서 작동한 후, 다시 메모리에 쓴다.

CPU를 할당받았다가, (Running) 중간에  빼앗길 때, (Preemption) 다른 프로세스가 일을 처리하는 경우, 원하는 값과 다른 결과가 나올 수 있다.. (값이 보장되지 않는 문제가 있음 - Race Condition)

### Mutual Exclusion Methods

- Mutual Exclusion: 상호배제, 한 프로세스가 임계 영역에 있는 경우, 다른 프로세스가 들어오지 못하게 하는 것
- Primitives (가장 기본이 되는 연산 - Mutual Exclusion을 달성하기 위한 기본 연산)
    - **enterCS() primitive**
        - Critical Section 진입 전 검사
        - 다른 프로세스가 critical section 안에 있는지 검사한다.
    - **exitCS() primitive**
        - Critical Section을 벗어날 때의 후처리 과정
        - Critical Section을 벗어남을 시스템이 알림
    ![criticalSection](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/a127dc2b-bdd5-42ce-814d-49ede4cf8fc9)

    
- Requirements for ME Primitives
    - Mutual Exclusion (상호배제)
        - Critical Section에 프로세스가 있으면, 다른 프로세스의 진입을 금지한다.
    - Progress (진행)
        - CS안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해 하면 안된다.
        - CS에 들어가 있지도 않으면 방해x
    - Bounded waiting (한정 대기)
        - 프로세스의 CS 진입은 유한 시간 내에 허용되어야 함
    ![MEPrimitiveVer1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/c6f1e738-23ad-4b48-82fa-5771c191dec9)

    
    - [v1. 내 turn일때만 들어가는 방법] → Progress의 조건을 위배한다
        - P0이 critical section에 진입하지 않는 경우, 또는 실행되다 죽어버리는 경우, 다른 프로세스 못들어감
        - 한 Process가 두 번 연속 cs에 진입이 불가능함
    ![MEPrimitieVer2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/5872e926-569b-40ed-a719-524f8fc29c82)

    
    - [v2. 들어가서 깃발을 드는 방법] → Mutual Exclusion 위배
        - 들어가기 직전에 깃발을 드는 것이 문제
    ![MEPrimitiveVer3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/b0f1f1b8-0e43-4950-a6a3-4d533530d646)

    
    - [v3. 들어가기 전에 깃발을 드는 방법] → Progress, Bounded waiting 조건 위배
        - 하나의 프로세스가 깃발을 들고 진행하고 있다가 Preeptive되었을 때, 다른 프로세스도 깃발을 든다면 무한 대기
    
    # 2. Mutual Exclusion Solution
    
    ### SW Solution - Dekker’s Algorithm
    
    
![TwoMEAlgorithm](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/213c02f8-b5ce-41ab-b750-0337600ccfbe)

### SW Solution - Peterson’s Algorithm
![Peterson'sAlgorithm](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/e49900a0-fee0-4efb-8ec2-fe765f667c24)


### SW Solution - Dijkstra’s Algorithm

n-process Mutual Exclusion 해결 방안

- Dijkstra 알고리즘의 flag[] 변수
    - idle:프로세스가 임계 지역 진입을 시도하고 있지 않을 때
    - want-in: 프로세서의 임계 지역 진입 시도 1단계일때
    - in-CS: 프로세스의 임계 지역 진입 시도 2단계 및 임계 지역 내에 있을 때

![Dijkstra'sAlgorithm](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/619abc9f-4d8f-467c-b2f8-e741061d7e6a)

### SW Solution들의 문제점

- 속도가 느림
- 구현이 복잡함
- ME Primitive 실행 중에도 Preeptive 될 수 있음
- 공유 데이터 수정 중은 interrupt를 억제할 수 있지만, overhead 발생
- busy wait (기다리는 중에도 while문을 돈다 - 비효율적임)




# 2. Mutual Exclusion Solution - HW Solution

### TestAndSet(TAS) instruction

- Test와 Set을 한 번에 수행하는 기계어
- Machine Instruction
    - Atomicity, Indivisible
    - **실행 중 interrupt를 받지 않는 것이 보장됨 (Preemption 되지 않음) // 한번에 실행하는 것을 보장**
    - Busy Waiting → Inefficient
<img width="479" alt="TAS" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/26564e29-5f0f-40a9-adae-049bf20fa91f">

- 3개 이상의 프로세스의 경우 Bounded Waiting 조건 위배
    - 1번 나올때 3번, 3번 나올때 4번이 들어가면 2번은 들어갈 수 없음
<img width="449" alt="n-process" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/86b9423f-6c29-428f-93b6-8479a38a04ba">


### HW Solution 장단점

- 장점: 구현이 간단하다
- 단점: Busy Waiting
- Busy Waiting 문제를 해소한 상호배제 기법
    
    → Semaphore: 대부분의 OS들이 사용
    

# 3. Mutual Exclusion Solution - OS Supported SW Solutions

### Spinlock

> P, V를 보장하지만 Busy Waiting..
> 
- 정수형 변수
- 초기화, P(), V() 연산으로만 접근이 가능하다
    - 위 연산들은 indivisible(or atomic) 연산
    - OS support
    - 전체가 한 instruction cycle에 수행됨
    - P는 물건을 꺼내고, V는 물건을 집어넣는다.

```java
P(S) { // 해당 과정은 한번에 처리되는 것을 보장
	while (S <= 0) do
	endwhile;
	S <- S - 1;
}
```

```java
V(S) { // 해당 과정은 한번에 처리되는 것을 보장
	S <- S + 1;
}
```
<img width="478" alt="spinlock" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/c73b74b9-c04d-4674-8a1d-7185a6d7cfdf">


### Spinlock 문제점

- 멀티 프로세서 시스템에서만 사용 가능 (CPU 2개 이상)
- Busy Waiting (While문에서 뱅뱅 돌고있음..)

### Semaphore

- 1965년 Dijkstra가 제안
- Busy Waiting 문제 해결
- 음이 아닌 정수형 변수(S)
    - 초기화 연산, P(), V()로만 접근 가능
    - P: Probern(검사), V: Verhogen(증가)
    - S는 물건의 개수라고 생각하기

- **임의의 S 변수 하나에 ready queue 하나가 할당됨!**

### Semaphore의 분류

- Binary Semaphore
    - S가 0과 1 두 종류의 값만 갖는 경우
    - 상호배제나 프로세스 동기화의 목적으로 사용
- Counting Semaphore
    - S가 0이상의 정수값을 가질 수 있는 경우
    - Producer-Consumer 문제 등을 해결하기 위해 사용
        - 생산자 - 소비자 문제

### Semaphore 연산자

- 초기화 연산 : S 변수에 초기값을 부여하는 연산
- P()연산, V()연산
![semaphore1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/b5381954-63b0-48a0-a003-6d0836441c12)


- 모두 indivisible 연산
    - OS support
    - 전체가 한 instruction cycle에 수행 됨
![semaphore2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/91a35be1-3480-46bd-91b0-ed4208bd5153)


### Process Synchronization 문제 Semaphore로 해결하기

- 프로세스들의 실행 순서 맞추기
- 프로세스들은 병행적이며, 비동기적으로 수행한다.
![semaphore3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/6b3c92c2-f36f-4b52-9743-db36b4283ed0)


### Producer-Consumer Problem

- 생산자 프로세스: 메시지를 생성하는 프로세스 그룹
- 소비자 프로세스; 메세지 전달받는 프로세스 그룹
- “동기화 필요’ : 생산 중일때 소비하면 안되고, 다른 생산자가 생산중일때 생산해도 안된다.
    - Producer-Consumer Problem with single buffer
    ![semaphore4](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/bc862f1e-c8fd-44a2-b3dc-c7c8124dc658)

    
    ```java
    소비 하였는가? -> 1
    생산 하였는가? -> 0
    ```
    
    - Producer-Consumer Problem with N buffer
    ![semaphore5](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/05113c41-b95d-469f-adbb-23f906d719e6)

    
- Reader-Writer Problem
    - Reader: 데이터에 대해 읽기 연산만 수행
    - Writer: 데이터에 대해 갱신 연산을 수행
- 데이터 무결성 보장 필요
    - Reader들은 동시에 데이터 접근 가능
    - Writer들이 동시 데이터 접근 시, 상호배제 필요
- 해결법
    - reader/writer에 대한 우선권 부여
        - reader preference solution
        - writer preference solution

### Semaphore 장단점

- No busy waiting; 기다려야 하는 프로세스는 block 상태가 됨
- Semaphore queue에 대한 wake-up 순서는 비결정적임 ; starvation problem

### Eventcount / Sequencer

- 은행의 번호표와 비슷한 개념
- **Sequencer**
    - 정수형 번호
    - 생성 시 0으로 초기화, 감소하지 않음
    - 발생 사건들의 순서 유지
    - ticket() 연산으로만 접근 가능
- **ticket(S)**
    - 현재까지 ticket() 연산이 호출 된 횟수를 반환
- **EventCount**
    - 정수형 변수
    - 생성 시 0으로 초기화, 감소하지 않음
    - 특정 사건으로 발생 횟수를 기록
    - read(E), advance(E), await(E, v) 연산으로만 접근 가능
- **read(E)**
    - 현재 **EventCount**값 반환
- **advance(E)**
    - E ← E+1
    - E를 기다리고 있는 프로세스를 깨움 (wake-up)
- **await(E, v)**
    - V는 정수형 변수
    - if (E < v)이면 E에 연결된 Qe에 프로세스 전달 (push) 및 CPU scheduler 호출
- 장단점
    - No busy waiting
    - No startvation
    - Semaphore 보다 더 low-level control이 가능

# 4. Language-Level solution - Monitor

- Monitor
- Flexible
- Difficult to use
    - Error-prone

### High-level Mechanism

- Monitor
    - 공유 데이터와 Critical section의 집합
    - conditional variable
    - wait(), signal() operations
- Monitor의 구조
    - Entry queue
    - Mutual exclusion
    - information hiding(정보 은폐)
    - condition queue(조건 큐)
    - signaler queue (신호제공자 큐)
- 자원할당 문제
    - 책을 대출, 반납하는 구조
- Producer - Consumer Problem
- 장단점
    - 장점: 사용이 쉽다,  Deadlock 등 error 발생 가능성이 낮다
    - 단점: 지원하는 언어에서만 사용이 가능하다, 컴파일러가 os를 이해하고 있어야한다
