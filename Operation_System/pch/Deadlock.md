## Deadlock의 개념
- Blocked / Asleep state
  - 프로세스가 특정 이벤트를 기다리는 상태
  - 프로세스가 필요한 자원을 기다리는 상태
 
- Deadlock state
  - 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
  - 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
 
- Deadlock와 Starvation이 차이점
  : Deadlock은 이벤트를 계속해서 기다리는 상태이고, Startvation은 프로세서 할당을 계속해서 기다리는 상태이다.

  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/b16827c4-fac6-498f-bcab-9668b72f0ab8)
<br>

## 자원의 분류
- 일반적 분류
  - Hardware resources vs Software resources
- 다른 분류 법
    - 선점 가능 여부에 따른 분류
    - 할당 단위에 따른 분류
    - 동시 사용 가능 여부에 따른 분류
    - 재사용 가능 여부에 따른 분류
 
1. 선점 가능 여부에 따른 분류
   - Preemptible resources
     - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
     - Processor, memory 등
   - Non-preemptible resources
     - 선점 당하면, 이후 진행에 문제가 발생하는 자원
     - disk drive 등
<br>
    
2. 할당 단위에 따른 분류
  - Total allocation resources
    - 자원 전체를 프로세스에게 할당
  - Partitioned allocation resources
    - 하나의 자원을 여로 조각으로 나누어, 여러 프로세스들에게 할당
<br>

3. 동시 사용 가능 여부에 따른 분류
   - Exclusive allocation resources
       - 한 순간에 한 프로세스만 사용 가능한 자원
       - Processor, memory, disk drice 등
    - Shared allocation resource
       - 여러 프로세스가 동시에 사용 가능한 자원
       - Program, shared date 등
<br>

4. 재사용 가능 여부에 따른 분류
   - SR (Serialy-reusable Resources)
     - 시스템 내에 항상 존재하는 자원
     - 사용이 끝나면 다른 프로세스가 사용 가능
   - CR (Consumable Resources)
     - 한 프로세스가 사용한 후에 사라지는 자원
<br>

## Deadlock 발생 필요 조건
  - Exclusive use of resources
  - Non-preemptible resources
  - Hold and wait(Partial allocation)
  - Circular wait
  - 위 4개의 발생 조건 중 하나를 제거하면 Deadlock이 절대 발생하지 않음
<br>

## Deadlock 해결 방법
- Deadlock prevention methods : 교착상태 예방
- Deadlock avoidance mothod : 교착상태 회피
- Deadlock detection and deadlock recovery mothods : 교착상태 탐지 및 복구
<br>

## 교착상태 예방
1. 모든 자원을 공유 허용
  - Exclusive use of resoureces 조건 제거
  - 현실적으로 불가능
<br>

2. 모든 자원에 대한 선점 허용
  - Non-preemptible resources 조건 제거
  - 현실적으로 불가능
  - 프로세스가 할당받을 수 없는 자원을 요청한 경우, 기존에 갖고있던 자원 모두 반납 후 작업 취소
  - 단, 위 방법은 심각한 자원 낭비가 발생되기 때문에 비효율적
<br>

3. 필요 자원 한번에 모두 할당
   - Hold and wait 조건 제거
   - 필요하지 않은 순간에도 가지고 자원을 갖고있어 자원 낭비 발생
   - 무한 대기 현상 발생 가능
<br>

4. Circular wait 조건 제거
   - 자원들에게 순서를 부여하여 프로세스 순서의 증가 방향으로 자원 요청시킴
   - 자원 낭비 발생하여 비현실적
<br>

- 위의 4가지 방법들은 모두 Deadlock을 발생시키지 않도록 해주나, 심각한 자원 낭비가 발생하며 비현실적이다.

## 교착상태 회피
- 시스템의 상태를 계속 감시
- 시스템이 deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지
<br>

- 가정
  - 프로세스의 수가 고정됨
  - 자원의 종류와 수가 고정됨
  - 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
  - 프로세스는 자원을 사용 후 반드시 반납함
<br>

- 중요 용어
1. Safe state
   - 모든 프로세스가 정상적 종료가 가능한 상태
   - Safe sequence가 존재하여 Deadlock 상태가 되지 않음을 보장함
2. Unsafe state
   - Deadlock 상태가 될 가능성이 있음
   - 반드시 발생한다는 의미는 아님
<br>

- Dijkstra's banker's algorithm
  - Deadlock avoidance algorithm
  - 한 종류의 자원이 여러개있다는 가정에 시스템을 항상 safe state로 유지한다.
  
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/ec56b495-7031-405a-8967-57470b9f9ab4)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/fc76b8c2-5d3a-41ba-912d-191830054948)

- 장점
  - Daedlock 발생을 막을 수 있다.
- 단점
  - High overhead : 항상 시스템을 감시하고 있어야 한다.
  - Low resource utilization : Safe state 유지를 위해 사용되지 않는 자원이 존재
  - 프로세스 수, 자원 수가 고정적이며 필요한 최대 자원 수를 알고 있어야 하는등 현실적이지 않음
<br>

## 교착상태 탐지(Deadlock Detection)
- 주기적으로 시스템이 deadlock 상태인지 확인함
- 주로 Resource Allocation Graph (RAG) 기법을 사용하여 확인함
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/a6137da9-d94e-4c75-b3b8-2c29effb55c8)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/fb98932c-3141-47cf-9978-8fa76482aaf1)

- RAG 동작과정
  - 1) Unblocked process에 연결 된 모든 edge를 제거한다.
  - 2) 더 이상 Unblocked process가 없을 때까지 1번을 반복한다.

- 위 결과 최종 그래프에서 모든 edge가 제거되면 deadlock이 없는 상태이고, edge가 남으면 deadlock이 존재하는 상태이다.
<br>

- 중요 용어
1. Graph reduction
   - 주어진 RAG에서 edge를 하나씩 지워가는 방법
2. Completely reduced
   - 모든 edge가 제거됨
   - Deadlock에 빠진 프로세스가 없음
3. Irreducible
   - 지울 수 없는 edge가 존재
   - 하나 이상의 프로세스가 deadlock 상태
4. Unblocked process
   - 필요한 자원을 모두 할당 받을 수 있는 상태 ( 요청 < 남은 수 )
