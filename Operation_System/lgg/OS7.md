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
