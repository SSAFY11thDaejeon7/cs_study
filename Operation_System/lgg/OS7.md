# 7강

# Deadlock

---

**개념**

Blocked,Asleep state : 프로세스가 특정 이벤트 or 자원을 기다리는 상태

**Deadlock : 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우**

**Deadlock vs Starvation**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/30349d22-b2a5-4ace-b9e3-517388ec67b0)

Deadlock : 프로세스가 asleep상태에서 발생 가능성이 없는 자원을 무한히 기다리는 경우

Starvation : 프로세스가 ready 상태에서 운이 없어서 다른 프로세스들에게 우선순위 밀려 cpu 할당을 못받는경우(발생 가능성 있음)

그렇다면

## 자원의 분류

- HW 자원 vs SW 자원
- 선점 가능 여부에 따른 분류
- 할당 단위에 따른 분류
- 동시 사용 가능 여부에 따른 분류
- 재사용 가능 여부에 따른 분류

**선점 가능 여부에 따른 분류**

1. Preemptible resource : 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원 (Processsor, memory)
2. Non-preemptible resource : 선점 당하면, 이후 진행에 문제가 발생하는 자원 (disk drive)

**할당 단위에 따른 분류**

1. Total allocation resource : 자원 전체를 프로세서에게 할당 (Processor, disk drive)
2. Partitioned allocation resource : 하나의 자원을 여러 조각으로 나누어 여러 프로세스들에게 할당(Memory)

**동시 사용 가능 여부에 따른 분류**

1. Exclusive allocation resource : 한 순간에 한 프로세스만 사용 가능한 자원 (Processor, memory, disk drive)
2. Shared allocation resource : 여러 프로세스가 동시에 사용 가능한 자원 ( Program(sw), shared data)

**재사용 가능 여부에 따른 분류**

1. SR(Serially-reusable resource) : 시스템 내에 항상 존재하는 자원. 사용이 끝나면, 다른 프로세스가 사용 가능. (Processor, memory, disk drive, program)
2. CR(Consumable resource) : 한 프로세스가 사용한 후에 사라지는 자원(signal, message)

## Deadlock을 발생시킬 수 있는 자원의 형태?

- Non-preemptible resource
- Exclusive allocation resource
- Serially reusable resource
- 할당 단위는 영향 XX

cf. CR로 인한 deadlock 발생 경우도 있지만, deadlock 모델 너무 복잡해지기 때문에 CR까지는 일단 생각하지말자

## Deadlock 발생 상황

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/81fdd22a-2802-4d7f-aefb-2d631aad66c8)

P1이 R2요청. P2가 R1요청. P1이 R1요청.

근데 여기서 P2가 R2를 요청하면 서로 자원을 하나씩 잡고 무한 대기에 빠진다. 두 프로세스 모두 진행 불가능.

## Deadlock 표현법

- Graph Model
- State Transition Model

## Deadlock 발생 필요 조건 4가지

4가지를 모두 만족해야 Deadlock이다.

**자원의 특성**

1. Exclusive use of resources
2. Non-preemptible resources

**프로세스의 특성**

1. Hold and wait(Partial allocation) : 자원을 하나 hold하고 다른 자원 요청
2. Circular wait : 이런 요청 관계가 circular를 이룬다.

## Deadlock Prevention

Deadlock 발생 조건 4가지 중에 1개를 없애자

1. 모든 자원 공유 허용 → 현실적 불가능
2. 모든 자원 선점 허융 → 현실적 불가능, 자원 낭비
3. 필요 자원 한번에 모두 할당 → 자원 낭비
4. Circular wait 조건 제거 → 자원을 요청하는 순서를 부여 (자원들에 순서를 매기고 증가하는 방향으로만 자원 요청이 가능). 현실적이지만 자원 낭비 발생



## Deadlock Avoidance

deadlock이 발생할 것 같은 상황을 피해가자

- 시스템 상황을 계속 감시
- 시스템 deadlock이 발생할 것 같으면, 자원 할당 요청 보류
- 시스템을 항상 **safe state**로 유지

**safe state**

모든 프로세스가 정상적으로 종료 가능한 상태

safe sequence 존재

**unsafe state**

deadlock 발생할 가능성이 있음

반드시 발생한다는 의미는 아님.

Deadlock Avoidance 2가지 알고리즘

1. Dijkstra’s algorithm : Banker’s algorithm
2. Habermann’s algorithm

1. **Dijkstra’s algorithm**
    
    가정 : 한 종류의 자원이 여러개 있는 상황. 프로세스들은 필요한 자원이 모두 충족되면 사용하고 반납하게 됨.
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/11918c91-7d8d-4125-962b-14c91b7f848c)
    
    safe sequence : 모든 프로세스들이 순서대로 자원들을 사용할 수 있게 나누어 줄 수 있는 상황
    
    하나 이상의 safe sequence 존제 → deadlock 발생 안함.
    
2. **Habermann’s algorithm**
    
    Dijkstra’s algorithm 확장 → 자원 여러개 존재할 경우. safe sequence 알고리즘 자체는 동일
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/92de4f36-5e87-4cc9-ad61-1c0ae42ce636)
    

**결론**

Deadlock Avoidance로 deadlock이 발생할 것 같은 상황(unsafe state)을 피할 수 있다.

단점

- High Overhead : 항상 시스템을 감시하고 있어야 한다
- Low Resource Utilization : safe state유지를 위해 사용되지 않는 자원이 존재한다.
- Not practical : 프로세스 수, 자원 수가 고정되어있다고 가정했음. 비현실적

---

## Deadlock Detection & Recovery

- Deadlock 방지를 위한 사전 작업을 하지 않음. 발생하면 원인 찾고 recovery
- 주기적으로 deadlock이 발생했는지 확인 필요
- Deadlock 발생 확인하기 위해 Resource Allocation Graph(RAG) 사용

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/af692e51-56f7-46cb-9912-7bf3364e79e1)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/6330fb6d-2c50-4bf7-84f5-9890bd64be22)

**Graph Reduction**

주어진 RAG에서 edge를 하나씩 지워가는 방법

edge가 모두 지워진다면 → deadlock 발생 x

**edge 지우는 방법**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/ced93eb4-8cb4-4b53-bd79-361ccaad702b)

Pi가 요청하는 모든 Rj 자원에 대해,

Rj available한 자원이 있다면 edge를 지운다.

deadlock이 발생하는지 graph reduction으로 판단할 수 있지만,

**단점**

high overhead : 계속해서 이런 graph reduction을 진행해야함. 특히 노드가 많을 때는 overhead 크다.

## Deadlock Recovery

1. Process Termination
    
    deadlock 상태인 프로세스들 중 일부를 종료
    
    종료시킬 프로세스의 우선순위 기준이 필요함. (Termination cost model)
    
    - lowest termination cost process first(만만한 프로세스 먼저 종료)
        
        장점 : simple, low overhead
        
        단점 : 불필요한 프로세스들 종료 가능성 큼.
        
    - minimun cost recovery(최적의 판단)
        
        단점 : 모든 경우의 수 판단해야하므로 overhead 크다.
        
2. Resource Preemption
    
    deadlock 해결 위해 preemption(선점)할 자원을 선택
    
    해당 자원을 가지고 있는 프로세스를 종료시킴
    
    단점 : deadlock 상태가 아닌 프로세스가 종료될 수 있다.
    

**checkpoint-restart method**

1,2 모두 프로세스를 종료시키는 행위. 이를 위해 checkpoint-restart method기법 사용

프로세스의 수행 중 특정 checkpoint 마다 context를 저장. 향후 rollback 프로세스 재시작에 사용
