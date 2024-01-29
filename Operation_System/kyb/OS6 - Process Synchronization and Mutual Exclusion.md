## Process Synchronization(동기화)

- 프로세스들이 서로 동작을 맞추는 것
- 프로세스들이 서로 정보를 공유하는 것

동기화의 이유

병행 수행 중인 비동기적 프로세스들이 공유 자원에 동시 접근할 때, 문제가 생길 수 있음

- Shared data(공유 데이터): 여러 프로세스들이 공유하는 데이터
- Critical section(임계 영역): 공유 데이터를 접근하는 코드 영역
- Mutual exclusion(상호 배제): 둘 이상의 프로세스가 동시에 임계 영역에 진입하는 것을 막는 것

**ex) 두 프로세스가 같은 자원에 동시 접근하는 상황**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/0daa1d35-cdb9-418c-b88a-cd1ec5ca7f36)


수행 과정(순서)에 따라, 결과값 차이(Race condition) 발생

- 1 → 2 → 3 → A → B → C 혹은 A → B → C → 1 → 2 → 3의 순서로 진행되는 경우
    - sdata = 2
- 1 → 2 → A → B → C → 3의 순서로 진행되는 경우
    - sdata = 1
    

## (Mutual exclusion)상호 배제

한 프로세스가 임계 영역에서 작업하는 동안, 다른 프로세스가 못하도록 막아주는 것

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/ef11c84a-7b39-4973-98b7-0c866b0fbbc1)


### 상호 배제 메서드

- Mutual exclusion primitives
    - enterCS(): 임계 영역 진입 전 검사
        - 다른 프로세스가 임계 영역 안에 있는지 검사
    - exitCS(): 임계 영역을 벗어날 때의 후처리 과정
        - 임계 영역을 벗어남을 시스템이 알림
        
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/c09e3a3f-7810-4d41-a819-29447519795e)

        
- ME primitives를 위한 요구 사항
    - Mutual exclusion(상호 배제): 임계 영역에 프로세스가 있으면, 다른 프로세스의 진입을 금지
    - Progress(진행): 임계 영역 안에 있는 프로세스 외에는, 임계 영역에 다른 프로세스가 진입하는 것을 방해하면 안됨
    - Bounded waiting(한정 대기): 프로세스의 임계 영역 진입은, 유한 시간 내에 허용되어야 함

### 상호 배제  V1

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/cfb95efd-828e-45c3-98d9-2fe4f328e591)


예상되는 문제(Progress 조건 위배)

- turn이 0일 때, P0이 작업을 완료하지 못하고 종료되는 경우
    - turn 값이 변경되지 않아 P1은 영영 실행되지 못함
- 두 프로세스의 도착 시간 차이가 발생하는 경우
    - P0 작업 완료 → 재작업 수행 전까지, P1이 수행되지 않은 경우
        - 임계 영역은 비어있지만 turn값이 1이라 P0 수행 불가
    

### 상호 배제 V2

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/64cb90bc-554e-48ff-985f-4e4603d47f68)


예상되는 문제(Mutual Exclusion 조건 위배)

- ①에서 preemption이 발생한 경우
    - P1 임계 영역 진입 → P0 작업 재수행 → P0 임계 영역 진입
        - 임계 영역에서 두 프로세스가 동시 수행

### 상호 배제 V3

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/b44fc769-20e1-4424-8928-23664a0e0269)


예상되는 문제(Progress, waiting 조건 위배)

- ②에서 preemption이 발생하는 경우
    - flag[0]: true → preemption → flag[1]: true → flag[0]: false까지 대기 → P0 작업 재수행 → flag [1]: false까지 대기
        - 임계 영역은 비어있지만, 두 프로세스가 서로의 flag에 의해 진입 불가

## Mutual Exclusion Solutions

- SW Solutions
    - Dekker’s algorithm(Perterson’s algorithm)
    - Dijkstra’s algorithm
- HW Solutions
    - TestAndSet(TAS) instruction
- OS supported SW solution
    - Spinlock
    - Semaphore
    - Eventcount / sequencer
- Language-Level solution
    - Monitor

### Dekker’s Algorithm

- Two process ME를 보장하는 최초의 알고리즘

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/e6914145-d37e-4c83-b3d8-886f67814d1b)


- flag와 turn을 같이 사용
- 두 flag가 모두 true여도, turn을 통해 순서를 부여하여 순차 처리

### Peterson’s Algorithm

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/9bb03044-f688-4af4-8153-ac0ecfb7abb0)


- Dekker’s Algorithm보다 간단하게 구현
- 두 프로세스가 서로 turn을 양보 → 나중에 양보한 프로세스를 늦게 처리

### Dijkstra’s Algorithm

**flag[] 변수**
    
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/a26babd2-5f35-4973-a8aa-32b112a942b4)

    

**작업**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/994c6ac2-6179-41ea-aeda-21f81dbac612)


- 1단계
    - flag: want-in → 내 turn이 아니라면, 현재 turn이 끝날 때 까지 대기 → turn을 나에게 할당
- 2단계
    - flag: in-CS → while문 수행 → in-CS 상태인 flag가 자신밖에 없다면 임계 영역 진입, 아니라면 1단계로 복귀
- 장점: Mutual exclusion, Progress, Bounded waiting 문제를 모두 해결
- 단점: 조건을 만족할 때 까지 진입 시도를 반복(Busy waiting)

## SW solution들의 문제점

- 속도가 느림
- 구현이 복잡
- ME primitive 실행 중 preemption 될 수 있음
    - 공유 데이터 수정 중은 interrupt를 억제 함으로서 해결 가능
        - overhead 발생
- Busy waiting

## HW Solution

### TestAndSet(TAS) instruction

- Test와 Set을 한번에 수행하는  기계어
- Machine instruction(전체 작업을 한번에 수행)
    - 실행 중 interrupt를 받지 않음(preemption 되지 않음)
- 3개 이상의 프로세스의 경우, Bounded waiting 조건 위배
- 장점: 구현이 간단
- 단점: Busy waiting

## OS supported SW solution

### Spinlock

- 정수 변수
- 초기화, P(), V() 연산으로만 접근 가능
    - indivisible(ot atomic) 연산
        - 실행되는 동안 선점 되지 않음
        - 전체가 한 instruction cycle에 수행 됨

### Spinlock 예제

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/1d0439d5-3cab-4753-894f-bc0bbb20ff6f)


- 임계 영역에 자원 존재(active: 1) → P(locking) 수행 후 임계 영역 내에서 작업(active: 0) → 작업 종료 후 V(unlocking) 수행(active: 1)
- 위 과정 중 다른 프로세스는 active가 1이 될 때까지 대기, 자원이 반납 되어 active: 1이 될 경우 작업 진행
- 멀티 프로세서 시스템에서만 사용 가능
- Busy waiting

### Semaphore

- Dijkstra가 제안
- Busy waiting 문제 해결(No busy waiting)
    - 기다려야 하는 프로세스는 block(asleep)상태가 됨
- 음이 아닌 정수형 변수(S)
    - 초기화 연산, P(), V()로만 접근 가능
- 임의의 S 변수 하나에 ready queue가 할당 됨
- 단점: Semaphore queue에 대한 wake-up 순서는 비 결정적
    - 기아 현상 야기

- Binary semaphore
    - S가 0과 1 두 종류의 값만 갖는 경우
    - 상호 배제나 프로세스 동기화의 목적으로 사용
- Counting semaphore
    - S가 0 이상의 정수 값을 가질 수 있는 경우
    - 생산자-소비자 문제 등을 해결하기 위해 사용

### Spinlock과의 차이

- P(S)
    - Spinlock: 자원(S)이 없는 경우, 자원이 반납될 때 까지 반복 수행
    - Semaphore: 자원(S)이 없는 경우, 해당 자원의 ready queue에서 대기
- V(S)
    - Spinlock: 자원 사용 후 반납, 이후 반복 대기 중인 프로세스가 작업 수행
    - Semaphore: 자원 사용 후, ready queue에 프로세스가 있다면 해당 프로세스를 wakeup 후 자원 반납

### Semaphore로 해결 가능한 동기화 문제들

- 상호 배제 문제
- 프로세스 동기화 문제
- 생산자-소비자 문제
- Reader-writer 문제
- 식사하는 철학자들 문제

### Reader-Writer 문제

- Reader는 읽기 연산만 수행
- Writer는 갱신 연산을 수행
- 데이터 무결성 보장 필요
    - Reader들은 동시에 데이터 접근 가능
    - Writer들(또는 reader와 write)이 동시 데이터 접근 시, 상호 배제(동기화) 필요
- 해결법
    - reader / writer에 대한 우선권 부여
