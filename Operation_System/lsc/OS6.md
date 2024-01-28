# [OS] Lecture 6. Process Synchronization and Mutual Exclusion

## Process Synchronization (동기화)
- 다중 프로그래밍 시스템
  - 여러 개의 프로세스들이 존재
  - 프로세스들은 서로 독립적으로 동작
  - 공유 자원 또는 데이터가 있을 때, 문제 발생 가능

- 동기화(Synchronization)
  - 프로세스들이 서로 동작을 맞추는 것
  - 프로세스들이 서로 정보를 공유하는 것

## Asynchronous and Concurrent P's
- 비동기적(Asynchronous)
  - 프로세스들이 서로에 대해 모름
- 병행적(Concurrent)
  - 여러 개의 프로세스 들이 동시에 시스템에 존재
- 병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근할 때 문제가 발생할 수 있음

## Terminologies
- 공유 데이터(Shared or Critical data)
  - 여러 프로세스들이 공유하는 데이터
- 임계 영역(Critical section)
  - 공유 데이터를 접근하는 코드 영역(code segment)
- 상호배제(Mutual exclusion)
  - 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/6e00364d-dc36-4bd5-8d20-1cc8289e7469)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/15b148d7-1490-46c4-99f2-cff87a2c45fc)


## Mutual Exclusion Methods
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/b86cb235-fcd6-4555-8c7a-8ecdef347db8)

- Mutual exclusion primitives
  - enterCS() primitive
    - Critical section 진입 전 검사
    - 다른 프로세스가 critical section 안에 있는지 검사
  - exitCS() primitive
    - Critical section을 벗어날 때의 후처리 과정
    - Critical section을 벗어남을 시스템이 알림
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e1411e83-e678-47d7-86fa-80e3756a623c)


## Requirements for ME primitives
- Mutual exclusion(상호배제)
  - Critical section(CS)에 프로세스가 있으면, 다른 프로세스의 진입을 금지
- Progress(진행)
  - CS 안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
- Bounded waiting(한정대기)
  - 프로세스의 CS 진입은 유한시간 내에 허용되어야 함
    
## Two Process Mutual Exclusion
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/ce1a1c86-25cf-49ec-b366-12ba630ea2cb)
- Progress 조건 위배
  - P0이 critical Section에 진입하지 않는 경우
  - 한 Progress가 두 번 연속 CS에 진입 불가

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/44138d02-985c-4dbc-a565-c2e7c8991ef8)
- Progress, Bounded wating 조건 위배
## Mutual Exclusion Solutions
- SW solutions
  - 2-Process Mutual Exclusion
    - Dekker's algorithm
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/b276f17a-98fa-40fd-84fc-4a31b5a3eae0)

      - Two process ME을 보장하는 최초의 알고리즘
    - Peterson's algorithm
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/5f775976-c467-4cf9-bd5d-b3fa72e6f994)

      - Dekker's algorithm 보다 간단하게 구현
  - N-Process Mutual Exclusion
    - Dijkstra's algorithm
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/482fa6b2-a533-4ee9-8e24-6395deb21181)
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/df2109f1-605e-4b1d-b9ce-79dccd91f960)

      - 최초로 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결
      - 실행 시간이 가장 짧은 프로세스에 프로세서 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공
    - Knuth's algorithm
      - 이전 알고리즘 관계 분석 후 일치하는 패턴을 찾아 패턴의 반복을 줄여서 프로세스에 프로세서 할당
      - 무한정 연기할 가능성을 배제하는 해결책을 제시했으나, 프로세스들이 아주 오래 기다려야 함
    - Eisenberg and McGuire's algorithm
    - Lamport's algorithm
      - 사람들로 붐비는 빵집에서 번호표 뽑아 빵 사려고 기다리는 사람들에 비유해서 만든 알고리즘 (Bakery algorithm)
      - 준비 상태 큐에서 기다리는 프로세스마다 우선순위를 부여하여 그 중 우선순위가 가장 높은 프로세스에 먼저 프로세서를 할당함
    - Hansen's algorithm
      - 실행 시간이 긴 프로세스에 불리한 부분을 보완하는 것
      - 대기시간과 실행 시간을 이용하는 모니터 방법
      - 분산 처리 프로세서 간의 병행성 제어 많이 발표
  - SW solution들의 문제점
    - 속도가 느림
    - 구현이 복잡함
    - ME primitive 실행 중 preemption 될 수 있음
      - 공유 데이터 수정 중은 interrput를 억제함으로서 해결 가능
        - overhead 발생
    - Busy waiting
      - Inefficient
- HW solution
  - TestAndSet(TAS) instruction
- OS supported SW solution
  - Spinlock
  - Semaphore
  - Eventcount/sequencer
- Language-Level solution
  - Monitor
