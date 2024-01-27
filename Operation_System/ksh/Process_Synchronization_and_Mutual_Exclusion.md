# Process Synchronization (동기화)
- 다중 프로그래밍 시스템
  - 여러개의 프로세스들이 존재
  - 프로세스들은 서로 독립적으로 동작
  - 공유 자원 또는 데이터가 있을 때, 문제 발생 가능
 
- 동기화 (Synchronization)
  - 프로세스들이 서로 동작을 맞추는 것
  - 프로세스들이 서로 정보를 공유하는 것

- 비동기적 (Asynchronous)
  - 프로세스들이 서로에 대해 모름
- 병행적 (Concurrent)
  - 여러개의 프로세스들이 동시에 시스템에 존재
- 병행 수행적인 비동기적 프로세스들이 공유 자원에 동시 접근할 때 문제가 발생할 수 있음

- Shared data(공유 데이터): 여러 프로세스들이 공유하는 데이터
- Critical section(임계 영역): 공유 데이터를 접근하는 코드 영역
- Mutual exclusion(상호배제): 둘 이상의 프로세스가 동시에 임계 영역에 진입하는 것을 막는 것

- 기계어 명령의 특성
  - 원자성, 분리불가능
  - 한 기계어 명령의 실행 도중에 interrupt(방해) 받지 않음
 
- Race condition: 프로세스의 실행 순서에 따라 실행 결과가 달라질 수 있다.
- Race condition을 막아 원하는 결과를 보장하기 위한 것: **상호 배제**

## Mutual Exclusion (상호 배제)
### Methods
- Mutual exclusion primitives (기본 연산)
  - enterCS() primitive
    - Critical section 진입 전 검사
    - 다른 프로세스가 critical section 안에 있는지 검사
  - exitCS() primitive
    - Critical section을 벗어날 때의 후처리 과정
    - Critical section을 벗어남을 시스템이 알림
- 요구사항
  - 상호배제: 임계 영역에 프로세스가 있으면 다른 프로세스의 진입을 금지
  - 진행: 임계 영역 안에 있는 프로세스 외에는 다른 프로세스가 임계 영역에 진입하는 것을 방해하면 안됨
  - 한정 대기: 프로세스의 CS 진입은 유한시간 내에 허용되어야 함

- Dekker's Algorithm: Two process 상호 배제를 보장하는 최초의 알고리즘
  - turn과 flag를 통해 상호 배제를 보장함
- Peterson's Algorithm
  - Dekker 알고리즘보다 간단하게 구현됨

### N-Process 상호 배제
- 다익스트라 알고리즘
  - 최초로 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결
  - 실행 시간이 가장 짧은 프로세스에 프로세서를 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공

- SW solution들의 문제점
  - 속도가 느림
  - 구현이 복잡함
  - 상호배제 primitive 실행 중에 선점될 수 있음
    - 공유 데이터 수정중에 interrupt를 억제함으로써 해결 가능 -> overhead 발생
  - Busy Waiting 발생 (프로세스를 시작하지 못하는 상태)
