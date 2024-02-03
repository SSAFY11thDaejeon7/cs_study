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
![9일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/c0be0aff-08ea-459c-a537-f786bc3d4ec4)

- S33의 경우 P1, P2가 자원을 서로 반납하지 않고 자원을 요구하게 되므로 Deadlock 발생

### Deadlock 발생 필요 조건
- Exclusive use of resources: 자원의 특성
- Non-preemptible resources: 자원의 특성
- Hold and wait(Partial allocation): vmfhtptmdml xmrtjd
  - 자원을 하나 hold하고 다른 자원 요청
- Circular wait: vmfhtptmdml xmrtjd
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

