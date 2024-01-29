# 프로세스 동기화& 상호배제

### TestAndSet (TAS) instruction

- Test 와 Set을 한번에 수행하는 기계어
- Machine instruction
    - Atomicity, Indivisgle
    - 실행 중 interrupt를 받지 않음 (preemption 되지 않음)
- Busy waiting
    - Inefficient

## ME with TAS Instruction

- 장점
    - 구현이 간단
- 단점
    - Busy Waiting
        - Inefficient

- Busy waiting 문제를 해소한 상호배제 기법
    - Semaphore
        - 대부분 os들이 사용하는S 기법

## Spinlock

- 정수 변수
- 초기화, P(), V() 연산으로만 접근 가능
    - 위 연산들은 indivisible (or atomic) 연산
        - OS support
        - 전체가 한 instruction cycle에 수행 됨

해당 p에서는 preemtive를 방지하기 때문에 임계 구역에 동시에 들어가거나 아무도 들어가지 못하는 것을 막는다. ⇒ 상호배제 문제 해결

- 멀티 프로세서 시스템에서만 사용 가능
    - cpu가 두 개 있어서 pi와 pj가 동시에 동작해야지 예상한 동작을 한다.
- Busy waiting
    - 다른 프로세스가 임계구역을 사용할 수 있을 때까지 계속 기다S린다.

## Semaphore

- 1965년 Dijkstra가 제안
- Busy waiting 문제 해결
- 음이 아닌 정수형 변수(S)
    - 초기화 연산, P(), V() 로만 접근 가능
    - p: probern (검사)
    - v: verhogen (증가)
- 임의의 S 변수 하나에 ready queue 하나가 할당 됨

- 초기화 연산
    - S 변수에  초기값을 부여하는 연산
- 모두 indivisible 연산
    - OS support
    - 전체가 한 instruction cycle에 수행 됨

- No busy waiting
    - 기다려야 하는 프로세스는 block(asleep)상태가 됨
- Semaphore queue에 대한 wake-up 순서는 비결정적
    - Starvation problem

### Binary semaphore

- S가 0과 1 두 종류의 값만 갖는 경우
- 상호배제나 프로세스 동기화의 목적으로 사용

### Counting semaphore

- S가 0 이상의 정수값을 가질 수 있는 경우
- Producer-Consumer 문제 등을 해결하기 위해 사용
    - 생산자-소비자 문제

스핀락은 임계 영역에 들어가기 위해 계속 돌면서 대기를 한다면, 세마포는 (대기실에서) 대기를 하고 일이 끝나면 깨워준다.

### Semaphore로 해결 가능한 동기화 문제들

- 상호배제 문제
- 프로세스 동기화 문제
- 생산자- 소비자 문제
- reader-writer 문제
- Dining philosopher problem
- 기타

### Processs synchronization

- Process 들의 실행 순서 맞추기
    - 프로세스들은 병행적이며, 비동기적으로 Pr수

![47475](https://github.com/ensk26/cs_study/assets/70767115/1688e2f5-3139-4529-a749-acf48ddc5d62)


### Producer-Consumer problem

- 생산자(Producer) 프로세스
    - 메시지를 생성하는 프로세스 그룹
- 소비자(Consumer) 프로세스
    - 메시지 전달받는 프로세스 그룹

### Reader-Writer problem

- Reader
    - 데이터에 대해 읽기 연산만 수행
- Writer
    - 데이터에 대해 갱신 연산을 수행
- 데이터 무결성 보장 필요
    - Reader들은 동시에 데이터 접근 가능
    - Writer들(또는 reader와 write)이 동시 데이터 접근 시, 상호배제(동기화) 필요
- 해결법
    - reader / writer 에 대한 우선권 부여
        - reader preferences solution
            - 읽는건 여러명 가능, 사후처리는 처음인 한명만 가능
        - writer preference solution
