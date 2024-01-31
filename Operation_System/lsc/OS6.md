# [OS] Lecture 6. Process Synchronization and Mutual Exclusion

## Process Synchronization (동기화)
- **다중 프로그래밍 시스템**
    - 여러 개의 프로세스들이 존재
    - 프로세스들은 서로 독립적으로 동작
    - 공유 자원 또는 데이터가 있을 때, 문제 발생 가능

- **동기화(Synchronization)**
    - 프로세스들이 서로 동작을 맞추는 것
    - 프로세스들이 서로 정보를 공유하는 것

## Asynchronous and Concurrent P's
- **비동기적(Asynchronous)**
    - 프로세스들이 서로에 대해 모름
- **병행적(Concurrent)**
    - 여러 개의 프로세스 들이 동시에 시스템에 존재
- **병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근할 때 문제가 발생할 수 있음**

## Terminologies
- **공유 데이터(Shared or Critical data)**
    - 여러 프로세스들이 공유하는 데이터
- **임계 영역(Critical section)**
    - 공유 데이터를 접근하는 코드 영역(code segment)
- **상호배제(Mutual exclusion)**
    - 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/6e00364d-dc36-4bd5-8d20-1cc8289e7469)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/15b148d7-1490-46c4-99f2-cff87a2c45fc)


## Mutual Exclusion Methods
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/b86cb235-fcd6-4555-8c7a-8ecdef347db8)

- **Mutual exclusion primitives**
    - enterCS() primitive
        - Critical section 진입 전 검사
        - 다른 프로세스가 critical section 안에 있는지 검사
    - exitCS() primitive
        - Critical section을 벗어날 때의 후처리 과정
        - Critical section을 벗어남을 시스템이 알림
          ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e1411e83-e678-47d7-86fa-80e3756a623c)


## Requirements for ME primitives
- **Mutual exclusion(상호배제)**
    - Critical section(CS)에 프로세스가 있으면, 다른 프로세스의 진입을 금지
- **Progress(진행)**
    - CS 안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
- **Bounded waiting(한정대기)**
    - 프로세스의 CS 진입은 유한시간 내에 허용되어야 함

## Two Process Mutual Exclusion
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/ce1a1c86-25cf-49ec-b366-12ba630ea2cb)
- **Progress 조건 위배**
    - P0이 critical Section에 진입하지 않는 경우
    - 한 Progress가 두 번 연속 CS에 진입 불가

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/44138d02-985c-4dbc-a565-c2e7c8991ef8)
- **Progress, Bounded wating 조건 위배**
## Mutual Exclusion Solutions
- **SW solutions**
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
- **HW solution**
    - TestAndSet(TAS) instruction
      - Test와 Set을 한번에 수행하는 기계어
      - Machine instruction
        - Atomicity, Indivisible
        - 실행 중 interrupt를 받지 않음 (preemption 되지 않음)
      - Busy waiting
        - Inefficient
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/90bb63dd-9335-4f78-bc6d-8755628222e5)

    - ME with TAS Instruction
      - 3개 이상의 프로세스의 경우, Bounded waiting 조건 위배
    - 장점
      - 구현이 간단
    - 단점
      - Busy waiting
        - Inefficient
    - Busy waiting 문제를 해소한 상호배제 기법
      - Semaphore
        - 대부분의 OS들이 사용하는 기법
- **OS supported SW solution**
    - Spinlock
      - 정수 변수
      - 초기화, P(), V() 연산으로만 접근 가능
        - 위 연산들은 indivisible (or atomic) 연산
          - OS support
          - 전체가 한 instruction cycle에 수행됨
            ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/97241b18-cde5-40bc-a9c8-30e5266d105f)

      - 멀티 프로세서 시스템에서만 사용 가능
      - Busy waiting
    - Semaphore
      - 1965년 다익스트라가 제안
      - Busy waiting 문제 해결
      - 음이 아닌 정수형 변수(S)
        - 초기화 연산, P(), V()로만 접근 가능
          - P: Probern (검사)
          - V: Verhogen (증가)
        - 임의의 S 변수 하나에 ready queue 하나가 할당 됨
      - Binary semaphore
        - S가 0과 1 두 종류의 값만 갖는 경우
        - 상호배제나 프로세스 동기화의 목적으로 사용
      - Counting semaphore
        - S가 0 이상의 정수값을 가질 수 있는 경우
        - Producer-Consumer 문제 등을 해결하기 위해 사용
          - 생산자-소비자 문제
      - 초기화 연산
        - S 변수에 초기값을 부여하는 연산
      - P() 연산, V() 연산
      - 모두 indivisible 연산
        - OS support
        - 전체가 한 instruction cycle에 수행됨
      - Semaphore로 해결 가능한 동기화 문제들
        - 상호배제 문제
          - Mutual exclusion
        - 프로세스 동기화 문제
          - Process synchronization problem
        - 생산자-소비자 문제
          - Producer-consumer problem
        - Reader-writer 문제
        - Dining philosopher problem
        - 기타
      - Process sychronization
        - Process들의 실행 순서 맞추기
          - 프로세스들은 병행적이며, 비동기적으로 수행
            ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/07c80fa2-53ff-4bab-bf0e-4f19393ad231)
            ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/ccf61532-d27f-41b6-b645-8fbfb3847628)


      - Producer-Consumer problem
        - 생산자(Producer) 프로세스
          - 메시지를 생성하는 프로세스 그룹
        - 소비자(Consumer) 프로세스
          - 메시지 전달받는 프로세스 그룹
            ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e75b4651-b07c-4a43-bcc0-99122fdab73a)

        - Producer-Consumer problem with single buffer
          ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/db9477de-dc4c-4947-8e38-bf748e237b2e)

        - Producer-Consumer problem with N-buffers
          ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/22867416-1c30-40b0-8148-0c6b9f51d154)
          ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/15d39204-b1d6-457f-8db5-192c9d747b4a)

      - Reader-Writer problem
        - Reader
          - 데이터에 대해 읽기 연산만 수행
        - Writer
          - 데이터에 대해 갱신 연산을 수행
        - 데이터 무결성 보장 필요
          - Reader들은 동시에 데이터 접근 가능
          - Writer들(또는 reader와 write)이 동시 데이터 접근 시, 상호배제(동기화) 필요
        - 해결법
          - reader/writer에 대한 우선권 부여
            - reader preference solution
            - writer preference solution
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/62f6daff-344d-492d-a5c4-3ddb622096de)

      - No busy waiting
        - 기다려야하는 프로세스는 block(sleep)상태가 됨
        - Semaphore queue에 대한 wake-up 순서는 비결정적
          - Starvation problem
    - Eventcount/sequencer
      - 은행의 번호표와 비슷한 개념
      - Sequencer
        - 정수형 변수
        - 생성시 0으로 초기화, 감소하지 않음
        - 발생 사건들의 순서 유지
        - ticket() 연산으로만 접근 가능
      - ticket(S)
        - 현재까지 ticket() 연산이 호출된 횟수를 반환
        - Indivisible operation
      - Eventcount
        - 정수형 변수
        - 생성시 0으로 초기화, 감소하지 않음
        - 특정 사건의 발생 횟수를 기록
        - read(E), advance(E), await(E,v) 연산으로만 접근 가능
      - read(E)
        - 현재 Eventcount값 반환
      - advance(E)
        - E <- E + 1
        - E를 기다리고 있는 프로세스를 깨움 (wake-up)
      - await(E,v)
        - V는 정수형 변수
        - if(E<v)이면 E에 연결된 Q_E에 프로세스 전달(push) 및 CPU scheduler 호출
      - Mutual exclusion
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/b0040ad2-96cb-4213-b0f6-4187511c5107)

      - Producer-Consumer problem
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/601c3ada-402b-4304-8efd-a11d05639fba)

      - No busy waiting
      - No starvation
        - FIFO scheduling for Q_E
      - Semaphore보다 더 low-level control이 가능
- **Language-Level solution**
  - Monitor
    - 공유 데이터와 Critical section의 집합
    - Conditional variable
      - wait(), signal() operations
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/a740a71c-ebe2-4f27-8687-02dd714dbd8d)

    - Entry queue(진입 큐)
      - 모니터 내의 prodecure 수만큼 존재
    - Mutual exclusion
      - 모니터 내에는 항상 하나의 프로세스만 진입 가능
    - Information hiding(정보 은폐)
      - 공유 데이터는 모니터 내의 프로세스만 접근 가능
    - Condition queue(조건 큐)
      - 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
    - Signaler queue(신호제공자 큐)
      - 모니터에 항상 하나의 신호제공자 큐가 존재
      - signal() 명령을 실행한 프로세스가 임시 대기
    - 자원 할당 문제
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/373c2fa5-7457-4525-b54e-8007cd14610e)
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/f2268ccc-5738-445b-b4db-1b6121932548)


    - 자원 할당 시나리오
      - 자원 R 사용 가능
      - Monitor 안에 프로세스 없음
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/a0b344bd-a1ce-480f-b103-180d37f49f49)

      - 프로세스 Pj가 모니터 안에서 자원 R을 요청
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/7de38d44-c56c-4735-b5f9-76275eda9ad7)

      - 자원 R이 Pj에게 할당 됨
      - 프로세스 Pk가 R 요청, Pm 또한 R 요청
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/c554e5b2-70cf-4ad1-ab23-9576daa7108c)

      - Pj가 R 반환
      - R_Free.signal() 호출에 의해 Pk가 wakeup
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/1e5b95f3-52ab-4d39-9e27-57894eea11ba)

      - 자원 R이 Pk에게 할당 됨
      - Pj가 모니터 안으로 돌아와서, 남은 작업 수행
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/28048d02-dd1a-45ac-8c72-5ce5eee9dec7)

    - Producer-Consumer Problem
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/3857cd1b-666c-42b2-a794-c5c7a5db512d)
      ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/0f4abb89-fb0d-4190-b97f-46999035b499)

    - Reader-Writer Problem
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
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/20b696a4-6e47-4141-a396-f39e32a96a46)

    - Dining philosopher problem
      - 5명의 철학자
      - 철학자들은 생각하는 일, 스파게티 먹는 일만 반복함
      - 공유 자원: 스파게티, 포크
      - 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야 함
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/c11aa54e-d221-4965-9903-0f4967c6ea56)
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/6fb72566-cd17-4646-8257-eef99064dcee)
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/c6dc2258-9360-439b-869c-28735449486c)
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/86581632-4557-42c3-90d8-2f82675dbceb)




    - 장점
      - 사용이 쉽다
      - Deadlock 등 error 발생 가능성이 낮음
    - 단점
      - 지원하는 언어에서만 사용 가능
      - 컴파일러가 OS를 이해하고 있어야 함
        - Critical section 접근을 위한 코드 생성
