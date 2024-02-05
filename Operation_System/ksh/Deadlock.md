# 교착 상태 (Deadlock)
- Blocked/Asleep state
  - 프로세스가 특정 이벤트를 기다리는 상태
  - 프로세스가 필요한 자원을 기다리는 상태
 
- Deadlock state
  - 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
    - 프로세스가 deadlock 상태에 있음
  - 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
    - 시스템이 deadlock 상태에 있음

- Deadlock: asleep, blocked 상태에서 이벤트나 자원을 기다리는 것 (발생 가능성 없음)
- Starvation: ready 상태에서 CPU를 할당받기를 기다리는 것 (발생 가능성 있음)

## 자원의 분류
- 일반적 분류
  - 하드웨어 자원 / 소프트웨어 자원
- 다른 분류법
  - 선점 가능 여부에 따른 분류
  - 할당 단위에 따른 분류
  - 동시 사용 가능 여부에 따른 분류
  - 재사용 가능 여부에 따른 분류

### 선점 가능 여부에 따른 분류
- Preemptible resources
  - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
  - 프로세서, 메모리 등
- Non-preemptible resources
  - 선점 당한 후, 진행에 문제가 발생하는 자원
    - 롤백, 재시작등 특별한 동작이 필요
  - 디스크 드라이브 등

### 할당 단위에 따른 분류
- Total allocation resources
  - 자원 전체를 프로세스에게 할당
  - 프로세서, 디스크 드라이브 등
- Partitioned allocation resources
  - 하나의 자원을 여러 조각으로 나누어, 여러 프로세스들에게 할당
  - 메모리 등

### 동시 사용 가능 여부에 따른 분류
- Exclusive allocation resources
  - 한순간에 한 프로세스만 사용 가능한 자원
  - 프로세서, 메모리, 디스크 드라이브 등
- Shared allocation resource
  - 여러 프로세스가 동시에 사용 가능한 자원
  - 프로그램(SW), 공유 데이터 등
 
### 재사용 가능 여부에 따른 분류
- SR(Serially-reusable Resources)
  - 시스템 내에 항상 존재하는 자원
  - 사용이 끝나면 다른 프로세스가 사용 가능
  - 프로세서, 메모리, 디스크 드라이브, 프로그램 등
- CR(Consumable Resources)
  - 한 프로세스가 사용한 후에 사라지는 자원
  - 시그널, 메시지 등

- Deadlock을 발생시킬 수 있는 자원의 형태
  - Non-preemptible resources
    - 다른 프로세스가 자원을 사용중일 때 뺏을 수 없음
  - Exclusibe allocation resources
    - 자원을 여러 프로세스가 동시에 사용할 수 없는 경우
  - 할당 단위는 영향을 미치지 않음
  - Serially reusable resources
  - CR을 대상으로 하는 Deadlock model은 고려하기에 너무 복잡함

## Deadlock 발생의 예
- 2개의 프로세스 (P1, P2)
- 2개의 자원 (R1, R2)

![9일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/53566939-d407-4f07-8e6a-82101ad7158f)

- 위 상황은 서로 가진 것을 반납하지 않고 요구하고 있는 상황 -> Deadlock 발생

## Deadlock Model (표현법)
### Graph Model
- Node
  - 프로세스 노드(P1, P2), 자원 노드(R1, R2)
- Edge
  - Rj -> Pi: 자원 Rj가 프로세스 Pi에 할당됨
  - Pi -> Rj: 프로세스 Pi가 자원 Rj를 요청(대기 중)
 
![9일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/2884dd56-9f1c-4941-96fa-54b5ddd8d64c)

### State Transition Model
- 2개의 프로세스와 A type의 자원 2개(unit) 존재
- 프로세스는 한번에 자원 하나만 요청/반납 가능
- 
![9일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/c0be0aff-08ea-459c-a537-f786bc3d4ec4)

- S33의 경우 P1, P2가 자원을 서로 반납하지 않고 자원을 요구하게 되므로 Deadlock 발생

### Deadlock 발생 필요 조건
- Exclusive use of resources: 자원의 특성
- Non-preemptible resources: 자원의 특성
- Hold and wait(Partial allocation): 프로세스의 특성
  - 자원을 하나 hold하고 다른 자원 요청
- Circular wait: 프로세스의 특성
- 위의 4가지 조건을 모두 만족해야 Deadlock이 발생함!

## Deadlock 해결 방법
### Deadlock Prevention (교착 상태 예방)
> Deadlock 발생 필요 조건 4가지 중 하나를 제거

- 모든 자원을 공유 허용
  - Exclusvie use of resources 조건 제거
  - 현실적으로 불가능
- 모든 자원에 대해 선점 허용
  - Non-preemptible resources 조건 제거
  - 현실적으로 불가능
  - 유사한 방법
    - 프로세스가 할당받을 수 없는 자원을 요청한 경우, 기존에 가지고 있던 자원을 모두 반납하고 작업 취소, 이후 처음 또는 check-point부터 다시 시작
    - 심각한 자원 낭비 발생 -> 비현실적
- 필요 자원 한번에 모두 할당 (Toal allocation)
  - Hold and wait 조건 제거
  - 자원 낭비 발생
    - 필요하지 않은 순간에도 가지고 있음
  - 무한 대기 현상 발생 가능
- Circular wait 조건 제거
  - Totally allocation을 일반화한 방법
  - 자원들에게 순서를 부여
  - 프로세스는 순서의 증가 방향으로만 자원 요청 가능
  - 자원 낭비 발생

- 4가지 예방 방법 모두 심각한 자원 낭비가 발생하고 비현실적임.

### Deadlock Avoidance (교착 상태 회피)
- 시스템의 상태를 계속 감시
- 시스템이 교착 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 **safe state**로 유지

- Safe state
  - 모든 프로세스가 정상적인 종료가 가능한 상태
  - Safe sequence가 존재: Deadlock 상태가 되지 않을 수 있음을 보장
- Unsafe state
  - Deadlock 상태가 될 가능성이 있음
  - 반드시 발생한다는 의미는 아님
 
- 가정
  - 프로세스의 수가 고정됨
  - 자원의 종류와 수가 고정됨
  - 프로세스가 요구하는 자원 및 최대 수량을 알고 있음
  - 프로세스는 자원을 사용 후 반드시 반납한다
  -> 비현실적임

- 다익스트라의 banker's algorithm
  - Deadlock avoidance를 위한 간단한 이론적 기법
  - 가정: 한 종류의 자원이 여러개
  - 시스템을 항상 safe state로 유지

![10일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/eb85c1c5-1d90-463e-b01b-2168984fc334)

- Max. Claim: 최대 필요수, Cur. Alloc.: 현재 할당 수, Additional Need: 요구 할당 수(반드시 필요x)
- 돈을 빌려주고 무조건 갚는다고 할 때, 누구한테 먼저 돈을 빌려줄까?
- Available resource units: 2
- 실행 종료 순서: P1 -> P3 -> P2 (Safe sequence)
- 현재 상태에서 안전 순서가 하나 이상 존재하면 Safe state임

![10일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/cfff6a3f-0ea1-4014-b201-43cafa377223)

- 위의 경우 safe sequence가 없으므로 Unsafe state
- 결과적으로 자원을 줬다고 가정해보고 safe sequnece가 없으면 요청을 거절함

- Habermann's algorithm
  - 다익스트라 알고리즘의 확장
  - 여러 종류의 자원 고려
  - 시스템을 항상 Safe state로 유지

> 결론
- Deadlock의 발생을 막을 수 있음
- High overhead
  - 항상 시스템을 감시하고 있어야 한다
- Low resource utilization
  - Safe state 유지를 위해, 사용되지않는 자원이 존재
- Not practical

### Deadlock detection and recovery methods (교착상태 탐지 및 복구)
- Deadlock 방지를 위한 사전 작업을 하지 않음
  - Deadlock이 발생 가능
- 주기적으로 deadlock 발생 확인
  - 시스템이 deadlock 상태인가?
  - 어떤 프로세스가 deadlock 상태인가?
- Resource Allocation Graph (RAG) 사용

- Resource Allocation Graph (RAG)
  - Deadlock 검출을 위해 사용
  - Directed, bipartite Graph(두개의 파트로 나누는 그래프)
 
- G = (N, E) : 그래프는 노드와 엣지로 이루어져있음
- 노드는 프로세스들과 리소스들로 나누어져있음
- 프로세서 -> 리소스(자원 요청) 또는 리소스 -> 프로세스(자원 할당) 방향의 엣지만 가능

- Graph reduction
  - 주어진 RAG에서 edge를 하나씩 지워가는 방법 -> Deadlock인지 아닌지 판단 가능
  - Completely reduced
    - 모든 edge가 제거됨
    - 교착상태에 빠진 프로세스가 없음
  - Irreducible
    - 지울수 없는 edge가 존재
    - 하나 이상의 프로세스가 교착상태
   
- Unblocked process에 연결된 edge를 지우기
  - 필요한 자원을 모두 할당받을 수 있는 프로세스

- Graph reduction procedure
1. Unblocked process에 연결된 모든 edge를 제거
2. 더이상 unblocked process가 없을 때까지 1 반복
- 최종 그래프에서
  - 모든 edge가 제거됨: 현재 상태에서 deadlock이 없음
- 일부 edge가 남음: 현재 상태에서 deadlock이 존재함

- High overhead
  - 검사 주기에 영향을 받음
  - Node의 수가 많은 경우

### 교착 상태 회피 vs 탐지
- 교착 상태 회피
  - 최악의 경우를 생각
  - Deadlock이 발생하지 않음
- 교착 상태 탐지
  - 최선의 경우를 생각
  - Deadlock 발생 시 Recovery 과정이 필요

### Deadlock Recovery
- Process termination
  - Deadlock 상태인 프로세스중 일부 종료
  - 어떤 프로세스를 종료시킬지 정하는 것은 cost model을 통해 정함
  - cost model은 우선순위, 종류, 총 수행시간, 남은 수행시간, 종료 비용등을 고려함
 
- 종료 비용이 가장 적은 프로세스 우선하는 경우
  - 단순하고 overhead가 적음
  - 불필요한 프로세스들이 종료될 가능성이 높음
- 비용을 최적화하는 최선의 선택을 하는 경우
  - 최소 비용으로 deadlock 상태를 해소할 수 있는 프로세스 선택
    - 모든 경우의 수를 고려해야 함
  - 복잡하고 overhead가 큼
  
- 자원을 선점하는 경우 (Resource preemption)
  - Deadlock 상태 해결을 위해 선점할 자원 선택
  - 해당 자원을 가지고 있는 프로세스를 종료시킴
    - Deadlock 상태가 아닌 프로세스가 종료될 수도 있음
    - 해당 프로세스는 이후 재시작됨

- Checkpoint-restart method
  - 프로세스의 수행 중 특정 지점마다 context를 저장
  - Rollback을 위해 사용
    - 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작
