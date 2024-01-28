## Process Synchronization (동기화)
- 다중 프로그래밍 시스템
  - 여러 개의 프로세스들이 존재
  - 프로세스들은 서로독립적으로 동작
  - 공유 자원 또는 데이터가 있을 때, 문제 발생 가능
<br>

- 동기화(Synchronization)
  - 프로세스들이 서로 동작을 맞추는 것
  - 프로세스들이 서로 정보를 공유하는 것
<br>

## Asynchronous and Concurrent P's (비동기적이며 병행적인 프로세스들)
- 비동기적 : 프로세스들이 서로 어떻게 동작하는지 모름
- 병행적 : 여러 개의 프로세스들이 동시에 시스템에 존재
- 병행 수행중인 비동기적 프로세스들이 공유 자원에 동시 접근할 때, 문제가 발생할 수 있음
<br>

## 중요 단어
1. Shared data (공유 데이터, = Critical data)
  - 여러 프로세스들이 공유하는 데이터
<br>

2. Critical section (임계 영역)
  - 공유 데이터를 접근하는 코드 영역(code segment)
<br>

3. Mutual exclusion (상호 배제)
  - 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것
<br>

4. Machine instruction (기계어 명령)
   - 한 기계어 명령 실행중에는 인터럽트를 받지 않음
   - 원자성(Atomicity), 분리불가능(INdivisible)
<br>

5. Race condition
   - 실행 순서에 따라 결과가 달라지는 현상
<br>

## Mutual Exclusion Methods
1. enterCS() primitive
   - Critical section 진입 전 검사
   - 다른 프로세스가 ciritical section 안에 있는지 검사
<br>

2. exitCS() primitive
   - Critical section을 벗어날 때의 후처리 과정
   - Critical section을 벗어남을 시스템이 알림
<br>

## Requirements for Mutual Exclusion primitives
1. Mutual exclusion (상호배제)
   - Ciritical section(CS)에 프로세스가 있으면 다른 프로세스의 진입을 금지
2. Progress (진행)
   - CS안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨
3. Bounded waiting (한정대기)
   - 프로세스의 cS 진입은 유효시간 내에 허용되어야 함
<br>

## Mutual Exclusution Solutions
1. SW solutions
   - 문제점
     - 속도가 느림
     - 구현이 복잡함
     - Mutual Exclusion 실행 중 다른 프로세스에게 프로세서를 뺏길 수 있다.
       - 공유 데이터 수정중 인터럽트를 억제함으로 방지는 가능하나 overhead가 발생한다.
     - 임계 영역에 들어가기 전, 반복문을 통해 임계영역에 진입할 수 있는지 코드가 계속 돌아가 비효율적이다.(Busy waiting)
