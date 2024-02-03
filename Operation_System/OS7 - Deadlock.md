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

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/3aaceace-b7bb-4001-9734-1bf04273410a)


- Node
    - 프로세스 노드(P1, P2), 자원 노드(R1, R2)
- Edge
    - Rj → Pi: 자원 Rj이 프로세스 Pi에 할당 됨
    - Pi → Rj: 프로세스 Pi가 자원 Rj을 요청(대기 중)
    

### State Transition Model

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/8a8aa9f5-f757-4c67-a5ab-22e332c74ce5)


- 예제
    - 2개의 프로세스와 A type의 자원 2개 존재
    - 프로세스는 한번에 하나의 자원만 요청/반납 가능

**Process가 2개일 때 Model**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/61f45ffb-4812-412d-bc5f-0134dec8658e)


## Deadlock 발생 필요 조건

**자원의 특성**

- Exclusive allocation resources
- Non-preemptible resources

프로세스의 특성

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
