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
