# 프로세스 관리

## 프로세스

- 실행을 위해 시스템(커널)에 등록된 작업
- 시스템 성능 향상을 위해 커널에 의해 관리 됨
- 프로세스 관리 블록(PCB)을 할당 받는 개체

## Process Control Block (PCB)

- 커널 공간 내에 존재
- 각 프로세스들에 대한 정보를 관리
- OS가 프로세스 관리에 필요한 정보 저장
- 프로세스 생성 시, 생성된다.

### PCB가 관리하는 정보

- PID : Process Identification Number
- 스케줄링 정보: 프로세스 우선순위 등과 같은 스케줄링 관련 정보들
- 프로세스 상태: 자원 할당, 요청 정보 등
- 메모리 관리 정보: page table, segment table 등
- 입출력 상태 정보: 할당 받은 입출력 장치, 파일 등에 대한 정보 등
- 문맥 저장 영역 (context save area): 프로세스의 레지스터 상태를 저장하는 공간 등
- 계정 정보: 자원 사용 시간 등을 관리

PCB 정보는 운영체제 별로 서로 다르다.

PCB 참조 및 갱신 속도는 OS의 성능을 결정 짓는 중요한 요소 중 하나이다.

## 프로세스의 종류

### 시스템(커널) 프로세스

모든 시스템 메모리와 프로세서의 명령에 액세스할 수 있는 프로세스이다.

프로세스 실행 순서를 제어하거나 다른 사용자 및 커널(운영체제) 영역을 침범하지 못하게 감시하고, 사용자 프로세스를 생성하는 기능을 한다.

### 사용자 프로세스

사용자 코드를 수행하는 프로세스이다.

### 독립 프로세스

다른 프로세스에 영향을 주지 않거나 다른 프로세스의 영향을 받지 않으면서 수행하는 병행 프로세스이다.

### 협력 프로세스

다른 프로세스에 영향을 주거나 다른 프로세스에서 영향을 받는 병행 프로세스이다.

## 자원

커널의 관리 하에 프로세스에게 할당/반납 되는 수동적인 개체이다. 

ex)

하드웨어 자원 - 프로세서, 메모리, 디스크, 모니터, 키보드, …

소프트웨어 자원 - 메세지, signal, 파일, 설치된 소프트웨어, …

## 프로세스의 상태

자원 간의 상호작용에 의해 결정된다.

![](https://github.com/ensk26/cs_study/assets/70767115/5484399d-f7a7-46a7-8490-fe879ec814d9)

### created state

- 작업을 커널에 등록
- PCB 할당 및 프로세스 생성
- 커널이 사용할 수 있는 메모리가 있는지 확인 및 프로세스 상태 전의
    - ready or suspended ready

### ready state

프로세서 외에 다른 모든 자원을 할당 받은 상태이다.

- 프로세서 할당 대기 상태
- 즉시 실행 가능 상태

### Running State

- 프로세서와 필요한 자원을 모두 할당 받은 상태
- Preemption : 프로세서를 빼앗겨서 runing → ready 상태로 변화는것
    - 프로세서 스케줄링(time-out, priority changes)
- Block/Sleep: running state → asleep state
    - asleep: i/o가 끝나길 기다리는 상태
    - I/O 등 자원 할당 요청

### Blocked/Asleep State

- 프로세서 외에 다른 자원을 기다리는 상태
    - 자원 할당은 system call에 의해 이루어짐
- Wake-up
    - Asleep state → ready state

다른 프로세스가 들어와서 갑자기 끼어 들면 안되기 때문에 wait로 가서 다음 cpu를 기다립니다.

### Suspended State

- 메모리를 할당 받지 못한(빼앗긴) 상태
- memory image를 swap device에 보관
    - memory image: 메모리의 상태를 찍어놓은 곳
    - swap device: 프로그램 정보 저장을 위한 특별한 파일 시스템
- 커널 또는 사용자에 의해 발생
- suspended ready: 메모리가 없고 레디하는 상태이다.
- suspended blocked: i/o를 기다리고 있는데(asleep) 메모리 공간도 뺐겼을때, suspended blocked 상태로 간다.

### swap-out, swap-in

메모리를 다시 할당 받을 때 이전에 했던 작업을 이어서 하기 위해 memory image로 메모리 상태를 저장합니다.

메모리를 뺏기면서 메모리 정보를 저장하는 과정을 swap-out(suspended)라고 하고, 다시 메모리를 할당 받아 해당 정보를 복구하는 것을 swap-in(resume) 이라고 한다.

### Terminated/Zombie state

- 프로세스 수행이 끝난 상태
- 모든 자원 반납 후, 커널 내에 일부 PCB 정보만 남아 있는 상태
    - 이후 프로세스 관리를 위해 정보 수

terminated에서는 모든 자원을 다 반납한 상태이면서, pcb 정보만 남아 있는 상태이다. 커널이 해당 정보를 수집하고 삭제한다.

![](https://github.com/ensk26/cs_study/assets/70767115/e7ebcee6-8a80-4064-b020-d040785e1c5b)

### 프로세스 관리를 위한 자료 구조

![](https://github.com/ensk26/cs_study/assets/70767115/9972a0d6-c68d-4448-a5f1-c349a8ed060c)

- Ready Queue(list): 프로세스들이 줄서 있는 공간
- Blocked Lists: asleep 상태에서는 프로세스 외 다른 자원들을 기다리고 있어, 자원 별로 큐를 따로 관리하고 있다.
    - ex) i/o queue, device queue

# 인터럽트

## 인터럽트

- 예상치 못한, 외부에서 발생한 이벤트

### 인터럽트 종류

- I/O interrupt
- Clock interrupt
- Console interrupt
- Program check interrupt
- Machine check interrupt
- Inter-process interrupt
- System call interrupt

### 인터럽트 처리 과정

![](https://github.com/ensk26/cs_study/assets/70767115/bffebbc3-390e-4939-aeb3-decaefae2fd2)

프로세서 내에 p1이라는 프로세스가 존재할 때

1. p1에 인터럽트를 발생하면 프로세스를 중단 시킨다.
2. PCB에 Context saving을 한다.
3. 커널이 개입을 해서 첫 인터럽트 발생 장소와 원인을 파악한다.
4. 파악 후, 어떤 인터럽트 서비스 루틴(ISR)을 호출할지 결정을 한다.
5. 해당 서비스 루틴을 프로세서에서 처리하도록 넣어준다.
    1. 서비스 루틴도 프로그램이기 때문

context saving: 일의 진행 상황을 저장

## Context Switching (문맥 교환)

### Context

- 프로세스와 관련된 정보들의 집합
    - CPU register context ⇒ CPU 안에 있던 내용
    - Code &data, Stack, PCB ⇒ in memory

### Context saving

- 현재 프로세스의 Register context를 메모리에 저장하는 작업

### Context restoring

- Register context를 프로세스로 복구하는 작업

### Context switching

- 실행 중인 프로세스의 context를 저장하고, 앞으로 실행할 프로세스의 context를 복구 하는 일
    - 커널의 개입으로 이루어짐
    

### Context Switch Overhead

- Context Switch은 자주 발생 하므로 OS의 성능에 큰 영향을 준다.
    - 프로세스 개수만 생각해도 많다.
- 그래서 불필요한 Context switching을 줄이는 것이 중요하다.
    - 스레드(thread) 사용 등
