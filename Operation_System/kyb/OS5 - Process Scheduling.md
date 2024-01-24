# 프로세스 스케줄링

현재 시스템은 여러 개의 프로세스가 존재하는, 다중 프로그래밍 환경

때문에, 자원을 할당 할 프로세스를 선택해야 함 (스케줄링)

**자원 관리**

- 시분할(time-sharing) 관리
    - 하나의 자원을 여러 스레드들이 번갈아 가며 사용
    - **프로세스 스케줄링**
        - 프로세서 사용시간을 프로세스들에게 분배
- 공간 분할(space-sharing) 관리
    - 하나의 자원을 분할하여 동시에 사용
        - ex) memory
            

## 스케줄링의 목적

- 시스템의 성능 향상
- 대표적 시스템 성능 지표
    - 응답 시간
        - 대화형 시스템, real-time 시스템
    - 작업 처리량
        - 일괄 처리 시스템
    - 자원 활용도
        - 고성능 장비
- **목적**에 맞는 지표를 고려하여 스케줄링 기법 선택

## 대기시간, 응답시간, 반환시간

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/bb98a55c-d898-456e-87d1-539f56f21085)


- 대기 시간: 프로세스 도착 → 실행 시작까지 걸린 시간
- 응답 시간: 프로세스 도착 → 응답까지 시간
- 실행 시간: 실행 시작 → 실행 종료까지 걸린 시간
- 반환 시간: 프로세스 도착 → 실행 종료까지 걸린 시간

## 스케줄링 기준

- 프로세스의 특성
    - I/O-bounded or compute-bounded
- 시스템 특성
    - 일괄처리 시스템 or 대화형 시스템
- 프로세스의 긴급성
    - hard or soft realtime, non-real time system
- 프로세스 우선순위
- 프로세스 총 실행 시간

**CPU burst vs I/O burst**

- 프로세스 수행 = (CPU 사용 + I/O 대기)
- compute-bounded
    - CPU 사용 시간(연산) 이 더 많은 경우
- I/O-bounded
    - I/O가 더 많은 경우
    

## 스케줄링 단계

### Long-term scheduling

- Job scheduling
    - 시스템에 제출 할 작업 결정
- 다중프로그래밍 정도 조절
    - 시스템 내 프로세스 수 조절
- I/O-bounded와 compute-bounded 프로세스들을 잘 섞어서 선택해야 함
- 시분할 시스템에서는 모든 작업을 시스템에 등록
    - Long-term scheduling이 덜 중요
    

### Mid-term scheduling

- 메모리 할당 결정
- Swapping(swap-in / swap-out)

### Short-term scheduling

- **Process scheduling**
    - 프로세서를 할당할 프로세스를 결정
- 가장 빈번하게 발생
    - 인터럽트, block, time-out 등
    - 매우 빨라야 함

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/0254b7a9-4f71-4460-a523-1062dc0ffb09)


## 스케줄링 정책(Policy)

### 선점 / 비선점

- 비선점 스케줄링(Non-preemptive)
    - 할당 받을 자원을 스스로 반납할 때 까지 사용
        - ex) System call, I/O 등
    - 장점
        - Context switch overhead가 적음
    - 단점
        - 잦은 우선순위 역전, 평균 응답 시간 증가
- 선점 스케줄링(Preemptive)
    - 타의에 의해 자원을 빼앗길 수 있음
        - ex) 할당 시간 종료, 우선순위 높은 프로세스 등장
    - 시분할 시스템, 실시간 시스템 등에 적합
    - Context switch overhead가 큼

### 우선순위(Priority)

- 정적 우선순위(Static)
    - 프로세스 생성 시 결정된 우선순위가 유지됨
    - 구현이 쉽고, overhead가 적음
    - 시스템 환경 변화에 대한 대응 어려움

- 동적 우선순위(Dynamic)
    - 프로세스 상태 변화에 따라 우선순위 변경
    - 구현이 복잡, 우선순위 재계산으로 인한 overhead가 큼
    - 시스템 환경 변화에 유연한 대응 가능

 # 기본 프로세스 스케줄링 기법

## 1. FCFS(First-Come-First-Service)

먼저 들어온 순서(선착순)대로 스케줄링하는 기법

- 비선점 스케줄링
- 스케줄링 기준
    - 도착 시간(Ready queue 기준)
    - 먼저 도착한 프로세스를 먼저 처리
- 자원을 효율적으로 처리 가능
    - 오는대로 스케줄링에 던져주면 됨, 불필요한 스케줄링 오버헤드가 적음(낮음), cpu가 계속 일할 수있음
- 일괄처리(batch) 시스템에 적합, 대화형(interactive) 시스템에 부적함
- 단점
    - **Convoy effect**
        - 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기 시간을 갖게 되는 현상(대기 시간 >> 실행 시간)
    - 긴 평균 응답시간

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/b606702d-5a5c-4ff4-b4f5-8c163f0f43a0)



## 2. RR(Round-Robin)

일정 시간마다 돌아가며 스케줄링하는 기법

- 비선점 스케줄링
- 스케줄링 기준
    - 도착 시간(Ready-queue 기준)
    - 먼저 도착한 프로세스를 먼저 처리
- 자원 사용 제한 시간이 있음
    - 시스템 파라미터
    - 프로세스는 할당된 시간이 지나면 자원 반납
        - Timer-runout
    - 특정 프로세스의 자원 독점(monopoly) 방지
    - Context switch overhead가 큼
- 대화형, 시분할 시스템에 적합
- **Time quantum(시간 할당량)**이 시스템 성능을 결정하는 핵심 요소
    - 매우 큰 경우 → FCFS
    - 매우 작은 경우 → processor sharing
        - 사용자는 모든 프로세스가 각각의 프로세서 위에서 실행되는 것처럼 느낌
            - 체감 프로세서 속도 = 실제 프로세서 성능의 1/n
        - 높은 Context switch overhead
    
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/e4af5673-ddbb-42fa-bfef-60926640e504)

    

## 3. SPN(Shortest-Process-Next)

Burst time이 적은 프로세스 순으로 스케줄링

- 비선점 스케줄링
- 스케줄링 기준
    - Burst time 가장 작은 프로세스를 먼저 처리
    - SJF(Shortest Job first) scheduling
- 장점
    - 평균 대기시간(WT) 최소화
    - 시스템 내 프로세스 수 최소화
        - 스케줄링 부하 감소, 메모리 절약 → 시스템 효율 향상
    - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
    - Starvation(기아, 무한 대기) 현상 발생
        - Burst time이 긴 프로세스는 자원을 할당 받지 못할 수 있음 → Aging 등으로 해결해야 함
        - 정확한 실행 시간을 알 수 없음 → 실행 시간 예측 기법이 필요
    
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/d828bc09-d2b1-4148-a660-e98a32df6829)

    

## 4. SRTN(Short Remaining Time Next)

- SPN의 변형
- 비선점 스케줄링
    - 잔여 실행 시간이 더 적은 프로세스가 ready 상태가 되면 선점됨
- SPN의 장점 극대화
- 단점
    - 프로세스 생성 시, 총 실행 시간 예측이 필요함
    - 잔여 실행을 계속 추적해야 함 = overhead
    - Context switch overhead
- **현실적으로, 구현 및 사용은 어려움**

## 5. HRRN(High-Response-Ratio-Next)

- SPN의 변형
    - SPN + **Aging concepts**, 비선점 스케줄링
- Aging concepts
    - 프로세스의 대기 시간(WT)을 고려하여 기회를 제공
- 스케줄링 기준: Criteria
    - Response ratio가 높은 프로세스 우선
- Response ratio(응답률) = (WT + BT) / BT
    - SPN의 장점 + Starvation 방지
    - 실행 시간 예측 기법 필요
    
    

## 6. MLQ(Multi-level Queue)

- 작업(or 우선순위)별 별도의 ready queue를 가짐
    - 최초 배정된 queue를 벗어나지 못함
    - 각각의 queue는, 자신만의 스케줄링 기법 사용
- Queue 사이에는 우선순위 기반의 스케줄링 사용
- 장점: 빠른 응답 시간
- 단점
    - 여러 개의 Queue 관리 등 스케줄링 overhead
    - 우선순위가 낮은 queue는 Starvation 현상 발생 가능
    
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/535f7a13-7b99-4a24-b54b-c7d50c26e1aa)

    

## 7. MFQ(Multi-level Feedback Queue)

- 프로세스의 queue간 이동이 허용된 MLQ
- Feedback을 통해 우선 순위 조정
    - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
    - 동적 우선순위
    - 비선점 스케줄링
    - Favor short burst-time processes
    - Favor I/O bounded processes
    - Improve adaptability
- 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있음

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/6b3c7242-dd64-4d2b-ab62-665dc79f8c0d)


- 단점
    - 설계 및 구현이 복잡, 스케줄링 overhead가 큼
    - Starvation 문제 등
- 변형
    - 각 준비 Queue마다 시간 할당량을 다르게 배정
        - 프로세스의 특성에 맞는 형태로 시스템 운영 가능
    - 입출력 위주 프로세스들을 상위 단계의 큐로 이동, 우선 순위 높임
        - 프로세스가 block될 때 상위의 준비 큐로 진입하게 함
        - 시스템 전체의 평균 응답 시간 줄임, 입출력 작업 분산 시킴
    - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동
        - Aging 기법
