# 교착 상태

### DeadLock의 개념

- Blocked/Asleep state
    - 프로세스가 특정 이벤트를 기다리는 상태
    - 프로세스가 필요한 자원을 기다리는 상태

- Deadlock state
    - 프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우
        - 프로세스가 deadlock 상태에 있음
    - 시스템 내에 deadlock에 빠진 프로세스가 있는 경우
        - 시스템이 deadlock 상태에 있음

asleep 상태 blocked 상태에 deadlock이 존재한다. 여기서 어떤 자원, 이벤트를 기다린다.

그런데 해당 이벤트가 일어날 가능성이 없는 경우 발생한다.

프로세스를 기다리고 있는 cpu starvation

우선순위가 계속 밀려 발생하는 starvation

### 선점 가능 여부에 따른 분류

- Preemptible resoutces
    - 선점 당한 후, 돌아와도 문제가 발생하지 않는 자원
        - cpu가 context switching 내부에 saving과 restoring 덕분에 돌아와도 문제가 발생하지 않는다.
    - Processor, memory 등

- Non-preemptible resources
    - 선점 당하면, 이후 진행에 문제가 발생하는 자원
        - Rollback, restart 등 특별한 동작이 필요
    - disk dive 등
    

### 할당 단위에 따른 분류

- Total allocation resources
    - 자원 전체를 프로세스에게 할당
    - Processor, disk dirve 등

- Partitioned allocation resources
    - 하나의 자원을 여러 조작으로 나누어, 여러 프로세스들에게 할당
    - Memory 등

### 동시 사용 가능 여부에 따른 분류

- Exclusive allocation resources
    - 한 순간에 한 프로세스만 사용 가능한 자원
    - Processor, memory, disk drive 등

- Shared allocation resource
    - 여러 프로세스가 동시에 사용 가능한 자원
    - Program(sw), shared data

### 재 사용 가능 여부에 따른 분류

- SR(Serially-reusable Resources)
    - 시스템 내에 항상 존재 하는 자원
    - 사용이 끝나면, 다른 프로세스가 사용 가능
    - Processor, memory, disk drive, program 등

- CR (Consumable Resources)
    - 한 프로세스가 사용한 후에 사라지는 자원
    - signal, message
    

### Deadlock과 자원의 종류

- Deadlock을 발생시킬 수 있는 자원의 형태
    - Non-preemptible resources
    - Exclusive allocation resources
    - Serially reusable resources
    - 할당 단위는 영향을 미치지 않음

### Graph Model

- Node
    - 프로세스 노드(P1, P2), 자원 노드(R1, R2)
- Edge
    - Rj → Pi : 자원 Rj 이 프로세스 Pi에 할당 됨
        - p1이 r2 자원을 할당 받았다
    - Pi → Rj: 프로세스 Pi가 자원 Rj을 요청 (대기중)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ee82f485-7a24-4585-a1f8-76d0368b5799/5c7a3ca0-613b-486d-9b95-d98fd8117251/Untitled.png)

### State Transition Model

예제 

- 2개의 프로세스와 A type의 자원 2개(unit) 존재
- 프로세스는 한번에 자원 하나만 요청/반납 가능

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ee82f485-7a24-4585-a1f8-76d0368b5799/0c3b3d56-d38f-4ea5-b115-f6ec93d386a9/Untitled.png)

### Deadlock 발생 필요 조건

자원의 특성

- Exclusive use of resources
- Non-preemptible resources

프로세스의 특성

- Hold and wait (Partial allocation)
- Circular wait

### Deadlock 해결 방법

- 교착상태 예방
- 교착상태 회피
- 교착상태 탐지 및 복구

### Deadlock Prevention

- **4개의 deadlock 발생 필요 조건 중 하나를 제거**
    - Exclusive use of resources
    - Non-preemptible resources
    - Hold and wait (Partial allocation)
    - Circular wait
- **Deadlock이 절대 발생하지 않음**

- 모든 자원을 공유 허용
    - Exclusive use of resources 조건 제거
    - 현실적으로 불가능

- 모든 자원에 대해 선점 허용
    - Non-preemptible resources 조건 제거
    - 현실적으로 불가능
    - 유사한 방법
        - 프로세스가 할당 받을 수 없는 자원을 요청한 경우, 기존에 가지고 있던 자원을 모두 반납하고 작업 취소
            - 이후 처음 (또는 check-point)부터 다시 시작
        - **심각한 자원 낭비 발생 → 비현실적**

- 필요 자원 한번에 모두 할당 (total allocation)
    - hold and wait 조건 제거
    - 자원 낭비 발생
        - 필요하지 않은 순간에도 가지고 있음
    - 무한 대기 현상 발생 가능

- circular wait 조건 제거
    - totally allocation을 일반화 한 방법
    - 자원들에게 순서를 부여
    - 프로세스는 순서의 증가 방향으로만 자원 요청 가능
    - 자원 낭비 발생
