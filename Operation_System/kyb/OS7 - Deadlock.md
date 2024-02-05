# DeadLock(교착 상태)

- 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
- 시스템 내에 deadlock에 빠진 프로세스가 있는 경우

**Starvation의 차이**

Starvation: Ready 상태에 존재, 우선순위는 밀려있지만 실행 될 가능성 O

Deadlock: Asleep 상태에 존재, 실행 될 가능성 X

- 일반적 관점의 자원 분류
    - HW 자원
    - SW 자원
- Deadlock 관점의 자원 분류
    - 선점 가능 여부
    - 할당 단위
    - 동시 사용 가능 여부
    - 재사용 가능 여부

## 선점 가능 여부에 따른 분류

- Preemptible resources
    - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
    - Processor, memory 등
- Non-preemptible resources
    - 선점 당하면, 이후 진행에 문제가 발생하는 자원
        - Rollback, restart 등 특별한 동작 필요

## 할당 단위에 따른 분류

- Total allocation resources
    - 자원 전체를 프로세스에게 할당(ex. Process)
- Partitioned allocation resources
    - 하나의 자원을 여러 조각으로 나누어, 여러 프로세스들에게 할당(ex. memory)

## 동시 사용 가능 여부에 따른 분류

- Exclusive allocation resources
    - 한 순간에 한 프로세스만 사용 가능한 자원
        - Processor, memory, disk drive 등
- Shared allocation resource
    - 여러 프로세스가 동시에 사용 가능한 자원
        - Program(sw), shared data 등

## 재사용 가능 여부에 따른 분류

- SR(Serially-reuserable Resources)
    - 시스템 내에 항상 존재하는 자원
    - 사용이 끝나면, 다른 프로세스가 사용 가능
    - Processor, memory, disk drive, program 등
- CR(Consumable Resources)
    - 한 프로세스가 사용한 후에 사라지는 자원
        - Signal, message 등

## Deadlock을 발생시킬 수 있는 자원의 형태

- Non-preemptible resources
- Exclusive allocation resources
- SR(Serially-reuserable Resources)
- 할당 단위는 영향을 미치지 않음

## Deadlock Model(표현법)

### Graph Model

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/a407563f-004b-4ed4-b64b-f3147278b0b2)


- Node
    - 프로세스 노드(P1, P2), 자원 노드(R1, R2)
- Edge
    - Rj → Pi: 자원 Rj이 프로세스 Pi에 할당 됨
    - Pi → Rj: 프로세스 Pi가 자원 Rj을 요청(대기 중)
    

### State Transition Model

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/f724c51e-32c1-4a70-95fd-9745ee7b432e)


- 예제
    - 2개의 프로세스와 A type의 자원 2개 존재
    - 프로세스는 한번에 하나의 자원만 요청/반납 가능

**Process가 2개일 때 Model**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/3c769ce4-fe08-4691-969f-d7e4f51f5b97)


## Deadlock 발생 필요 조건

**자원의 특성**

- Exclusive allocation resources
- Non-preemptible resources

**프로세스의 특성**

- Hold and wait(Partial allocation)
    - 자원을 하나 hold하고 다른 자원 요청
- Circular wait

## Deadlock 예방

Deadlock을 **절대** 발생하지 않도록 예방하는 기법

**4개의 deadlock 발생 필요 조건 중 하나를 제거**

- 모든 자원을 공유 허용
    - Exclusive allocation resources 조건 제거
    - 현실적으로 불가능
- 모든 자원에 대해 선점 허용
    - Non-preemptible resources 조건 제거
    - 현실적으로 불가능
    - 유사한 방법
- 필요 자원 한번에 모두 할당(Total allocation)
    - Hold and wait 조건 제거
    - 자원 낭비 발생: 필요하지 않은 순간에도 자원을 가지고 있음
    - 무한 대기 현상 발생 가능
- Circular wait 조건 제거
    - Totally allocation을 일반화 한 방법
    - 자원들에게 순서를 부여
    - 프로세스는 순서의 증가 방향으로만 자원 요청 가능
    - 자원 낭비 발생

**Deadlock 예방 기법은, 심각한 자원 낭비를 유발하고, 비현실적인 기법임.**

## Deadlock 회피

- 시스템의 상태를 계속 감시
- 시스템이 deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지

- Safe state
    - 모든 프로세스가 정상적으로 종료 가능한 상태
    - Safe sequence가 존재: Deadlock 상태가 되지 않을 수 있음을 보장
- Unsafe state
    - Deadlock 상태가 될 가능성이 있는 상태
    - 반드시 발생한다는 의미는 아님
    

**Deadlock 회피의 상황 가정**

- 프로세스의 수가 고정됨
- 자원의 종류와 수가 고정됨
- 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
- 프로세스는 자원을 사용 후 반드시 반납

 → **비현실적**

## Deadlock 회피 알고리즘

### Dijkstra’s banker’s algorithm

- 은행원 알고리즘
- Deadlock 회피를 위한 간단한 이론적 기법
- 가정: 한 종류의 자원이 여러개
- 시스템을 항상 safe state로 유지

**예제 1**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/5aa617f2-4948-4b0d-a1b8-b437d10796a5)


- 이용 가능 자원: 2
    - P1에 자원 2 할당 → P1이 작업을 마치고 3의 자원 반환
    - P3에 자원 3 할당 → P3이 작업을 마치고 5의 자원 반환
    - P2에 자원 5 할당 → P2가 작업을 마치고 9의 자원 반환

**예제 2**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/e106248e-a252-4f5a-97b9-2008c97e3e58)


위 상황은 Unsafe status로, 당장 교착 상태가 발생한 것은 아니지만, 이후 요청에 따라 교착상태에 빠질 수 있음

Banker’s algorithm은 한 프로세스가 자원을 요청할 때, 이를 수락한 경우를 가정해보고, 이 때 만약 Unsafe status가 될 수 있는 경우, 자원 요청을 거절함.

### Habermann’s algorithm

- Dijkstra’s algorithm의 확장
- 여러 종류의 자원 고려
- 시스템을 항상 safe state로 유지

**결론적으로, 회피 알고리즘은 Deadlock의 발생을 막을 수 있음**

그러나, 항상 시스템을 감시해야 하기에 **오버헤드가 높고**,

safe status 유지를 위해 **자원 사용률 또한 낮음**.

또한 회피를 위해 고려해야 하는 가정들이 **비 현실적임**.

## Deadlock 탐지

- Deadlock 방지를 위한 사전 작업을 하지 않음
    - Deadlock이 발생 가능
- 주기적으로 deadlock 발생 확인
- Resource Allocation Graph(RAG) 사용

### RAG

- Directed, bipartite Graph
- 그래프 G = (N, E) : Node, Edge로 구성

**Node**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/7d675c25-c94c-4153-8477-331c12c1d5d2)


**Edge**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/6e323d95-7c3c-4860-8b0f-1c8654fec1f5)


![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/8a6fb634-4c22-4e0c-83c0-60e69f281b39)


- Rk : k type의 자원
- tk: Rk의 단위 자원 수

### Deadlock 탐지 메서드

- Graph reduction
    - 주어진 RAG에서 edge를 하나씩 지워가는 방법
    - Completely reduced
        - 모든 edge가 제거 됨
        - Deadlock에 빠진 프로세스가 없음
    - Irreducible
        - 지울 수 없는 edge가 존재
        - 하나 이상의 프로세스가 deadlock 상태
    - Unblocked process
        - 필요한 자원을 모두 할당 받을 수 있는 프로세스
    - Procedure
        1. Unblocked process에 연결 된 모든 edge를 제거
        2. 더 이상 Unblocked process가 없을 때  까지 1 반복
        - 최종 graph
            - 모든 edge가 제거 됨
            - 일부 edge가 남음
    - High Overhead
        - 검사 주기에 영향을 받음
        - Node의 수가 많은 경우
    

## Deadlock 회피 vs 탐지

- **회피**
    - 최악의 경우를 생각 → 앞으로 일어날 일을 고려
    - Deadlock이 발생하지 않음
- **탐지**
    - 최선의 경우를 생각 → 현재 상태만 고려
    - Deadlock 발생 시 Recovery 과정 필요
    - Deadlock 발생 시 Recovery 과정 필요

## Deadlock 회복

- Process termination
    - Deadlock 상태인 프로세스 중 일부 종료
    - Termination cost model
        - 종료 시킬 deadlock 상태의 프로세스 선택
        - Termination cost
            - 우선 순위
            - 종류
            - 총 수행 시간
            - 남은 수행 시간
            - 종료 비용
    - 종료 프로세스 선택
        - Lowest-termination cost process first
            - 간단함
            - 낮은 오버헤드
            - 불필요한 프로세스들이 종료 될 가능성이 높음
        - Minimum cost recovery
            - 최소 비용으로 deadlock 상태를 해소할 수 있는 process 선택
            - Complex
            - 높은 오버헤드
            
- Resource preemption
    - Deadlock 상태 해결을 위해 선점할 자원 선택
    - 해당 자원을 가지고 있는 프로세스를 종료 시킴
        - Deadlock 상태가 아닌 프로세스가 종료 될 수도 있음
        - 종료된 프로세스는 이후 재시작 됨
    - 선점할 자원 선택
        - Preemption cost model이 필요
        - minimum cost recovery method 사용
- Checkpoint-restart method
    - 프로세스의 수행 중 특정 지점(checkpoint)마다 context를 저장
    - Rollback을 위해 사용
        - 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작.

**결론적으로, 회복 기법은 Deadlock을 검출 한 후 이를 해결하는 것임.**

**Deadlock 회복 메서드**

- Process termination
    - deadlock 상태에 있는 프로세스를 종료 시킴
    - 강제 종료 된 프로세스는 이후 재시작 됨
- Resource preemption
    - Deadlock 상태 해결 위해 선점할 자원 선택
    - 선정된 자원을 가지고 있는 프로세스에서 자원을 빼앗음
        - 자원을 빼앗긴 프로세스는 강제 종료 됨
