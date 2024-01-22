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