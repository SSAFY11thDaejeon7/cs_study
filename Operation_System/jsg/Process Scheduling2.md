# Scheduling Algorithms

## FCFS(First-Come-First-Service)
<img width="418" alt="스크린샷 2024-01-24 오후 9 35 27" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/dc6af899-aa3e-403d-addf-f44cc5e89542">

- Non-preemptive scheduling
- 스케줄링 기준 (Criteria)
  - 도착시간(ready queue 기준)
  - 먼저 도착한 프로세스를 먼저 처리
- 자원을 효율적으로 사용 가능
- Batch system에 적합, interactive system에 부적합
- 단점
  - Convoy effect
    - 하나의 수행시간 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게 되는 현상(대기시간 >> 실행시간)
  - 긴 평균 응답시간(response time)

<img width="425" alt="스크린샷 2024-01-24 오후 9 43 09" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d8e1cce4-031c-4ddd-bcd0-5b7dbbd3c5af">

***Normalized TT 는 상대적으로 기다린 시간**

## RR(Round-Robin)
<img width="477" alt="스크린샷 2024-01-24 오후 9 49 26" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/9114cd13-9d6b-4feb-ac74-05dad06edfe1">

- Preemptive scheduling
- 스케줄링 기준(Criteria)
  - 도착 시간(ready queue 기준)
  - 먼저 도착한 프로세스를 먼저 처리
- **자원 사용 제한 시간(time quantum)이 있음**
  - System parameter
  - 프로세스는 할당된 시간이 지나면 자원 반납
  - 특정 프로세스의 자원 독점(monopoly) 방지
  - Context switch overhead가 큼
- 대화형, 시분할 시스템에 적합
- Time quantum이 시스템 성능을 결정하는 핵심 요소
  - Time quantum이 무한대라면 FCFS와 다를게 없음
  - Time quantum이 0에 수렴한다면 동시에 쓰는 느낌이 나지만 컨텍스트 스위치의 오버헤드가 커진다.

<img width="503" alt="스크린샷 2024-01-24 오후 9 55 36" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/2255af55-7f14-4ff2-8e4d-d5dcb19b73fa">

**Average response time(TT) = 5+18+4+15+12/5 = 10.8**

## SPN(Shortest-Process-Next)
- Non-preemptive scheduling
- 스케줄링 기준(Criteria)
  - 실행시간(burst time 기준)
  - Burst time 가장 작은 프로세스를 먼저 처리
    - SJF(Shortest Job First) scheduling
- 장점
  - 평균 대기시간 최소화
  - 시스템 내 프로세스 수 최소화
    - 스케줄링 부하 감소, 메모리 절약 -> 시스템 효율 향상
  - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
  - Starvation(무한대기) 현상 발생
    - BT가 긴 프로세스는 자원을 할당 받지 못할 수 있음
  - 정확한 실행시간을 알 수 없음
    - 실행시간 예측 기법이 필요
<img width="484" alt="스크린샷 2024-01-24 오후 10 02 42" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/db598cb5-7653-4983-a7c8-296624eb6aef">

## SRTN(Shortest Remaining Time Next)
- SPN의 변형
- Preeamptive schedulig
  - 잔여 실행 시간이 더 적은 프로세스가 ready 상태가 되면 선점됨
- 장점
  - SPN의 장점 극대화
- 단점
  - 프로세스 생성시, 총 실행시간 예측이 필요함
  - 잔여 실행을 계속 추적해야 함 = overhead
  - Context switching overhead
  - 구현 및 사용이 비현실적이다.

## HRRN(High-Response-Ratio-Next)
- SPN의 변형
  - SPN + Aging concepts, Non-preemptive scheduling
- Aging concepts
  - 프로세스 대기 시간을 고려하여 기회를 제공
- 스케줄링 기준(Criteria)
  - Response ratio가 높은 프로세스 우선
- Response ratio = 대기시간 + 필요시간 / 필요시간
  - SPN의 장점 + Starvation 방지
  - 실행시간 예측 기법 필요(overhead)
<img width="499" alt="스크린샷 2024-01-24 오후 10 10 37" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/91f5750e-bd62-41f9-8a3c-5c98c86a7c1c">

#### FCFS, RR 은 공평성
#### SPN, SRTN, HRRN은 효율성 (실행시간 예측 부하)

## MLQ(Multi-lever Queue)
<img width="485" alt="스크린샷 2024-01-24 오후 10 14 50" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/64ab7589-0f22-4eaf-9901-628ec662351e">

- 작업별 별도의 ready queue를 가짐
  - 최초 배정된 queue를 벗어나지 못함
  - 각각의 queue는 자신만의 스케줄링 기법 사용
- Queue 사이에는 우선순위 기반의 스케줄링 사용
- 장점
  - 중요한 것들은 응답시간이 빠르다.
  - 결코 빠르기만한 응답시간은 아니다.
- 단점
  - 여러개의 Queue 관리 등 스케줄링 overhead
  - 우선순위가 낮은 queue는 starvation현상 발생 가능
 
## MFQ
<img width="505" alt="스크린샷 2024-01-24 오후 10 17 49" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/0c0ba879-65f7-4bc1-b36d-55a6b558ad55">

- 프로세스의 Queue간 이동이 허용된 MLQ
- Feedback를 통해 우선순위 조정
  - 현재까지의 프로세서 사용 정보(패턴) 활용
- 특성
  - Dynamic priority(동적 우선순위)
  - Preemptive scheduling(선점)
  - Favor short burst-time processes
  - Favor I/O bounded processes
  - Improve adaptability
- 장점
  - 프로세스에 대한 사전 정보 없이 SPN, SRTN, HRRN 기법의 효과를 볼 수 있음
- 단점
  - 설계 및 구현이 복잡, 스케줄링 overhead가 큼
  - Starvation 문제 등
- 변형
  - 각 준비 큐마다 시간 할당량을 다르게 배정
    - 프로세스의 특성에 맞는 형태로 시스템 운영 가능
  - 입출력 위주 프로세스들을 상위 단계의 큐로 이동, 우선 순위 높임
    - 프로세스가 block될 때 상위의 준비 큐로 진입하게 함
    - 시스템 전체의 평균 응답 시간을 줄임, 입출력 작업 분산시킴
  - 대기 시간이 지정된 시간을 초과한 프로세스들을 상위 큐로 이동
    - 에이징(aging) 기법
