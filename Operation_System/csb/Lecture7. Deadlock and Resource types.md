# Deadlock

### Deadlock의 개념

- Blocked/Asleep state
    - 프로세스가 특정 이벤트를 기다리는 상태
    - 프로세스가 필요한 자원을 기다리는 상태
- Deadlock state
    - 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
        - 프로세스가 deadlock 상태에 있음
    - 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
        - 시스템이 deadlock상태에 있음

### Deadlock vs Starvation

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/468f35e3-243d-486e-a4d8-a811a797d1bf)


### 자원의 분류

- 일반적 분류
    - Hardware resources vs Software resourcecs
- 다른 분류법
    - 선점 가능 여부에 따른 분류
    - 할당 단위에 따른 분류
    - 동시 사용 가능 여부에 따른 분류
    - 재사용 가능 여부에 따른 분류

### 선점 가능 여부에 따른 분류

- Preemptible resources
    - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
    - Processor, memory 등
- Non-preemptible resources
    - 선점 당하면 이후 진행에 문제가 발생하는 자원
        - Rollback, restart 등 특별한 동작이 필요
    - disk drive 등

### 할당 단위에 따른 분류

- Total allocation resources
    - 자원 전체를 프로세스에게 할당
    - Processor, disk drive 등
- Partitioned allocation resources
    - 하나의 자원을 여러 조각으로 나누어, 여러 프로세스들에게 할당
    - memory 등

### 동시 사용 가능 여부에 따른 분류

- Exclusive allocation resources
    - 한 순간에 한 프로세스만 사용 가능한 자원
    - Processor, memory, disk drive 등
- Shared allocation resource
    - 여러 프로세스가 동시에 사용 가능한 자원
    - Program, shared data 등
 
### 재사용 가능 여부에 따른 분류

- SR(Serially-reusable resources)
    - 시스템 내이 항상 존재하는 자원
    - 사용이 끝나면, 다른 프로세스가 사용 가능
    - processor, memory, disk drive, program 등
- CR(Consumable resources)
    - 한 프로세스가 사용한 후에 사라지는 자원
    - signal, message 등
    
### Deadlock과 자원의 종류

- Deadlock을 발생시킬 수 있는 자원의 형태
    - Non-premmptible resources
    - Exclusive allocation resources
    - Serially reusable resources
    - 할당 단위는 영향을 미치지 않음
- CR을 대상으로 하는 Deadlock model → 너무 복잡

### Deadlock 발생의 예

- 2개의 프로세스 (P1, P2)
- 2개의 자원(R1, R2)

### Deadlock Model (표현법)

- Graph Model
- State Transition Model

### Graph Model

- Node
    - 프로세스 노드(P1, P2), 자원 노드(R1, R2)
- Edge
    - Rj → Pi: 자원 Rj이 프로세스 Pi에 할당됨
    - Pi → Rj: 프로세스 Pi가 자원 Rj을 요청(대기 중)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/859cb641-05a8-4878-8b22-208ad2e6030d)

### State Transition Model

- 예제
    - 2개의 프로세스와 A type의 자원 2개(unit) 존재
    - 프로세스는 한번에 자원 하나만 요청/반납 가능
- State
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/b82ae9aa-d533-4644-95c8-5bbf5ccc9917)

### Deadlock 발생 필요 조건

- 자원의 특성
    - Exclusive use of resources
    - Non-preemptible resources
- 프로세스의 특성
    - Hold and wait(Partial allocation)
        - 자원을 하나 hold 하고 다른 자원 요청
    - Circular wait
 
### Deadlock 해결 방법

- Deadlock prevention methods
    - 교착상태 예방
- Deadlock avoidance method
    - 교착상태 회피
- Deadlock detection and deadlock recovery methods
    - 교착상태 탐지 및 복구

### Deadlock Prevention

- 4개의 Deadlock 발생 필요 조건 중 하나를 제거 → Deadlock이 절대 발생하지 않음
    - Exclusive use of resources
    - Non-preemptible resources
    - Hold and wait(Partial allocation)
    - Circular wait

### 모든 자원을 공유 허용

- Exclusive use of resources 조건 제거
- 현실적으로 불가능

### 모든 자원에 대해 선점을 허용

- Non-preemptible resources 조건 제거
- 현실적으로 불가능
- 유사한 방법
    - 프로세스가 할당 받을 수 없는 자원을 요청한 경우, 기존에 가지고 있던 자원을 모두 반납하고 작업 취소 → 이후 처음(또는 check point)부터 다시 시작
    - 심각한 자원 낭비 발생 → 비현실적

### 필요한 자원을 한번에 모두 할당 (Total allocation)

- Hold and wait(Partial allocation) 조건 제거
- 필요하지 않은 순간에도 자원을 가짐 → 자원 낭비 발생
- 무한 대기 현상 발생 가능

### 자원들에게 순서를 부여

- Circular wait 조건 제거
- 프로세스는 순서의 증가 방향으로만 자원 요청 가능
- 자원 낭비 발생

### Deadlock Prevention

- 4개의 Deadlock 발생 필요 조건 중 하나를 제거
- Deadlock이 절대 발생하지 않음
- 심각한 자원 낭비 발생
    - Low device utilization
    - Reduced system throughput
- 비현실적