# 1. 스케줄링의 목적

### 프로세스 스케줄링이란

- 여러 개의 프로세스가 시스템 내 존재
- 자원을 할당할 프로세스를 선택 해야 함 ⇒ `스케줄링`
- 자원 관리
    - 시간 분할 관리
        - 하나의 자원을 여러 스레드들이 번갈아 가며 사용
        - 예) 프로세서
        - 프로세스 스케줄링: 프로세서 사용 시간을 프로세스들에게 분배
    - 공간 분할 관리
        - 하나의 자원을 분할하여 동시에 사용
        - 예) 메모리

### 스케줄링의 목적

> 시스템의 성능 향상!
> 
- 대표적인 시스템 성능의 지표
    - 응답시간: 작업 요청으로부터 응답을 받을 때까지의 시간 → 사용자 대화형 시스템, real-time 시스템에 적합
    - 작업 처리량: 단위 시간 동안 완료된 작업의 수 → 일괄 처리 시스템
    - 자원 활용도: 주어진 시간 동안 자원이 활용된 시간
- 목적에 맞는 지표를 고려하여 스케줄링 기법을 선택한다.
- 대기시간: 실행 시작까지의 시간
- 응답 시간: 첫 번째 출력까지의 시간
- 반환 시간: 실행 종료까지의 시간

# 2. 스케줄링 기준 및 단계

### 스케줄링의 기준

> 스케줄링 기법이 고려하는 항목들
> 
- 프로세스의 특성
    - I/O Bounded or Compute-Bounded
- 시스템의 특성
    - Batch System(일괄 처리 시스템) or Interactive System
- 프로세스의 긴급성
    - Hard, soft real time, non-real time systems
- 프로세스 우선 순위
- 프로세스 총 실행 시간

### CPU Burst vs I/O Burst

> 프로세스의 수행은 CPU 사용과 I/O 대기의 반복임!
수행 시간을 나타내는 Burst time은 스케줄링의 중요한 기준 중 하나임
> 
- CPU Burst: CPU 사용 시간 (CPU Burst가 더 긴 경우: Compute Bounded)
- I/O Burst: I/O 대기 시간 (I/O Burst가 더 긴 경우: I/O Bounded)

### 스케줄링의 단계

> 발생하는 빈도 및 할당 자원에 따른 구분
> 
- Long-term scheduling
    - Job scheduling: 시스템에 제출할 작업을 결정한다
    - 다중 프로그래밍의 정도를 조절한다 (시스템 내에 프로세스 수 조절)
    - I/O Bounded와 Compute Bounded 프로세스들을 잘 섞어서 선택해야 함
    - 시분할 시스템에서는 모든 작업을 시스템에 등록한다 → Long-term scheduling이 불필요함
- Mid-term scheduling
    - 메모리 할당을 결정 (memory allocation)
    - Intermediate-level scheduling
    - Swapping (Swap in/Swap out)
- Short-term Scheduling
    - Process 할당해주는 스케줄링
    - Low-level scheduling
    - 프로세서를 할당할 프로세스를 결정한다.
    - 가장 빈번하게 발생한다 (interrupt, block, time-out 등)
    - 매우  빨라야 함 !
    
    
<img width="650" alt="스케줄링의단계" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/784492fd-a1e1-4c56-a56f-0900fd961885">

# 3. 스케줄링 정책

### 선점 vs 비선점 (뺏을 수 있는가 없는가?)

- Non-preemptive scheduling: 할당 받을 자원을 스스로 반납할 때까지 사용
    - ex) System Call, I/O
    - 장점: context switch overhead가 적음
    - 단점: 잦은 우선순위 역전, 평균 응답 시간 증가
- preemptive scheduling: 타의에 의해 자원을 빼앗길 수 있음
    - ex) 할당 시간 종료, 우선순위가 높은 프로세스 등장
    - 단점: context switch overhead가 큼
    - Time-sharing System, real-time System 등에 적합 (응답성이 좋음)

### 우선순위

프로세스의 중요도

- Static priority (정적 우선순위)
    - 프로세스 생성 시 한번 우선순위가 결정되면 유지됨
    - 구현이 쉽고, overhead가 적음
    - 시스템 환경 변화에 대한 적응이 어려움
- Dynamic priority (동적 우선순위)
    - 프로세스의 상태 변화에 따라 priority 변경
    - 구현이 복잡, priority 재계산 overhead가 큼
    - 시스템 환경 변화에 유연한 대응이 가능하다

# 4. 기본 스케줄링 알고리즘

### 1) FCFS (First-Come-First-Service)

- Non-preemptive scheduling (비선점 스케줄링)
- 스케줄링 기준 (criteria)
    - 도착 시간 (ready queue 기준)
    - 먼저 도착한 프로세스를 먼저 처리한다
- 자원을 효율적으로 사용 가능: High resource utilization / Why?
    - Batch system (일괄 처리 시스템)에 적합하다 - 들어오는 것에 빨리 응답하는 것보다 퍼포먼스 높이는 게 목적
    - interactive system에 부적합 - 반응이 빨리오길 바라는 대화형 시스템에서는 부적합하다.
- 단점:
    - **Convey effect**: 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 가지게 되는 현상 (대기 시간 >> 실행 시간)
    - **긴 평균 응답시간** (Response time)
<img width="585" alt="FCFS" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/d0142454-705e-4859-b99f-66f7be83c640">


| Process ID | Arrival time | Burst Time(BT) | Waiting Time(WT) | Turnaround Time(TT) | Normalized TT (NTT = TT / BT) |
| --- | --- | --- | --- | --- | --- |
| P1 | 0 | 3 | 0 | 3 | 1 |
| P2 | 1 | 7 | 2 | 9 | 9/7 |
| P3 | 3 | 2 | 7 | 9 | 9/2 |
| P4 | 5 | 5 | 7 | 12 | 12/5 |
| P5 | 6 | 3 | 11 | 14 | 14/3 |

### 2) RR (Round-Robin)

- Preemptive scheduling
- 스케줄링 기준
    - 도착 시간
    - 먼저 도착한 프로세스를 먼저 처리
- **자원 사용 제한 시간 (Time Quantum)이 있음**
    - System parameter
    - 프로세스는 할당된 시간이 지나면 자원 반납
    - 특정 프로세스의 자원 독점(monopoly)을 방지
    - Context Switch overhead가 큼!
    - Time Quantum이 끝나면 Ready queue 맨 뒤에 가서 줄을 선다.
- 대화형, 시분할 시스템에 적합
- **Time Quantum이 시스템 성능을 결정하는 핵심 요소**
    - Very large (infinite) → FCFS
    - Very small time quantum → processor sharing
        - 사용자는 모든 프로세스가 각각의 프로세서 위에서 실행되는 것처럼 느낌
        - 체감 프로세서 속도 = 실제 프로세서 성능의 1/n
        - High context switch overhead
<img width="674" alt="RR" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/1550eadc-8959-4dfb-bfab-1a3fd3930120">


| Process ID | Arrival time | Burst Time(BT) | Waiting Time(WT) | Turnaround Time(TT) | Normalized TT (NTT = TT / BT) |
| --- | --- | --- | --- | --- | --- |
| P1 | 0 | 3 | 2 | 5 | 5 / 2 |
| P2 | 1 | 7 | 11 | 18 | 18 / 7 |
| P3 | 3 | 2 | 2 | 4 | 4 / 2 |
| P4 | 5 | 5 | 10 | 15 | 15 / 5 |
| P5 | 6 | 3 | 9 | 12 | 12 / 3 |

(Time Quantum = 2) Average response time(TT) = 10.8..

(Time Quantum = 3) Average response time(TT) = 9.8

### 3) SPN (Shortest Process Next)

- Non-preemptive scheduling
- 스케줄링 기준
    - 실행 시간
    - 현재 Ready queue내에 Burst time이 가장 작은 프로세스를 먼저 처리
    - = SJF(Shortest Job First) scheduling
- 장점
    - 평균 대기시간을 초기화 할 수 있음
    - 시스템 내 프로세스 수를 최소화할 수 있음
    - 스케줄링의 부하도 감소하여 메모리 절약을 하여 시스템 효율이 향상된다.
    - 많은 프로세스들에게 빠른 응답 시간을 제공할 수 있다!
- 단점
    - Starvation (무한 대기) 현상 발생
    - BT가 긴 프로세스는 일찍 wait 상태에 들어가더라도 자원을 할당 받지 못 할 수 있음
        - Aging 등으로 해결 (HRRN)
    - 정확한 실행시간을 알 수 없어 실행시간 예측 기법이 필요하다

### 4) SRTN (Shortest Remaining Time Next)

- SPN의 변형
- Preemptive scheduling
- 잔여 실행 시간이 더 적은 프로세스가 Ready 상태가 되면 선점됨
- 장점
    - SPN의 장점 극대화
- 단점
    - 프로세스 생성 시, 총 실행 시간 예측이 필요함
    - 잔여 실행을 계속 추적해야 함 = overhead 발생
    - context switching overhead
- 구현 및 사용이 비현실적이다

### 5) HRRN (High-Response-Ratio-Next)

- SPN의 변형 (Aging을 통해 기아 현상을 막는다)
- non-preemptive scheduling
- Aging Concepts : 프로세스의 대기 시간을 고려하여 기회를 제공 (노약자 배려)
- 스케줄링 기준: Response ratio가 높은 프로세스 우선
- Response ratio = (WT+BT/BT) = (waiting time + burst time) / burst time
    - SPN의 장점 + Starvation 방지
    - 실행 시간 예측 기법이 필요하다는 단점이 여전히 존재

<img width="796" alt="summary" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/fd921416-ff84-4841-88f2-e4dd4c1e62f0">

### 6) MLQ (Multi-level Queue)

- 작업 (or 우선순위)별 별도의 ready queue를 가짐
    - 최초 배정 된 queue를 벗어나지 못함
    - 각각의 queue는 자신만의 스케줄링 기법을 사용함
- Queue 사이에는 우선순위 기반의 스케줄링을 사용한다.
    - Fixed-priority preemptive scheduling
- 장점
    - 빠른 응답 시간
- 단점
    - 여러 개의 Queue 관리 등 스케줄링 overhead
    - 우선순위가 낮은 queue는 startvation 현상 발생 가능

<img width="716" alt="MultiLevelQueue" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/c8c7d65a-5ae5-40f1-9ac0-9e8f8eda159c">

### 7) MFQ (Multi-level Feedback Queue)

- 프로세스의 Queue간 이동이 허용된 MLQ (MLQ의 queue 이동 불가 단점 보완)
- Feed back을 통해 우선 순위 조정
    - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
    - Dynamic priority: 동적인 우선순위
    - Preemptive Scheduling
    - Favor short burst-time processes
    - Favor I/O bounded processes
    - Improve adaptability
- 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기반의 효과를 볼 수 있음 (BT 예상없이 비슷한 효과!)
<img width="752" alt="MultiLevelFeedbackQueue" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/04530761-68e9-46b7-97fb-8c6960a80ef1">

- 단점
    - 설계 및 구현이 복잡하고, 스케줄링의 overhead가 크다
    - Starvation 문제등이 존재
- 변형(장점)
    - 각 준비 큐마다 시간 할당량을 다르게 배정한다.
        - 프로세스의 특성에 맞는 형태로 시스템 운영이 가능함
    - 입출력 위주 프로세스들을 상위 단계로 큐로 이동하여 우선순위를 높인다.
        - 프로세스가 block될 때 상위의 준비 큐로 진입하게 함
        - 시스템 전체의 평균 응답 시간을 줄임
        - 입출력 작업 분산
    - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동시킨다.
        - 에이징 기법
