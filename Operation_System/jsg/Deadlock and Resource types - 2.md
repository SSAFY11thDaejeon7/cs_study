# Deadlock avoidance
- 시스템의 상태를 계속 감시
- 시스템이 deadlock 상태가 될 가능성이 있는 자원 할당 요청 보류
- 시스템을 항상 safe state로 유지
##### Safe state
- 모든 프로세스가 정상적 종료 가능한 상태
- Safe sequence가 존재
  - Deadlock상태가 되지 않을 수 있음을 보장
##### Unsafe state
- Deadlock 상태가 될 가능성이 있음
- 반드시 발생한다는 의미는 아님

### 가정
- 프로세스의 수가 고정됨
- 자원의 종류와 수가 고정됨
- 프로세스가 요구하는 자원 및 최대 수량을 알고있음
- 프로세스는 자원을 사용 후 반드시 반납한다.

#### Not practical

## Dijkstra's banker's algorithm

- Deadlock avoidance를 위한 간단한 이론적 기법
- 가정
  - 한 종류(resource type)의 자원이 여러개(unit)
- 시스템을 항상 safe state로 유지
- safe state
<img width="702" alt="스크린샷 2024-02-05 오후 9 17 36" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/b49dbc6c-faf2-4e10-b7c3-77a0ebac9777">

- Unsafe state 
<img width="664" alt="스크린샷 2024-02-05 오후 9 23 24" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/07c8f846-1461-4ae4-b438-279bec55cd7e">

## Havermann's algorithm
- Dijkstra's algorithm의 확장
- 여러 종류의 자원 고려
- 시스템을 항상 safe state로 유지
<img width="627" alt="스크린샷 2024-02-05 오후 9 27 58" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/aab6076c-eca6-459d-b859-ada959cc5170">

### 장단점
- Deadlock의 발생을 막을 수 있음
- High overhead
  - 항상 시스템을 감시하고 있어야 한다.
- Low resource utilization
  - Safe state 유지를 위해, 사용 되지 않는 자원이 존재
- Not practical
  - 가정
    - 프로세스 수, 자원 수가 고정
    - 필요한 최대 자원 수를 알고있음.

# Deadlock detection
- Deadlock 방지를 위한 사전 작업을 하지 않음
  - Deadlock이 발생 가능
- 주기적으로 deadlock 발생 확인
  - 시스템이 deadlodck 상태인가?
  - 어떤 프로세스가 deadlock 상태인가
- Resource Allocation Graph (RAG) 사용)
  - Deadlock 검출을 위해 사용
  - Directed, bipartite Graph
<img width="390" alt="스크린샷 2024-02-05 오후 9 33 37" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/fef9d4bc-ff6e-4fd1-b157-72103100d56f">

- |(a,b)| : (a->b)로 가는 edge의 수

<img width="493" alt="스크린샷 2024-02-05 오후 9 36 39" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/f1d0074e-b634-4d3f-8141-f593358dfc79">

## Graph reduction
- 주어진 RAG에서 edge를 하나씩 지워가는 방법
- Completely reduced
  - 모든 edge가 제거 됨
  - Deadlock에 빠진 프로세스가 없음
- Irreducible
  - 지울 수 없는 edge가 존재
  - 하나 이상의 프로세스가 deadlock 상태
- Graph reduction procedure
  1. Unblocked process에 연결된 모든 edge를 제거
  2. 더이상 unblocked process가 없을 때까지 1반복
- High overhead
  - 검사 주기에 영향을 받음
  - Node의 수가 많은 경우 
- 최종 Graph에서
  - 모든 edge가 제거 됨(Completely reduced)
    - 현재 상태에서 deadlock이 없음
<img width="666" alt="스크린샷 2024-02-05 오후 9 46 20" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/7ad099aa-e0b6-4def-98eb-b90598dec7b5">

  - 일부 edge가 남음(irreducible)
    - 현재 deadlock이 존재
<img width="671" alt="스크린샷 2024-02-05 오후 9 48 26" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/c3dcdcb4-9319-43b6-bb00-27a525d6f908">

### Unblocked process
<img width="658" alt="스크린샷 2024-02-05 오후 9 39 23" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/e70bbec8-5370-4330-9465-78e7df62fbbc">

## Deadlock Avoidance VS Detection
- Deadlock avoidance
  - 최악의 경우를 생각
    - 앞으로 일어날 일을 고려
- Deadlock detection
  - 최선의 경우를 생각
    - 현재 상태만 고려
  - Deadlock 발생 시 Recovery 과정이 필요
# Deadlock Recovery
- Deadlock을 검출 한 후 해결 하는 과정
- Deadlock recovery methods
  - Process termination
    - Deadlock 상태에 있는 프로세스를 종료 시킴
    - 강제 종료 된 프로세스는 이후 재시작 됨
    - Lowest-termination cost process first
      - Simple
      - Low oberhead
      - 불필요한 프로세스들이 종료 될 가능성이 높음
    - Minimum cost recovery
      - 최소 비용으로 deadlock 상태를 해소 할 수 있는 process 선택
        - 모든 경우의 수를 고려해야 함
      - Complex
      - High overhead 
  - Resource preemption
    - Deadlock 상태 해결을 위해 선점할 자원 선택
    - 선점된 자원을 가지고 있는 프로세스에서 자원을 빼앗음
      - 지원을 배앗긴 프로세스는 강제 종료 됨
      - Deadlock 상태가 아닌 프로세스가 종료 될수도 있음
      - 해당 프로세스는 이후 재시작 됨

  - Chechpoint-restart method
  <img width="608" alt="스크린샷 2024-02-05 오후 9 58 14" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/974f6160-0fa6-4cd0-a1a5-3801cd07eb89">

    - 프로세스의 수행 중 특정 지점(checkpoint) 마다 context를 저장
    - Rollback을 위해 사용
      - 프로세스 강제 종료 후, 가장 최근의 checkpoint에서 재시작
       


 


