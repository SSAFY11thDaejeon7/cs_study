# 프로세스 스케줄링

## 다중프로그래밍 (Multi-programming)
- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당 할 프로세스를 선택해야 함(스케줄링)
- 자원관리
  - 시간 분할(time sharing) 관리
    - 하나의 자원을 여러 쓰레드들이 번갈아 가며 사용
    - 예)프로세서(Processor)
    - 프로세스 스케줄링(Process scheduling)
      - 프로세서 사용시간을 프로세스들에게 분배
  - 공간 분할(space sharing) 관리
    - 하나의 자원을 분할하여 동시에 사용
    - 예) 메모리(memory)
## 스케줄링(Scheduling)의 목적
- 시스템의 성능(performance) 향상
#### 대표적 시스템 성능 지표(index)
- 응답시간(response time)  --> 대화형 시스템, real-time에 적합
  - 작업요청으로부터 응답을 받을때까지의 시간
- 작업 처리량(throughput) --> batch 시스템 적합
  - 단위 시간동동안 완료된 작업의 수
- 자원 활용도(resource utilization)
  - 주어진 시간(Tc)동안 자원이 활용된 시간(Tt)
**목적에 맞는 지표를 고려하여 스케줄링 기법을 선택**

<img width="514" alt="스크린샷 2024-01-23 오후 8 27 07" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/924e8414-d4de-4a1b-b1bc-5817a6206c2f">

- 대기시간(Wating time)
  - 프로세스가 실행을 시작하기까지 시간
- 응답시간(Burst time)
  - 작업이 수행되고 첫번째 출력이 나오는 시간
- 실행시간(Response time)
  - 실제로 프로세스가 실행된 시간
- 반환시간(Turn-around time)
  - 도착시간부터 실행 종료까지의 시간
 
## 스케줄링 기준(Criteria)
- 스케줄링 기법이 고려하는 항목들
- 프로세스(process)의 특성
  - I/O-bounded or compute-bounded
- 시스템 특성
  - Batch system or interactive system
- 프로세스의 긴급성(urgency)
- 프로세스 우선순위(priority)
- 프로세스 총 실행시간(total service time)

### CPU burst vs I/O burst
- 프로세서 수행 = CPU 사용 + I/O 대기 
- CPU burst (compute - bounded)
  - CPU 사용시간이 더 많음
- I/O burst (I/O - bounded)
  - I/O 대기시간이 더 많음

## 스케줄링의 단계 (Level)
- 발생하는 빈도 및 할당 자원에 따른 구분
- Long-term scheduling (가끔)
  - 장기 스케줄링
- Mid-term schedulig (종종)
  - 중기 스케줄링
- Short-term scheduling (자주)
  - 단기 스케줄링

#### Long-term Scheduling
- Job scheduling
  - 시스템에 제출 할(Kernel에 등록 할) 작업(Job) 결정
- 다중프로그래밍 정도(degree) 조절
  - 시스템 내에 프로세스 수 조절
- I/O - bounded와 compute-bounded 프로세스들을 잘 섞어서 선택해야 함
- 시분할 시스템에서는 모든 작업을 시스템에 등록
  - Long-term scheduling이 덜 중요하다.

#### Mid-term scheduling
- 메모리 할당 결정(memory allocation)
  - Intermediate-level scheduling
  - Swapping(swap-in/swap-out)
#### Short-term Scheduling
- Process scheduling
  - Low-level scheduling
  - 프로세서(processor)를 할당할 프로세스(process)를 결정
    - Processor scheduler, dispatcher
- 가장 빈번하게 발생
  - 매우 빨라야 함

<img width="514" alt="스크린샷 2024-01-23 오후 8 43 15" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/f1eecdd9-1c89-4a6b-a28e-158a297b8747">


## 스케줄링 정책(Policy)
- 선점 vs 비선점
- 우선순위
  - Priority
 
### Preemptive/Non-preemptive scheduling

- Non-preemptive scheduling (비선점)
  - 할당 받을 자원을 스스로 반납할 때까지 사용
    - 예) system call, I/O, Etc.
  - 장점
    - Context switch overhead가 적음
  - 단점
    - 잦은 우선순위 역전, 평균 응답시간 증가
   
 - Preemptive scheduling (선점)
   - 타의에 의해 자원을 빼앗길 수 있음
     - 예) 할당 시간 종료, 우선순위가 높은 프로세스 등장
   - Context switch overhead가 큼
   - Time-sharing system, real-time system 등에 적합
### Priority(우선순위)
- 프로세스의 중요도
- Static priority(정적 우선순위)
  - 프로세스 생성시 결정된 priority가 유지됨
  - 장점 : 구현이 쉽고 overhead가 적음
  - 단점 : 시스템 환경 변화에 대한 대응이 어려움
- Dynamic priority(동적 우선순위)
  - 프로세스의 상태 변화에 따라 priority 변경
  - 장점 : 시스템 환경 변화에 유연한 대응 가능
  - 단점 : 구현이 복잡, priority 재계산 overhead가 큼
