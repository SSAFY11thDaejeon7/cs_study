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
