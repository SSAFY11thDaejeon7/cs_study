# 프로세스

실행을 위해 시스템(커널)에 등록된 작업

시스템 성능 향상을 위해, 커널을 통해 관리됨

### 작업 / 프로그램

- 실행 할 프로그램 + 데이터
- 컴퓨터 시스템에 실행 요청 전의 상태

### 정의

- 실행중인 프로그램
- Process Control Block(PCB)

### 종류

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/93cc7705-8f28-4254-8b44-9900c9c6cd5c)



# 자원(Resocure)

커널의 관리 하에 프로세스에게 할당/반납되는 수동적 개체

### 분류

- HW 자원
    - 프로세서, 메모리, 디스크, 모니터, 키보드 등
- SW 자원
    - 메시지, 신호, 파일, 설치된 SW 등

# Process Control Block(PCB)

OS가 프로세스 관리에 필요한 데이터 저장

프로세스 생성 시 생성됨

**커널이 관리하는 영역**에 저장

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/86a03c4b-e8c4-4f0a-ad4b-d590f2f9ce52)



## PCB가 관리하는 정보

- PID(Process Identification Number)
    - 프로세스 고유 식별 번호
- 스케줄링 관련 정보
    - ex) 프로세스 우선 순위
- 프로세스 상태
    - 자원 할당, 요청 정보 등
- 메모리 관리 정보
- 입출력 상태 정보
- 문맥(Context) 저장 영역
    - 프로세스의 레지스터 상태 저장 공간 등
- 계정 정보

# 프로세스 상태(Process Status)

자원 간의 상호작용에 의해 결정

### 프로세스 상태 및 특성

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/88b91489-2930-418a-bf5c-b905eaf681fb)



  
**프로세스 상태 교환 다이어그램**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/f9051883-fc82-40ce-b735-336dbf5d487e)



## Create State

- 작업을 커널에 등록
- PCB 할당 및 프로세스 생성
- 커널은 가용 메모리 공간을 체크 및 프로세스 상태를 전이
    - Ready or Suspended ready
- Memory 상황에 따라 Ready or Suspended ready 상태로 전환

## Ready State

- 프로세서 외에 다른 모든 자원을 할당 받은 상태
    - 프로세스 할당을 대기하며, 즉시 실행 가능한 상태
- Dispatch(Schadule)시 Ready → Running 으로 전환

## Running State

- 프로세스와 필요한 자원을 모두 할당 받은 상태
- Preemption(Timer run-out)
    - 프로세스 스케줄링에 따라 다른 프로세스가 들어오거나, 할당된 시간이 끝난 경우
    - Running → Ready로 전환
- Block/Sleep
    - I/O 등 자원 할당 요청
    - Running → Asleep으로 전환 (I/O를 기다리는 상태)

## Blocked/Asleep State

- 프로세서 외에 다른 자원을 기다리는 상태
    - 자원 할당은 System call에 의해 이루어짐
- Wake-up
    - 기다리던 데이터가 도착 한 경우
    - Asleep → Ready로 전환

## Suspended State

- 메모리를 할당 받지 못한(빼앗긴) 상태
    - Memory image를 swap device에 보관
- 커널 또는 사용자에 의해 발생

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/5943bfe7-df7f-4338-9b79-5cd1d40833ba)



- Suspended ready
    - Memory가 없이 Ready인 상태
- Suspended block
    - blocked에서 memory를 빼앗긴 상태

- swap-out
    - memory를 뺏기며 swap device에 저장하는 것
- swap-in
    - 메모리를 할당 받아 복구하는 것

## Terminated/Zombie State

- 프로세스 수행이 끝난 상태
- 모든 자원 반납 후, 커널 내에 일부 PCB 정보만 남아 있는 상태
    - 이후 프로세스 관리를 위해 정보 수집

# 인터럽트(Interrupt)

예상치 못한, 외부에서 발생한 이벤트

### 종류

- I/O interrupt
- Clock interrupt
- Console interrupt
- Program check interrupt
- Machine check interrupt
- Inter-process interrupt
- System call interrupt

### 인터럽트 처리 과정

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/b2b51243-eee2-4dc6-8148-597239f86ca1)



# Context Switching(문맥 교환)

- Context
    - 프로세스와 관련된 정보들의 집합
- Context saving
    - 현재 프로세스의 Register context를 저장하는 작업
- Context restoring
    - Register context를 프로세스로 복구하는 작업
- Context switching
    - Context saving + resoring
    - 실행 중인 프로세스의 context를 저장하고, 앞으로 실행 할 프로세스의 context를 복구하는 일

### Context switch Overhead

- Context switching에 소요되는 비용
    - OS마다 다름
    - OS의 성능에 큰 영향을 줌
- 불필요한 Context switching을 줄이는 것이 중요
    - ex) 스레드 사용 등
