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

### Deadlock Avoidance

- 시스템의 상태를 계속 감시
- 시스템이 Deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지
- Safe state
    - 모든 프로세스가 정상적 종료 가능한 상태
    - Safe sequence가 존재
        - Deadlock상태가 되지 않을 수 있음을 보장
- Unsafe state
    - Deadlock 상태가 될 가능성이 있음
    - 반드시 발생한다는 의미는 아님
- 가정
    - 프로세스의 수가 고정됨
    - 자원의 종류와 수가 고정됨
    - 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
    - 프로세스는 자원을 사용 후 반드시 반납

### Dijkstra’s banker’s algorithm

- Deadlock avoidance를 위한 간단한 이론적 기법
- 가정
    - 한 종류의 자원(R)이 여러 개(units)
- 시스템을 항상 safe state로 유지

<img width="885" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/dc6752f9-ee63-4790-89d8-0360222565db">

- Unsafe state example
<img width="778" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/86dfb6ca-76fa-4880-ba0e-c3bb2bf0763c">

### Habermann’s algorithm

- Dijkstar’s algorithm의 확장
- 여러 종류의 자원 고려
    - Multiple resource types
    - Multiple resource units for each resource type
- 시스템을 항상 safe state로 유지
- Example
<img width="799" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/6fa2725e-72f7-4a77-8064-de0100e18ef8">

### Deadlock Avoidance 정리

- Deadlock의 발생을 막을 수 있음
- High overhead
    - 항상 시스템을 감시하고 있어야 함
- Low Resource Utilization
    - Safe state 유지를 위해, 사용 되지 않는 자원이 존재
- Not practical
    - 가정
        - 프로세스 수, 자원 수가 고정
        - 필요한 최대 자원 수를 알고 있음

### Deadlock Detection and Deadlock Recovery

- Deadlock 방지를 위한 사전 작업을 하지 않음
    - Deadlock이 발생 가능
- 주기적으로 Deadlock 발생 확인
    - 시스템이 Deadlock 상태인지
    - 어떤 프로세스가 Deadlock 상태인지
- Resource Allocation Graph(RAG) 사용

### Resource Allocation Graph(RAG)

- Deadlock 검출을 위해 사용
- Directed, bipartite Graph
- Directed graph G = (N, E)
- Edge는 프로세스 집합과 자원 집합 사이에만 존재

### Graph Reduction

- 주어진 RAG에서 edge를 하나씩 지워가는 방법
- Completely reduced
    - 모든 edge가 제거 됨
    - Deadlock에 빠진 프로세스가 없음
- Irreducible
    - 지울 수 없는 edge가 존재
    - 하나 이상의 프로세스가 deadlock 상태
- Unblokced process
    - 필요한 자원을 모두 할당 받을 수 있는 프로세스

<img width="833" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/730dfa38-b8c2-40ce-a5d0-aeb0b567270e">

- example 1
<img width="811" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/c687b945-8e81-473c-9e00-642aac5b6e91">

- example2 (남은 edge가 있으므로 Deadlock 상태)
<img width="877" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/7ae293cf-9436-4961-8392-4f4eea35367c">

### Graph Reduction 정리

- High overhead
    - 검사 주기에 영향을 받음
    - Node의 수가 많은 경우

### Deadlock Avoidance vs Detection

- Deadlock avoidance
    - 최악의 경우를 생각
    - Deadlock이 발생 하지 않음
- Deadlock detection
    - 최선의 경우를 생각
    - Deadlock 발생 시 Recovery 과정이 필요

### Deadlock Recovery - Process termination

- Deadlock 상태인 프로세스 중 일부 종료
- Termination cost model
    - 종료 시킬 Deadlock 상태의 프로세스 선택
    - Termination cost
        - 우선순위 / Process priority
        - 종류 / Process type
        - 총 수행 시간 / Accumulated excution time of the process
        - 남은 수행 시간 / Remaining time of the process
        - 종료 비용 / Accounting cost
        - Etc.
- Lowest-termination cost process first
    - 단순함
    - 낮은 오버헤드
    - 불필요한 프로세스들이 종료 될 가능성이 높음
- Minimum cost recovery
    - 최소 비용으로 Deadklock 상태를 해소 할 수 있는 process 선택
    - 모든 경우의 수를 고려해야 함으로 복잡하고 오버헤드가 큼
    

### Deadlock Recovery - Resource preemption

- Deadlock 상태 해결을 위해 선점할 자원 선택
- 해당 자원을 가지고 있는 프로세스를 종료 시킴
    - Deadlock 상태가 아닌 프로세스가 종료 될 수도 있음
    - 해당 프로세스는 이후 재시작 됨
- 선점할 자원 선택
    - Preemption cost model이 필요
    - Minimum cost recovery method 사용

### Deadlock Recovery

- Deadlock을 검출 한 후 해결하는 과정
- Deadlock recovery methods
    - Process termination
        - Deadlock 상태에 있는 프로세스를 종료 시킴
        - 강제 종료된 프로세스는 이후 재시작
    - Resource preemption
        - Deadlock 상태 해결 위해 선점할 자원 선택
        - 선정된 자원을 가지고 있는 프로세스에서 자원을 빼앗음
        - 자원을 빼앗긴 프로세스는 강제 종료됨
- Checkpoint-restart method
    - 프로세스의 수행 중 특정 지점마다 context를 저장
    - Rollback을 위해 사용
        - 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작
