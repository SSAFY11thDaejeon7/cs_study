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
    
2. HW solution
   - TestAndSet(TAS) instruction
     - Test와 Set을 한번에 수행하는 기계어
     - 기계 명령으로 실행 중 interrupt를 받지 않음(preemption되지 않음)
  <br>
  
  - 장점
    - 구현이 간단
  - 단점
    - Busy waiting 문제 -> 비효율적
   
3. OS supported SW solution

## OS Supported SW solution
<h3>Spinlock</h3>

- 정수 변수
- P(), V() 연산으로만 접근 가능하며 S 정수 변수로 접근 가능여부 판단
- P() : 다른 작업이 실행되지 못 하도록 임계구역을 잠금
- V() : 다른 작업이 실행될 수 있도록 임계구역을 풀음
- 멀티 프로세서 시스템에서만 사용 가능 ( 아니면 P()에서 무한 반복 )
- Busy waiting 문제 존재
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/f5977fb7-d925-41c3-bee7-e9470a16cdad)
<br><br>

<h3>Semaphore</h3>

- Spinlock과 비슷하게 정수형 변수 S, 초기화 연산 P(), V() 함수 사용
- 임의의 S 변수 하나에 ready queue 하나가 할당 됨
- 프로세스가 실행을 멈추고 다른 작업을 기다리는 행위를 반복문을 통해 busy waiting하지 않음
- ready queue 사용으로 대기 중인 다른 작업을 올려둠으로서 busy waiting 문제 해결
- 종류
  - 1) Binay semaphore
    - S가 0과 1 두 종류의 값만 갖는 경우
    - 상호배제나 프로세스 동기화의 목적으로 사용
  - 2) Counting semaphore
    - S가 0 이상의 정수값을 가질 수 있는 경우
    - 생산자 소비자 문제 해결을 위해 사용
<br>

- Semaphore로 해결이 가능한 동기화 문제들
  - 상호배제 문제 : S, P(), V() 를 사용해, 프로세스가 실행중이라면 임계 구역에 다른 작업이 들어오지 못하도록 함
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/f5977fb7-d925-41c3-bee7-e9470a16cdad)
  - 프로세스 동기화 문제 : Process들이 병행적이며 비동기적으로 수행될 때, 실행 순서를 맞춰준다.
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/d17b2de2-29eb-4df6-8991-15dd2c8bf9f3)
  - 생산자 소비자 문제 : 생산자가 메시지를 생성해내는 도중에 소비자가 메시지를 가져가지 못 하도록 세마포어가 막아준다.
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/63949848-70dc-4fec-8b43-b41519843065)
  - Reader writer 문제 : Reader에는 동시에 여러명이 접근해도 되지만, Reader가 데이터를 읽는 도중 Writer되거나,
      - Writer가 데이터를 쓰는 도중 Reader가 데이터를 읽어가면 문제가 발생한다.
      - 따라서 Reader로 데이터를 읽을 때는 Writer가 데이터에 접근하지 못하도록 한다.
      - 또 Writer가 데이터를 쓰는 도중에는 Reader가 데이터에 접근하지 못하도록 해주는 기능을 수행한다.

<br>

- Semaphore 특징
  - 프로세서 할당을 위해 기다리는 프로세스는 큐에 asleep 상태가 되어있기 때문에 busy waiting이 발생하지 않음
  - 큐에서 꺼내지는 프로세스는 무작위이므로, 특정 작업이 계속해서 호출되지 않는 startvation 문제가 발생할 수 있음

