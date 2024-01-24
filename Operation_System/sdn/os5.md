# 스케쥴링 알고리즘

## FCFS(First-Come-First-Service)

- Non-preemptive scheduling
- 스케줄링 기준(Criteria)
    - 도착 시간(ready queue기준)
    - 먼저 도착한 프로세스를 먼저 처리
- 자원을 효율적으로 사용 가능
    - High resource utilization
    - scheduling overhead가 낮다.
    - 프로세서가 계속 일할 수  있다.
- Batch system에 적합, interactive system(대화형 시스템)에 부적합
- 단점
    - convoy effect
        - 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게 되는 현상(대기시간 >> 실행 시간)
    - 긴 평균 응답시간(response time)
        
        

## RR(Round-Robin)

- Preemptive scheduling
- 스케줄링 기준(criteria)
    - 도착 시간(ready queue기준)
    - 먼저 도착한 프로세스를 먼저 처리
- 자원 사용 제한 시간(time quantum)이 있음
    - System parameter
    - 프로세스는 할당된 시간이 지나면 자원 반납
        - Timer-runout
    - 특정 프로세스의 자원 독점 방지
    - Context switch overhead가 큼
- 대화형, 시분할 시스템에 적합
- Time quantum이 시스템 성능을 결정하는 핵심 요소
    - 제한 시간이 매우 크다면 → FCFS
    - 제한 시간이 매우 작으면 → processor sharing
        - 사용자는 모든 프로세스가 각각의 프로세서 위에서 실행되는 것처럼 느낌
            - 체감 프로세서 속도 = 실제 프로세서 성능의 1/n
        - High context switch overhead

## SPN(Shortest-Process-Next)

- Non-preemptive scheduling
- 스케줄링 기준(Criteria)
    - 실행시간(burst time 기준)
    - Burst time 가장 작은 프로세스를 먼저 처리
        - SJF(Shortest Job First) scheduling
- 장점
    - 평균 대기시간(WT) 최소화
    - 시스템 내 프로세스 수 최소화
        - 스케줄링 부하 감소, 메모리 절약 → 시스템 효율 향상
        - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
    - Starvation (기아현상, 무한대기)현상 발생
        - BT가 긴 프로세스는 자원을 할당 받지 못 할 수 있음
            - Aging 등으로 해결(HRRN)
        - 정확한 실행시간을 알 수 없음
            - 실행시간 예측 기법이 필요

## SRTN (Shortest Remaining Time Next)

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

## HRRN (Hight-Response-Ratio-Next)

- SPN의 변형
    - SPN + Aging concepts, Non-preemptive scheduling
- Aging concepts
    - 프로세스의 대기 시간(wt)을 고려하여 기회를 제공
- 스케줄링 기준 (Criteria)
    - Response ratio가 높은 프로세스 우선
- Response ratio = (WT+BT)/BT (응답률)
    - SPN의 장점 + Starvation 방지
    - 실행 시간 예측 기법 필요 (overhead)

### SPN,  SRTN, HRRN의 실행 시측

시스템의 효율성과 성능을 높이기 위해 실행 시간 예측을 해야 한다.

실제 실행 시간을 예측하기 어려움으로 예측 없이 비슷한 효과를 가지는 알고리즘으로 MLQ, MFQ가 나온다.

## MLQ (Multi-level Queue)

- 작업(or 우선순위)별 별도의 ready queue를 가진다.
    - 최초 배정 된 queue를 벗어나지 못함
    - 각각의 queue는 자신만의 스케줄링 기법 사용
- Queue사이에는 우선순위 기반의 스케줄링 사용
    - fixed-priority preemptive scheduling
- 장점
    - 우선순위가 높은 작업은 빠른 응답시간을 가진다.
- 단점
    - 여러 개의 Queue 관리 등 스케줄링 overhead
    - 우선순위가 낮은 queue는 starvation 현상 발생 가능
    

## MFQ (Multi-level Feedback Queue)

- 프로세스의 Queue간 이동이 허용된 MLQ
    - 최초 배정된 queue를 벗어나지 못한 단점을 개선한것
- Feedback을 통해 우선 순위 조정
    - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
    - Dynamic priority
    - Preemptive scheduling
    - Favor short burst-time processess
    - Favor I/O bounded Processes
    - Improve adaptablity

- 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있다.
- 단점
    - 설계 및 구현이 복잡, 스케줄링 overhead가 큼
    - 아래쪽에 있는 작업은 Starvation 문제 있음
- 변형
    - 각 준비 큐마다 시간 할당량을 다르게 배정
        - 프로세스의 특성에 맞는 형태로 시스템 운영 가능
    - 입출력 위주 프로세스들을 상위 단계의 큐로 이동, 우선 순위 높임
        - 프로세스가 block될 때 상위의 준비 큐로 진입하게 함
        - 시스템 전체의 평균 응답 시간 줄임, 입출력 작업 분산 시킴
    - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동
        - 에이징 (aging) 기법
