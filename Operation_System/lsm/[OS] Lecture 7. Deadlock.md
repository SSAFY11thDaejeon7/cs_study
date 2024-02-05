# 1. **Deadlock and Resource types**

### Deadlock의 개념

- Blocked/ Asleep state
    - 프로세스가 특정 이벤트를 기다리는 상태
    - 프로세스가 필요한 자원을 기다리는 상태
- Deadlock state
    - 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우 
    → 프로세스가 deadlock 상태에 있음
    - 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
- starvation과의 차이점
    - 기다리고 있는 자원, 어느 상태에 있는지 차이
<img width="845" alt="deadlock과starvation" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/59f08ca1-ca38-4edf-aff2-6ca301d40446">


### 자원의 분류

- 일반적 분류
    - hardware resources vs software resources
- 선점 가능 여부에 따른 분류
    - Preemptible resources
        - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
        - (processor, memory 등)
    - Non-Preemptible resources
        - 선점 당하면, 이후 진행에 문제가 발생하는 자원
        - 
        - (disk drive 등)
- 할당 단위에 따른 분류
    - Total allocation resources
        - 자원 전체를 프로세스에게 할당
        - processor disk drive 등
    - Partitioned allocation resources
        - 하나의 자원을 여러 조각으로 나누어, 여러 프로세스들에게 할당한다.
        - Memory 등
- 동시 사용 가능 여부에 따른 분류
    - Exclusive allocation resources
        - 한 순간에 한 프로세스만 사용 가능한 자원
        - Processor, memory, disk drive 등
    - Shared allocation resource
        - 여러 프로세스가 동시에 사용 가능한 자원
        - Program, shared data 등
- 재사용 가능 여부에 따른 분류
    - SR (Serially-reusable Resources)
        - 시스템 내에 항상 존재하는 자원
        - 사용이 끝나면, 다른 프로세스가 사용 가능
    - CR (Consumable Resources)
        - 한 프로세스가 사용한 후에 사라지는 자원

### Deadlock과 자원의 종류

- Deadlock을 발생시킬 수 있는 자원의 형태
    - Non-preemptible resources
    - Exclusive allocation resources
    - Serially reusable resources
    - 할당 단위는 영향을 미치지 않음
- CR을 대상으로 하는 Deadlock model은 너무 복잡하다.

### Deadlock 발생의 예

- 2개의 프로세스와 2개의 자원이 있을때
<img width="390" alt="데드락발생예" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/4ea94dc2-6aea-4d52-aaa8-e1a8f00e9433">


- Deadlock model (표현법)
    - Graph Model: 싸이클 형성 시 데드락
        - Node: 프로세스 노드, 자원 노드
        - Edge: 어떠한 자원이 프로세스에 할당되는 것
        <img width="302" alt="데드락싸이클" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/5791fa06-6cc6-44f4-a2f0-45fc87cdb46e">

        
    - State Transition Model
        - 2개의 프로세스와 A type의 자원 2개 존재
        - 프로세스는 한번에 자원 하나만 요청/반납 가능
        
        <img width="387" alt="statetransitionmodel1" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/5f660c77-1e92-4a30-995e-651d27bc493f">

        <img width="858" alt="statetransitionmodel2" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/76c14e46-c62d-4b37-9e44-4c65cb2d2f53">

        ### Deadlock 발생 필요 조건
        
        1. Exclusive use of resources: 하나의 프로세스만 사용하는 자원
        2. Non-Preemptive resources: 뺏을 수 없는 자원
        3. Hold and wait (자원을 하나 hold하고 다른 자원을 요청)
        4. Circular wait (순환대기)
    
    > Deadlock의 필요 조건을 하나씩 없애서 데드락이 발생하지 않도록 하는 것
    > 

# 2. Deadlock Prevention

### Deadlock Prevention

- 모든 자원을 공유 허용 (Exclusive use of resources)
- 모든 자원에 대해 선점 허용 (Non-Preemptive resources)
- 필요 자원 한번에 모두 할당
- Circular wait 조건 제거

4개의 deadlock 발생 필요 조건 중 하나를 제거한다.

Deadlock이 절대 발생하지 않지만, 심각한 자원을 낭비한다.

비현실적이다.

# 3. Deadlock Avoidance

### Deadlock Avoidance

- 시스템의 상태를 계속 감시한다.
- 시스템이 deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지한다.

- **Safe State**:
    - 모든 프로세스가 정상적으로 종료 가능한 상태
    - safe sequence가 존재 → Deadlock 상태가 되지 않을 수 있음을 보장
- **Unsafe State**
    - Deadlock 상태가 될 가능성이 있음
    - 반드시 발생한다는 의미는 아님
- 가정
    - 프로세스 수 고정
    - 자원의 종류와 수 고정
    - 프로세스가 요구하는 자원의 최대 수량을 알고 있음
    - 프로세스는 자원을 사용 후 반드시 반납한다.

→ Not practical

- 다익스트라의 banker’s algorithm
    - Deadlock avoidance를 위한 간단한 이론적 기법
    - 가정
        - 한 종류의 자원이 여러 개
    - 시스템을 항상 safe state로 유지한다.
<img width="594" alt="safestate" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/ae909192-c78b-4711-85c0-f1b8e1104e10">


- Habermann’s algorithm
    - 뱅커’s 알고리즘 확장 (자원의 개수만 많아짐)

> Deadlock의 발생을 막을 수 있지만
High Overhead와 Low resource utilization의 문제로 실용적이지 않다.
> 

# 4. Deadlock detection and deadlock recovery methods

- Deadlock 방지를 위한 사전 작업을 하지 않음 → 발생이 가능하다.

### Resource Allocation Graph (RAG)
<img width="343" alt="resourceallocationgraph" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/b152e3ec-1837-44c7-aa12-cf11266fd8c2">


- Directed graph G = (N, E)
    - N = {Np, Nr} where

### Graph reduction

- 주어진 RAG에서 edge를 하나씩 지워가는 방법
- 모두 지워질 경우, deadlock이 아니다!
- Completely reduced
    - 모든 edge가 제거 됨
    - Deadlock에 빠진 프로세스가 없음
- Irreducible
    - 지울 수 없는 edge가 존재
    - 하나 이상의 프로세스가 deadlock 상태
- Unblocked Process: 필요한 자원을 모두 할당 받을 수 있는 프로세스
- Graph reduction procedure
    1. Unblocked process에 연결된 모든 edge를 제거
    2. 더 이상 unblocked process가 없을 때까지 1 반복
    
    최종 Graph에서 
    
    모든 edge가 제거 됨: 현재 상태에서 deadlock이 없음
    
    일부 edge가 남음: 현재 deadlock이 존재
    

> High overhead (검사 주기에 영향을 받고 Node의 수가 많음)
Low overhead deadlock detection methods 
(Special Case)
> 

### Deadlock Avoidance vs Detection

- Deadlock Avoidance
    - 최악의 경우를 생각: 앞으로 일어날 일을 고려한다.
    - Deadlock이 발생하지 않는다.
- Deadlock detection
    - 최선의 경우를 생각: 현재 상태만 고려한다.
    - Deadlock 발생 시 Recovery 과정이 필요하다.

### Deadlock Recovery

- Process termination: Deadlock 상태인 프로세스 중 일부를 종료한다.
- Termination cost model: 종료 시킬 deadlock 상태의 프로세스를 선택한다.
