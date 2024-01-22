# [OS] Lecture 5. Process Scheduling

## 다중프로그래밍 (Multi-programming)
- 여러개의 프로세스가 시스템 내 존재
- 자원을 할당할 프로세스를 선택해야함
  - 스케줄링(Scheduling)
- 자원 관리
  - 시간 분할 (time sharing)관리
    - 하나의 자원을 여러 스레드들이 번갈아 가며 사용
    - 예) 프로세서
    - 프로세스 스케줄링
      - 프로세서 사용시간을 프로세스들에게 분배
  - 공간 분할 (space sharing)관리
    - 하나의 자원을 분할하여 동시에 사용
    - 예) 메모리

## 스케줄링의 목적
- 시스템의 성능(performance) 향상
- 대표적 시스템 성능 지표(index)
  - 응답시간(response time)
    - 작업 요청(submission)으로부터 응답을 받을때까지의 시간
  - 작업 처리량(throughput)
    - 단위 시간 동안 완료된 작업의 수
  - 자원 활용도(resource utilization)
    - 주어진 시간(Tc) 동안 자원이 활용된 시간(Tr)
    - Utilization = Tr / Tc
- 목적에 맞는 지표를 고려하여 스케줄링 기법을 선택

## 시스템 성능 지표들
- 평균 응답 시간, 처리량, 자원 활용도, 공평성, 실행 대기 방지, 예측 가능성...

## 대기시간, 응답시간, 반환시간
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/23e3dd0e-edfe-497e-a3cf-fbce16c65b6a)

- 대기(Waiting) 시간: 도착 ~ 실행 시작
- 응답(Response) 시간: 도착 ~ 첫 번째 출력
- 반환(Turn around) 시간: 도착 ~ 실행 종료
- 실행(Burst) 시간: 실행 시작 ~ 실행 종료

## 스케줄링 기준 (Criteria)
- 스케줄링 기법이 고려하는 항목들
- 프로세스의 특성
  - **I/O-bounded** or **compute-bounded**
- 시스템 특성
  - **Batch system** or **interactive system**
- 프로세스의 긴급성(urgency)
  - **Hard-** or **Soft-** real time, **Non-** real time systems
- 프로세스 우선순위
- 프로세스 총 실행 시간

## CPU burst vs I/O burst
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/d414ab6e-1bc6-4d6d-a29e-3e62b70f532d)

- 프로세스 수행 = CPU 사용 + I/O 대기
- CPU burst: CPU 사용 시간
  - compute-bounded process
- I/O burst: I/O 대기 시간
  - I/O-bounded process
- Burst time은 스케줄링의 중요한 기준 중 하나

## 스케줄링의 단계 (Level)
- 발생하는 빈도 및 할당 자원에 따른 구분
- Long-term scheduling
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e01e957d-1e8e-4332-9565-b91b4594fa82)

  - 장기 스케줄링
  - Job scheduling
    - 시스템에 제출할(kernel에 등록할) 작업 결정
      - Admission scheduling, High-level scheduling
  - 다중프로그래밍 정도(degree) 조절
    - 시스템 내에 프로세스 수 조절
  - I/O-bounded와 compute-bounded 프로세스들을 잘 섞어서 선택해야 함
  - 시분할 시스템에서는 모든 작업을 시스템에 등록
    - Long-term scheduling이 불필요
- Mid-term scheduling
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/5672d478-bd6a-4558-90ec-16f291c91de7)

  - 중기 스케줄링
  - Memory allocation
    - Intermediate-level scheduling
    - Swapping (swap-in/swap-out)
- Short-term scheduling
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/cbec1331-a058-4252-88ff-1c8e8a835a34)

  - 단기 스케줄링
  - Process scheduling
    - Low-level scheduling
    - 프로세서를 할당할 프로세스를 결정
      - Processor scheduler, dispatcher
  - 가장 빈번하게 발생
    - Interrupt, block (I/O), time-out, Etc.
    - 매우 빨라야 함

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e3b52e94-19a1-46ed-a8e8-67ec547e3a84)


## 스케줄링 정책 (Policy)
- 선점 vs 비선점
  - Non-preemptive scheduling
    - 할당 받을 자원을 스스로 반납할 때까지 사용
      - 예) system call, I/O, Etc.
    - 장점: Context switch overhead가 적음
    - 단점: 잦은 우선순위 역전, 평균 응답 시간 증가
  - Preemptive scheduling
    - 타의에 의해 자원을 빼앗길 수 있음
      - 예) 할당시간 종료, 우선순위가 높은 프로세스 등장
    - Context switch overhead가 큼
    - Time-sharing system, real-time system 등에 적합
- 우선순위
  - Priority
    - 프로세스의 중요도
    - Static priority (정적 우선순위)
      - 프로세스 생성시 결정된 priority가 유지됨
      - 구현이 쉽고, overhead가 적음
      - 시스템 환경 변화에 대한 대응이 어려움
    - Dynamic priority (동적 우선순위)
      - 프로세스 상태 변화에 따라 priority 변경
      - 구현이 복잡, priority 재계산 overhead가 큼
      - 시스템 환경 변화에 유연한 대응 가능
