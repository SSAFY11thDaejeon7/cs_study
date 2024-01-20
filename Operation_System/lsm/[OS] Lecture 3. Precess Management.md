# 1. 프로세스의 개념
### 작업(job) / 프로그램(Program) vs 프로세스

[작업(job) / 프로그램(Program)]

- 실행 할 프로그램 + 데이터 (우리가 짠 프로그램)
- 컴퓨터 시스템에 실행 요청 전의 상태
- Disk에 보관되어 있음

[프로세스]

- job이 시스템에 등록된 상태 (메모리를 할당받음)
- 실행을 위해 시스템(커널)에 등록된 작업
- 시스템 성능 향상을 위해 커널에 의해 관리 됨

### 프로세스의 정의

- 실행중인 프로그램
    - 커널에 등록되고 **커널의 관리**하에 있는 작업
    - 각종 **자원들을 요청**하고 할당 받을 수 있는 개체
    - 프로세스 관리 블록 (**PCB**)을 할당 받은 개체
    - 능동적인 개체(active entity) : 실행 중에 각종 자원을 요구, 할당, 반납하며 진행
- PCB(Process Control Block)
    - 커널 공간 (kernel space) 내에 존재
    - 각 프로세스들에 대한 정보를 관리

### 프로세스의 종류
<img width="999" alt="프로세스의 종류" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/397317ff-1db3-44af-9216-2824c7fefdee">


### 자원의 개념

- 커널의 관리 하에 프로세스에게 할당/반납 되는 수동적 개체(passive entity)
- 자원의 분류
    - H/W resources
        - Processor, memory, disk, monitor, keyboard
    - S/W resources
        - Message, signal, files, installed SWs

# 2. PCB(Process Control Block)

### PCB란

- OS가 프로세스 관리에 필요한 상태 정보를 저장함.
- 프로세스 생성 시 생성된다.
- 커널에 저장이 된다.
<img width="816" alt="PCB" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/6d9f4925-0d87-4915-b98b-92836ca8b63e">


### PCB가 관리하는 정보

- PID: Process Identification Number
    - 프로세스 고유 식별 번호
- 스케줄링 정보
    - 프로세스 우선순위 등과 같은 스케줄링 관련 정보들
- 프로세스 상태
    - 자원 할당, 요청 정보 등
- 메모리 관리 정보
    - Page table, segment table 등
- 입출력 상태 정보
    - 할당 받은 입출력 장치, 파일 등에 대한 정보 등
- 문맥 저장 영역 (context save area)
    - 프로세스의 레지스터 상태를 저장하는 공간 등
- 계정 정보
    - 자원 사용 시간 등을 관리

> - PCB 정보는 OS 별로 서로 다름
> - PCB 참조 및 갱신 속도는 OS의 성능을 결정 짓는 중요한 요소 중 하나임

# 3. 프로세스의 상태 (Process States)

> 프로세스는 시스템에 등록된 후, 여러 가지 상태를 거치면서 작업을 수행한다 (**자원 간의 상호작용**에 의해 결정)
> 

### 프로세스의 상태 및 특성
<img width="935" alt="프로세스의 상태 및 특성" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/773479f6-ab90-4efe-9a3f-796da3681636">


### Process State Transition Diagram
<img width="556" alt="Process State Transition Diagram" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/a92e540b-2bac-45cf-a238-548e13f59009">


### Active 상태 (메모리 O)

- Created State (생성된 상태)
    - 작업 (job)을 커널에 등록
    - PCB 할당 및 프로세스 생성
    - 커널이 가용 메모리 공간을 체크하고 프로세스 상태를 전이한다. (Ready or Suspended ready)
- Ready State
    - 프로세서 외에 다른 모든 자원을 할당 받은 상태
        - 메모리를 할당받음
        - 프로세서(CPU) 할당 대기 상태
        - 즉시 실행 가능 상태
    - Dispatch (or Schedule)
        - Ready State → Running State
- Running State
    - 프로세서와 필요한 자원을 모두 할당 받은 상태
        - Preemption (CPU를 뺏김)
            - Running State → Ready State
            - 프로세서 스케줄링 (ex. time-out, priority changes)
        - Block/sleep
            - Running State → asleep State
            - I/O 등 자원 할당 요청
- Blocked/Asleep ~~~~~~~~~~State~~~~~~~~~~
    - 프로세서 외에 다른 자원을 기다리는 상태
        - 자원 할당은 system call에 의해 이루어짐
        - 자원마다 따로 큐로 각각 관리함
    - Wake-up
        - Asleep State → Ready State

### Suspended 상태 (메모리 X)

- Suspended State
    - 메모리를 할당 받지 못한(빼앗긴) 상태
        - Memory image를 swap device에 보관
        - Memory image: 메모리를 빼앗길때 메모리를 사용하던 상태를 image로 저장한 것
        - swap device: 프로그램 정보 저장을 위한 특별한 파일 시스템 (일종의 하드 디스크)
        - 커널 또는 사용자에 의해 발생
    - Swap-out (Suspended), Swap-in (resume)
- Terminated/Zombie State
    - 프로세스 수행이 끝난 상태
    - 모든 자원 반납 후, 커널 내에 일부 PCB 정보만 남아 있는 상태
    - 이후 프로세스 관리를 위해 정보를 수집한다.

# 4. 인터럽트 (Interrupt)

> 예상치 못한, 외부에서 발생한 이벤트 (Unexpected, external events)
> 

### 인터럽트의 종류

- I/O interrupt: 게임과 같은 프로세스가 예상하지 못한 순간에 마우스 클릭 등을 하면, 이를 알려주는 것
- Clock interrupt
- Console interrupt
- Program check interrupt
- Machine check interrupt
- Inter-process interrupt
- System call interrupt

### 인터럽트 처리 과정

1. 인터럽트 발생
2. 프로세스 중단(커널의 개입)
    - context saving 수행 (문맥을 PCB에 저장한다)
3. 인터럽트 처리 (interrupt handling)
    - 인터럽트 발생 장소, 원인 파악
    - 인터럽트 서비스 할 것인지 결정 (어떤 서비스 루틴 호출할 것인지)
    - 인터럽트 서비스 루틴 호출
    - Ready 상태에 있던 프로세스 중 하나에 CPU를 할당한다. (context를 바탕으로 복구한다)

# 5. Context Switching (문맥 교환)

- Context
    - 프로세스와 관련된 정보들의 집합
        - CPU Register context ⇒ in CPU 
        (CPU가 어떤 프로세스를 처리할 때는 반드시 해당 데이터를 레지스터에 올린다. 그 레지스터에 있는 context)
        - Code & Data & Stack & PCB ⇒ in memory
- Context Saving
    - 현재 프로세스의 Register context를 저장하는 작업
- Context restoring
    - Register context를 프로세스로 복구하는 작업
- Context Switching
    - 실행중인 프로세스의 context를 저장하고, 앞으로 실행할 프로세스의 context를 복구하는 일
    - 커널의 개입으로 이루어짐

### Context Switch Overhead

- Context switching에 소요되는 비용
    - OS마다 다름
    - 매우 자주 발생함 ⇒ OS의 성능에 큰 영향을 줌
- 불필요한 Context switching을 줄이는 것이 중요하다
    - ex ) **스레드**를  활용해서 멀티스레드 프로그래밍을 한다.
