## 다중 프로그래밍 (Multi-programming)
- 여러개의 프로세스가 시스템 내에 존재함
- 자원을 할당할 프로세스를 선택해야 함
  - **스케줄링(Scheduling)**
- 자원 관리
  - 시간 분할 관리
    - 하나의 자원을 여러 스레드들이 번갈아 가며 사용
    - 프로세스 스케줄링
      - 프로세서 사용 시간을 프로세스들에게 분배
  - 공간 분할 관리
    - 하나의 자원을 분할하여 동시에 사용
<br>

## 스케줄링의 목적
- 시스템의 성능 향상
- 대표적인 시스템의 성능 지표
  - 응답시간 -> interactive / real-time system에서 중요
    - 작업 요청으로부터 응답을 받을때까지의 시간
  - 작업 처리량 -> batch system에서 중요
    -단위 시간동안 완료된 작업의 수
  - 자원 활용도 -> 비싼 장비를 샀다면 유휴 시간을 줄이는 게 좋음
    - 주어진 시간동안 자원이 활용된 시간
- 목적에 맞는 지표를 고려하여 스케줄링 기법을 선택
<br>

## 대기시간, 응답시간, 반환시간

![4일차-5](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/e61f772e-543a-494d-856e-d20500081ea7)

- 대기시간: 프로세스가 도착하고 실행을 시작하기 전까지의 시간
- 응답시간: 프로세스가 도착하고 첫번째 출력을 하기까지의 시간
- 실행시간: 프로세스가 실제로 실행된 시간
- 반환시간: 프로세스가 도착하고 실행이 종료될 때까지의 시간
<br>

## 스케줄링 기준
> 스케줄링 기법이 고려하는 항목들

- 프로세스의 특성
  - I/O-bounded or compute-bounded
  - I/O 대기 시간이 더 많은지, CPU 사용 시간이 더 많은지
- 시스템 특성
  - Batch system or interactice system
- 프로세스의 긴급성
  - Hard- or soft- real time, non-real time systems
- 프로세스 우선순위
- 프로세스 총 실행 시간
<br>

## 스케줄링의 단계
> 발생하는 빈도 및 할당 자원에 따른 구분

- 장기 / 중기 / 단기 스케줄링
<br>

### Long-term Scheduling
- Job scheduling
  - 시스템에 제출할 작업 결정
- 다중프로그래밍 정도 조절
  - 시스템 내에 프로세스 수 조절
- I/O-bounded 와 compute-bounded 프로세스들을 잘 섞어서 선택해야 함
- 시분할 시스템에서는 모든 작업을 시스템에 등록
  - Long-term scheduling이 불필요
<br>

## Mid-term Scheduling
- 메모리 할당 결정
- suspended ready -> ready 상태로 이동할 때
<br>

## Short-term Scheduling
- Process scheduling
  - Low-level scheduling
  - 프로세서를 할당할 프로세스를 결정
- ready -> running 상태로 이동할 때
- 가장 빈번하게 발생
  - 매우 빨라야 함
<br>

## 스케줄링의 단계

![4일차-6](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/3b866ddd-29d4-473d-9d04-a699471bc6e3)
<br>

## 스케줄링 정책

### Preemptive (선점) / Non-preemptive (비선점)
- 비선점 스케줄링
  - 할당 받을 자원을 스스로 반납할 때까지 사용
  - 장점
    - Context switch overhead가 적음
  - 단점
    - 잦은 우선순위 역전, 평균 응답 시간 증가
- 선점 스케줄링
  - 타의에 의해 자원을 빼앗길 수 있음
  - Context switch overhead가 큼
  - Time-sharing system, real-time system 등에 적합
<br>

### Priority (우선순위)
- Static priority (정적 우선순위)
  - 프로세스 생성시 결정된 우선순위가 유지됨
  - 구현이 쉽고, overhead가 적음
  - 시스템 환경 변화에 대한 대응이 어려움
 
- Dynamic priority (동적 우선순위)
  - 프로세스의 상태 변화에 따라 priority 변경
  - 구현이 복잡, priority 재계산 overhead가 큼
  - 시스템 환경 변화에 유연한 대응 가능
<br>

## 기본 스케줄링 알고리즘들

### FCFS (First-Come-First-Service)
> 선착순 알고리즘
- 비선점 스케줄링
- 스케줄링 기준
  - 도착 시간
  - 먼저 도착한 프로세스를 먼저 처리
- 자원을 효율적으로 사용 가능
- Batch system에 적합, interactive system에 부적합
- 단점
  - 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게될 수 있음
  - 긴 평균 응답시간
<br>

## RR (Round-Robin)
> 돌아가며 프로세스를 쓰는 알고리즘
- 선점 스케줄링
- 스케줄링 기준
  - 도착 시간
  - 먼저 도착한 프로세스를 먼저 처리
- **자원 사용 제한 시간이 있음**
  - System parameter
  - 프로세스는 할당된 시간이 지나면 자원 반납
  - 특정 프로세스의 자원 독점 방지
  - Context switch overhead가 큼
- 대화형, 시분할 시스템에 적합
- 제한 시간이 시스템 성능을 결정하는 핵심 요소
  - 제한 시간이 매우 큰 경우: FCFS
  - 제한 시간이 매우 작은 경우: processor sharing
<br>

## SPN (Shortest-Prcoess-Next)
- 비선점 스케줄링
- 스케줄링 기준
  - 실행시간
  - 실행시간이 가장 작은 프로세스를 먼저 처리
- 장점
  - 평균 대기시간(WT) 최소화
  - 시스템 내 프로세스 수 최소화
    - 스케줄링 부하 감소, 메모리 절약 -> 시스템 효율 향상
  - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
  - Starvation(무한대기) 현상 발생
    - 실행시간이 긴 프로세스는 자원을 할당받지 못 할 수도 있음
      - Aging 등으로 해결(ex. HRRN)
  - 정확한 실행시간을 알 수 없음
    - 실행시간 예측 기법이 필요
<br>

## SRTN (Shortest-Remaining-Time-Next)
> SPN의 변형
- 선점 스케줄링
  - 잔여 실행 시간이 더 적은 프로세스가 ready 상태가 되면 선점됨
- 장점
  - SPN의 장점 극대화
- 단점
  - 프로세스 생성시, 총 실행 시간 예측이 필요함
  - 잔여 실행을 계속 추적해야 함 = overhead
  - Context switching overhead

## HRRN (High-Response-Ratio-Next)
- SPN의 변형. 기아 현상을 해결
- Aging 컨셉트를 가지고 있음
- 비선점 스케줄링
- Aging concepts
  - 프로세스의 대기 시간을 고려하여 기회를 제공
- 스케줄링 기준
  - 응답률이 높은 프로세스 우선
- 응답률 (Response ratio = (대기시간 + 실행시간) / 실행시간
  - SPN의 장점 + 기아현상 방지
  - 실행 시간 예측 기법이 필요함

## MLQ (Multi-level Queue)
- 작업(or 우선순위)별 별도의 ready queue를 가짐
  - 최초 배정된 queue를 벗어나지 못함
  - 각각의 queue는 자신만의 스케줄링 기법 사용
- Queue 사이에는 우선순위 기반의 스케줄링 사용
- 장점
  - 중요한 작업들은 빠르게 처리할 수 있음
- 단점
  - 여러 개의 큐 관리등 스케줄링 overhead 발생
  - 우선순위가 낮은 큐는 기아현상 발생 가능

## MFQ (Multi-level Feedback Queue)
- 프로세스의 Queue간 이동이 허용된 MLQ
- Feedback을 통해 우선 순위 조정
  - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
  - 우선순위가 동적임
  - 선점 스케줄링
- 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있음
- 단점
  - 설계 및 ㄱ현이 복잡, 스케줄링 overhead가 큼
  - 기아현상 발생
- 변형
  - 각 준비 큐마다 시간 할당량을 다르게 배정
  - 입출력 위주 프로세스들을 상위 단계 큐로 이동해서 우선순위를 높임
  - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동


