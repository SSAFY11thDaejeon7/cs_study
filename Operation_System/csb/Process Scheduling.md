# 다중 프로그래밍
### 다중 프로그래밍 (Multi-programming)

- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당 할 프로세스를 선택 해야 함(스케줄링, Scheduling)
- 자원 관리
    - 시간 분할(time sharing)관리
        - 하나의 자원(프로세서)을 여러 스레드들이 번갈아 가며 사용
        - 프로세스 스케줄링(Process scheduling): 프로세서 사용시간을 프로세스들에게 분배
    - 공간 분할(space sharing) 관리
        - 하나의 자원(메모리)을 분할하여 동시에 사용

# 스케줄링
### 스케줄링의 목적

- 시스템의 성능(performance) 향상
- 대표적 시스템 성능 지표(index)
    - 응답시간(response time): 작업 요청(submission)으로부터 응답을 받을때까지의 시간
    - 작업 처리량(throughput): 단위 시간 동안 완료된 작업의 수
    - 자원 활용도(resource utilization): 주어진 시간동안 자원이 활용된 시간 (자원이 활용된 시간 / 주어진 시간)
- 목적에 맞는 지표를 고려하여 스케줄링 기법을 선택

### 시스템 성능 지표들

- 평균 응답 시간(mean response time)
    - 사용자 지향적 (ex. interactive systems)
- 처리량(throughput)
    - 시스템 지향적 (ex. batch systems)
- 자원 활용도(resource utilization)
- 공평성(fairness)
- 실행 대기 방지
    - 무기한 대기 방지
- 예측 가능성(predictability)
    - 적절한 시간안에 응답을 보장하는가

### 대기시간, 응답시간, 반환시간

<img width="986" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/ee898ee3-066d-4647-b794-678d8edb5511">


### 스케줄링 기준 (Criteria)

- 스케줄링 기법이 고려하는 항목들
- 프로세스의 특성 (I/O-bounded or compute-bounded)
- 시스템 특성 (Batch system or interactive system)
- 프로세스의 긴급성 (Hard- or soft- real time, non-real time systems)
- 프로세스 우선순위 (priority)
- 프로세스 총 실행 시간 (total service time)

### CPU burst vs I/O burst

- 프로세스 수행 = CPU 사용 + I/O대기
- CPU burst (compute-bounded process): CPU 사용 시간
- I/O burst (I/O-bounded process): I/O 대기 시간
- Burst time은 스케줄링의 중요한 기준 중 하나

# 스케줄링의 단계 (Level)

- 발생하는 빈도 및 할당 자원에 따른 구분
- Long-term scheduling
    - 장기 스케줄링
    - Job scheduling
- Mid-term scheduling
    - 중기 스케줄링
    - Memory allocation
- Short-term scheduling
    - 단기 스케줄링
    - Process scheduling

<img width="1029" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/98c73591-d5c3-4c44-bfc1-b31acda238ce">


### Long-term Scheduling

- Job scheduling
    - 시스템에 제출 할 (Kernel에 등록 할) 작업 결정
- 다중 프로그래밍 정도 조절 → 시스템 내에 프로세스 수를 조절
- I/O-bounded 와 compute-bounded 프로세스들을 잘 섞어서 선택해야 함
    - 시스템의 효율이 높아짐
- 시분할 시스템에서는 모든 작업을 시스템에 등록하기 때문에 Long-term scheduling이 덜 중요함

### Mid-term Scheduling

- 메모리 할당 결정(memory allocation)
    - Swapping(swap-in / swap-out)

### Short-term Scheduling

- Process Scheduling
    - 프로세서를 할당할 프로세스를 결정
- 가장 빈번하게 발생
    - Interrupt, block (I/O), time-out
    - 매우 빨라야 함

# 스케줄링 정책 (Policy)

### Preemptive (선점) / Non-preemptive Scheduling (비선점)
- Non-preemptive scheduling
    - 할당 받을 자원을 스스로 반납할 때까지 사용
        - 예) system call, I/O, Etc.
    - 장점
        - Context switch overhead가 적음
    - 단점
        - 잦은 우선순위 역전, 평균 응답 시간 증가
- Preemptive scheduling
    - 타의에 의해 자원을 빼앗길 수 있음
        - 예) 할당 시간 종료, 우선순위가 높은 프로세스 등장
    - 장점
        - 높은 응답성 (Time-sharing system, Real-time system 에 적합)
    - 단점
        - Context switch overhead가 큼

### Priority
- 프로세스의 중요도
- Static priority (정적 우선순위)
    - 프로세스 생성시 결정된 priority가 유지됨
    - 구현이 쉽고, overhead가 적음
    - 시스템 환경 변화에 대한 대응이 어려움
- Dynamic priority (동적 우선순위)
    - 프로세스의 상태 변화에 따라 priority 변경
    - 구현이 복잡, priority 재계산 overhead가 큼
    - 시스템 환경 변화에 유연한 대응 가능

# 기본 스케줄링 알고리즘

### FCFS (First-Come-First-Service)
- Non-preemptive scheduling
- 스케줄링 기준
    - 도착 시간 (ready queue 기준)
    - 먼저 도착한 프로세스를 먼저 처리
- 자원을 효율적으로 사용 가능
    - High resource utilization ← 불필요한 스케줄링 오버헤드가 발생하지 않기 때문
- Batch System에 적합
- Interactive System에 부적합
- 단점
    - Convoy effect: 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게되는 현상 (대기시간 >> 실행시간)
    - 긴 평균 응답시간
<img width="597" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/e9c46974-89eb-4daf-b905-80b7e4bc2d2b">

<img width="896" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/aa2180ce-27b0-4860-8caf-bb319645c1d5">
- Normalized TT (NTT) = TT / BT (1에 가까울수록 이상적, 클수록 평균적으로 더 대기가 김)

### RR (Round-Robin)
- Preemptive scheduling
- 스케줄링 기준
    - 도착 시간(ready queue 기준)
    - 먼저 도착한 프로세스를 먼저 처리
- 자원 사용 제한 시간(time quantum) 이 있음
    - System parameter
    - 프로세스는 할단된 시간이 지나면 자원 반납(Timer-runout)
    - 특정 프로세스의 자원 독점(monopoly) 방지
    - Context switch overhead가 큼
- 대화형, 시분할 시스템에 적합
- Time quantum(제한 시간)이 시스템 성능을 결정하는 핵심 요소
    - 제한 시간이 무한에 가까울수록 → FCFS
    - 제한 시간이 작아질수록 → processor sharing
        - 사용자는 모든 프로세스가 각각의 프로세서 위에서 실행되는 것처럼 느끼지만 매우 높은 Context switch overhead 발생
<img width="657" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/8bcf79ae-22fc-4099-b50f-d6a912025db1">

<img width="1012" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/30b00eec-161f-43d2-adf2-4a54bdf80c88">
- FCFS에 비해 NTT가 균등한 것을 확인할 수 있음

### SPN (Shortest-Process-Next)
- SJF(Shortest Job First) Scheduling
- Non-preemptive scheduling
- 스케줄링 기준
    - 실행시간(Burst time 기준)
    - Burst time이 가장 작은 프로세스를 먼저 처리
- 장점
    - 평균 대기시간 최소화
    - 시스템 내 프로세스 수 최소화
        - 스케줄링 부하 감소, 메모리 절약 → 시스템 효율 향상
    - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
    - Starvation(기아현상, 무한대기) 현상 발생
        - BT가 긴 프로세스는 자원을 할당 받지 못할 수 있음 → Aging 등으로 해결
    - 정확한 실행시간을 알 수 없음 → 실행시간 예측 기법 필요
<img width="1031" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/09c732ff-5f79-4d7d-9015-2f2279b9e3e3">

### SRTN(Shortest Remaining Time Next)
- SPN의 변형
- Preemptive scheduling
    - 잔여 실행 시간이 더 적은 프로세스가 ready 상태가 되면 선점됨
- 장점
    - SPN의 장점 극대화
- 단점
    - 프로세스 생성시, 총 실행 시간 예측이 필요함
    - 잔여 실행을 계속 추적해야 함 = overhead
    - Context switching overhead
- 구현 및 사용이 비현실적
<img width="836" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/14938ddd-fa5c-4bf9-b678-a9c256279927">

### HRRN (High-Response-Ratio-Next)
- SPN의 변형
    - SPN + Aging concepts, Non-preempitve scheduling
- Aging concepts
    - 프로세스의 대기 시간을 고려하여 기회를 제공
- 스케줄링 기준
    - Response ratio가 높은 프로세스 우선
- Response ratio(응답률) = (WT + BT) / BT
    - SPN의 장점 + Starvation 방지
    - 실행 시간 예측 기법 필요(overhead)
<img width="1076" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/f0b13694-ba32-43bf-a334-a63b4aa84548">

### MLQ (Multi-level Queue)
- 작업이나 우선순위별로 별도의 ready queue를 가짐
    - 최초 배정된 queue를 벗어나지 못함
    - 각각의 queue는 자신만의 스케줄링 기법 사용
- Queue 사이에는 우선순위 기반의 스케줄링 사용
- 장점
    - 우선순위가 높은 작업에 대해 빠른 응답 가능
- 단점
    - 여러 개의 Queue 관리 등 스케줄링 overhead
    - 우선순위가 낮은 queue는 starvation 현상 발생 가능
<img width="750" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/3c8dad0c-bd9f-472b-8c71-02d5078340ed">


### MFQ (Multi-level Feedback Queue)
- 프로세스의 Queue간 이동이 허용된 MLQ
- Feedback을 통해 우선 순위 조정
    - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
    - Dynamic priority
    - Preemptive scheduling
    - Favor short burst-time processes
    - Favor I/O bounded processes
    - Improve adaptability
- 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있음
- 단점
    - 설계 및 구현이 복잡, 스케줄링 overhead가 큼
    - Starvation 문제 등
- 변형
    - 각 준비 큐마다 시간 할당량을 다르게 배정
    - 입출력 위주 프로세스들을 상위 단계 큐로 이동, 우선 순위 높음
    - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동
<img width="730" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/643eec6e-78d6-4c32-8bbb-d5fe7784ccac">
