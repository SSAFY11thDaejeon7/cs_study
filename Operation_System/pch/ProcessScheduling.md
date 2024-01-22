## 다중프로그래밍 ( Multi-Programming )
  - 여러개의 프로세스가 시스템 내 존재
  - 자원을 할당 할 프로세스를 선택해야 함
    - 스케줄링
  - 자원 관리
    - 시간 분할 (time sharing) 관리
      - 하나의 자원을 여러 스레드들이 번갈아 가며 사용 ex) Processor
      - 프로세스 스케줄링 : 프로세서 사용시간을 프로세스들에게 분배
    - 공간 분할 (space sharing) 관리
      - 하나의 자원을 분할하여 동시에 사용 ex) 메모리
<br><br>
     
## 스케줄링의 목적
  - 시스템 성능(performance) 향상
  - 대표적 시스템 성능 지표
    - 응답시간 (response time) : 작업 요청으로부터 응답을 받을때까지의 시간
    - 작업 처리량(throughput) : 단위 시간 동안 완료된 작업의 수
    - 자원 활용도(resource utilization) : 주어진 시간동안 자원이 활용된 시간
  - 목적에 맞는 지표를 고려하여 스케줄링 기법을 선택한다.
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/95f9e4a2-5276-41a8-aa33-6001bf55ff21" width="800px;" alt=""/>
<br><br><br>

## 스케줄링 기준 및 단계
<h3>1. 스케줄링 기준 (Criteria)</h3>

  - 스케줄링 기법이 고려하는 항목들
  - 프로세스의 특성 : I/O-bounded or compute-bounded
  - 시스템 특성 : Batch system or interactive system
  - 프로세스의 긴급성 : Hard or soft real time, non real time systems
  - 프로세스 우선순위
  - 프로세스 총 실행 시간 등
<br><br>

<h3>2. CPU burst vs I/O burst</h3>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/0b18a722-568e-4eb0-b643-06a1bb131292" width="300px;" alt=""/>
<br><br><br>

  - 프로세스 수행 = CPU 사용 + I/O 대기
  - CPU burst : CPU 사용 시간
  - I/O burst : I/O 대기 시간
  - CPU burst > I/O burst : compute-bounded
  - CPU burst < I/O burst : I/O bounded
  - Burst time은 스케줄링의 중요한 기준 중 하나

<h3>3. 스케줄링 단계</h3>

  - 발생하는 빈도 및 할당 자원에 따른 구분
  1) Long term scheduling
    - 장기 스케줄링
    - Job scheduling
  2) Mid term scheduling
    - 중기 스케줄링
    - Momory allocation
  3) Short term scheduling
    - 단기 스케줄링
    - Process scheduling
<br><br>

<h4>Long-term Scheduling</h4>
  - Job scheduling : 시스템에 제출할 작업 결정 ( 프로세스로 만듦 )
  - 다중 프로그래밍 정도 조절 : 시스템 내의 프로세스 수 조절
  - I/O bounded와 compute bounded 프로세스들을 잘 섞어서 선택해야 함
    - 적절히 섞어주지 않으면, CPU가 오래 쉬거나 I/O 입력을 받지 못하는 경우가 발생한다. (효율 저하)
  - 시분할 시스템에서는 모든 작업을 시스템에 등록
    - 따라서 Long term scheduling이 상대적으로 덜 중요
<br><br>

<h4>Mid-term Scheduling</h4>

  - 메모리 할당 결정
  - Swapping (swap-in/swap-out) : 프로세스 상태가 메모리 할당 여부에 따라 ready, suspended 상태로 변환하는 과정
<br><br>

<h4>Short-term Scheduling</h4>

  - Process scheduling : 프로세서를 할당할 프로세스를 결정
  - 가장 빈번하게 발생
    - 성능에 가장 큰 영향을 미치므로 매우 빨라야함
    - Interrupt, block (I/O), time-out 등
<br><br>

## 스케줄링 정책
<h3>Preemptive scheduling vs Non-preemptive Scheduling</h3>

  1. 비선점 스케줄링
      - 할당받을 자원을 스스로 반납할 때까지 사용
      - Context switch overhead가 적음
      - 잦은 우선순위 역전(대기시간 증가로 중요한 일이 시작 못함)으로 평균 응답 시간이 증가한다.
    <br>

  2. 선점 스케줄링
    - 타의에 의해 자원을 빼앗길 수 있음
      - 할당 시간이 종료되거나 우선선위가 높은 프로세스 등장으로 뺴앗김
      - context switch overhead가 크다
      - Time-sharing system, real tiem system 등 사욯작용이 많은 시스템에 적합
<br><br>

<h3>Priority</h3>

1. 정적 우선순위 ( static priority)
  - 프로세스 생성시 결정된 우선순위가 유지
  - 구현이 쉽고 overhead가 적음
  - 시스템 환경 변화에 대한 대응이 어려움
2. 동적 우선순위
  - 프로세스 상태 변화에 따라 우선순위 변경
  - 구현이 복잡, 우선순위 재계산 overhaed가 큼
  - 시스템 환경변화에 유연한 대응이 가능
    
