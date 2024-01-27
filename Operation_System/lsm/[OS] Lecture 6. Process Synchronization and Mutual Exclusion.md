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
