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
        - 임계 영역은 비어있지만 turn값이 0이라 P0 수행 불가
    

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
