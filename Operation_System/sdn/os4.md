# 스레드 관리

## 프로세스와 스레드

프로세스는 자원을 할당 받고 제어한다.

프로세스가 자원들을 할당받고, 그 자원들을 스레드가 제어한다.

![](https://github.com/SSAFY11thDaejeon7/cs_study/assets/70767115/bdc55c03-1098-46a0-9bfb-392ad75528b2)

- 하나의 프로세스 안에 여러개의 쓰레드가 존재하고, 각각 제어를 할수 있다.
- 프로세스가 할당받은 리소스는 공유한다.

### 스레드

- 같은 프로세스의 스레드들은 동일한 주소 공간(리소스)을 공유한다.
- 스레드마다 각자의 작업 영역(스택 영역)을 가지게 된다.
- 자기의 할당된 영역에서 지역 데이터를 만들고 작업하게 된다.
- 공유되는 자원(ex. 소스 코드) pc로 자기 프로그램 흐름을 제어한다.
- 프로세서 활용의 기본 단위
- 제어 요소 외 코드, 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유

### 스레드 구성요소

- Thread ID
- Register set (PC, SP 등)
- Stack(local data)

### 스레드의 장점

- 사용자 응답성 (Responsiveness)
    - 일부 스레드의 처리가 지연되어도, 다른 스레드는 작업을 계속 처리 가능
        - 자원을 공유해 동시에 여러 작업이 가능

- 자원 공유(Resource sharing)
    - 자원을 공유해서 효율성 증가 (커널의 개입을 피할 수  있음)
        - ex) 동일 address space에서 스레드 여러 개
        - 자원을 공유하므로, context switch 비용이 발생하지 않는다.

- 경제성
    - 프로세스의 생성, context switch에 비해 효율적

- 멀티 프로세서(multi-processor)활용
    - 병렬처리를 통해 성능 향상

## 스레드의 구현

### 사용자 수준 스레드 (User thread)

- 사용자 영역의 스레드 라이브러리로 구현 됨
    - 스레드의 생성, 스케줄링 등
    - POSIX threads, Win32 threads, Java thread API 등

- 커널은 스레드의 존재를 모른다.

- 커널의 관리(개입)를 받지 않음
    - 생성 및 관리의 부하가 적음, 유연한 관리 가능
    - 이식성(portability)이 높음
        - 스레드 라이브러리만 있는 곳이면 만든 프로그램을 사용할 수 있다.

- 커널은 프로세스 단위로 자원 할당
    - 하나의 스레드가 block 상태가 되면,  프로세스도 block 상태로 내려 오기 때문에 모든 스레드가 대기한다.
        - single-threaded kernel의 경우
- 다대일(n:1) 모델

### 커널 수준 스레드 (Kernel thread)

- OS(Kernel)가 직접 관리
- 커널 영역에서 스레드의 생성, 관리 수행
    - 스레드 사이 context switching 등 부하(Overhead)가 큼

- 커널이 각 스레드를 개별적으로 관리
    - 프로세스 내 스레드들이 병행 수행 가능
        - 하나의 스레드가 block 상태가 되어도, 다른 스레드는 계속 작업 수행 가능
        
- 일대일(1:1) 모델

### 혼합형 (n:m) 스레드

- n개 사용자 수준 스레드 - m개의 커널 스레드(n>m)
    - 사용자는 원하는 수만 큼 스레드 사용
    - 커널 스레드는 자신에게 할당된 하나의 사용자 스레드가 block 상태가 되어도, 다른 스레드 수행 가능
        - 병행 처리 가능

- 효율적이면서도 유연
- 다대다(n:m) 모델

# 프로세스 스케줄링

## 다중 프로그래밍(Multi-programming)

- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당 할 프로세스를 선택해야 한다.
    - 스케줄링(Scheduling)
- 자원 관리
    - 시간분할(time sharing) 관리
        - 하나의 자원을 여러 스레드들이 번갈아 가며 사용
        - ex) 프로세서
        - 프로세스 스케줄링(Process scheduling)
            - 프로세서 사용시간을 프로세스들에게 분배
    
    - 공간 분할(space sharing)관리
        - 하나의 자원을 분할하여 동시에 사용
        - ex) 메모리(memory)

## 스케줄링(scheduling)의 목적

- 시스템의 성능(performance) 향상
- 대표적 시스템 성능 지표(index)
    - 응답시간 (response time)
        - 작업 요청(submission)으로부터 응답을 받을때까지의 시간
    - 작업 처리량(throughtput)
        - 단위 시간 동안 완료된 작업의 수
    - 자원 활용도(resource utilization)
        - 주어진 시작(Tr) 동안 자원이 활용된 시간 (Tc)
            - 활용도 = Tr/Tc

### 대기 시간, 응답시간, 반환시간

![](https://github.com/SSAFY11thDaejeon7/cs_study/assets/70767115/2aed86fa-78d8-495a-af60-bd5b010cee98)

## 스케줄링 기준 (Criteria)

- 스케줄링 기법이 고려하는 항목들
- 프로세스의 특성
    - I/O-bounded or compute-bounded

- 시스템 특성
    - batch system or interactive system

- 프로세스의 긴급성(urgency)
    - Hard or soft - real time, non - real time systems

- 프로세스 우선순위 (priority)
- 프로세스 총 실행 시간 (total service time)

### CPU burst vs I/O burst

- 프로그램은 cpu와 i/o를 사용을 반복하면서 돌아간다.
- 프로세스 수행
    - CPU 사용 + I/O 대기

- CPU burst
    - CPU 사용 시간이 성능을 결정 시킨다.
    - 사용 시간 더 길다.

- I/O burst
    - I/O 대기 시간이 성능을 결정 시킨다.
    - 대기 시간이 더 길다.
    
- Burst time은 스케줄링의 중요한 기준 중 하나이다.

## 스케줄링의 단계(Level)

- 발생하는 빈도 및 할당 자원에 따른 구분

![](https://github.com/SSAFY11thDaejeon7/cs_study/assets/70767115/7ad473ee-b82a-406c-a080-734b5dd86b31)

### Long-term scheduling

- 장기 스케줄링
- Job scheduling
- 시스템에서 제출할 (kernel에 등록 할) 작업(Job) 결정
    - Admission scheduling, High-level scheduling

- 다중프로그래밍 정도(degree)조절
    - 시스템 내에 프로세스 수 조절

- I/O-bounded와 compute-bounded 프로세스들을 잘 섞어서 선택해야 한다.
    - 한쪽만 일하고 다른 한쪽만 쉬면 시스템 입장에서는 비효율적이다.

- 시분할 시스템에서는 모든 작업을 시스템에 등록
    - Long-term scheduling이 불필요
    - 시간을 나눠서 등록하기 때 상대적으로 덜 중요하다.

### Mid-term scheduling

- 중기 스케줄링
- 메모리 할당 (Memory allocation)
    - Intermediate-level scheduling
    - Swapping (swap-in/swap-out)

### Short-term scheduling

- 단기 스케줄링
- Process scheduling
    - Low-leve scheduling
    - 프로세서를 할당할 프로세스를 결정
        - Processor scheduler(ready → running), dispatcher

- 가장 빈번하게 발생된다.
    - Interrupt, block(I/O), time-out, …
    - 매우 빨라야한다.
    - 너무 길면 오버헤드가 발생한다.

## 스케줄링 정책

### 선점 vs 비선점

- Non-preemptive scheduling
    - 할당 받을 자원을 스스로 반납할 때 까지 사용
        - system call, I/O
- 장점
    - context switch overhead가 적음
- 단점
    - 잦은 우선순위 역전, 평균 응답 시간 증가
    - 우선순위가 높아도 비선점이라서 대기를 해야한다.

- Preemptive scheduling
    - 타의에 의해 자원을 배앗길 수 있음
        - 예) 할당 시간 종료, 우선순위가 높은 프로세스 등장
    - Context switch overhead가 큼
    - Time-sharing system, real-time system 등에 적합 (응답성이 높음)

### Priority

- 프로세스의 중요도
- Static priority(정적 우선순위)
    - 프로세스 생성시 결정된 priority가 유지 됨
    - 구현이 쉽고, overhead가 적음
    - 시스템 환경 변화에 대한 대응이 어려움

- Dynamic priority (동적 우선순위)
    - 프로세스의 상태 변화에 따라 priority 변경
    - 구현이 복잡, priority 재계산 overhead가 큼
    - 시스템 환경 변화에 유연한 대응 가능
