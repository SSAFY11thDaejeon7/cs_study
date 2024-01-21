# 3강

# 프로세스

---

## 개념

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/5c9cf51d-9f8a-4ebc-970d-f12b5a43592f)
- **Job/Program 프로그램**
    
    아직 실행되지 않은, 디스크에 있는 상태인 프로그램을 일컬음.
    
- **프로세스**
    
    실행을 위해 시스템(커널)에 등록된 프로그램, 메모리를 할당받은 프로그
    
    OS 커널은 시스템 향상을 위해 이런 프로세스들을 관리
    
    **정의**
    
    - 커널에 등록되고 커널의 관리하에 있는 프로그램
    - 각종 자원들을 요청하고 할당 받을 수 있는 개체
    - 프로세스 관리 블록(PCB)을 할당 받은 개체

**PCB(Process Control Block)**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/3aae383b-a2b8-46d6-bf3e-b6c25dae9154)

OS가 프로세스 관리를 위해 필요한 정보들을 저장

- PID : 프로세스 고유 식별 번호
- 스케줄링 정보 : 프로세스 우선순위 등 스케줄링 관련 정보
- 프로세스 상태 : 자원 요청, 할당 정보
- 메모리 관리 정보 : page table, segmentation table 정보
- 입출력 상태 정보 : 할당 받은 입출력 장치, 파일 정보
- 문맥(context) 저장 영역 : 프로세스마다의 context 정보를 저장하는 공간
- 계정 정보 : 자원 사용 시간 등

프로세스 생성 시에 같이 생성됨. 커널 공간 내에 PCB가 존재

cf. PCB 정보는 OS별로 서로 다르다. PCB 참조 및 갱신 속도는 OS성능을 결정짓는 중요한 요소 중 하나이다.

**자원(Resource)**

1. HW 자원 : 프로세서, 메모리, I/O 장치 등
2. SW 자원 : 메시지,파일,신호 등

## 프로세스 상태 (자원 간의 상호작용에 의해 상태 결정)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/a6dad8e3-2b71-49c9-8aca-27f6af29fb92)
### Created State

**정의**

프로그램(Job)이 실행을 위해 커널에 등록된 상태

PCB 할당 및 프로세스 생성된 상태

OS는 가용 메모리 공간을 체크하고 프로세스를 Ready State or Suspended state로 보냄.

## 1. created state 이후에 메모리를 할당 받은 상태(active)

### Ready State

**정의**

프로세서(CPU) 외에 모든 자원(메모리 포함 HW 및 SW 자원)을 할당 받은 상태

CPU 할당을 기다리고 있다가, 할당 받으면 Dispatch(Schedule) 작업 후에 Running state로 가게 된다.

### Running State

**정의**

모든 자원과 CPU까지 할당 받은 상태. 프로세스 실제 실행 상태

1. Preemption : 프로세서(CPU)만 다시 뺏기면, Ready state로 다시 쫓겨남. 자원은 모두 준비 완료
    
    프로세스에 할당된 시간 초과 or 프로세스 우선순위 밀렸을 때
    
2. Block/Sleep : 자원 할당을 요청해야 할 때, 즉 CPU가 있어도 실행 못하는 상태일 때 **Asleep(Blocked) State**로 가게 된다.

Asleep상태에서 I/O 등 자원이 준비되어도 곧바로 Running State로 갈 수 없다. 다시 Ready 상태로 가서 순서 기다려야함.

### Terminated State

**정의**

프로세스 수행이 모두 끝났을 때, 모든 자원 반납 후 커널에게 PCB 정보를 제출 (커널이 이후 프로세스 관리를 위해 정보 수집)

## 2. created state 이후에 메모리를 할당 받지 못한(뺏긴) 상태(suspended)

**swap**

1 → 2 : swap-out (suspend 지연)

2 → 1 : swap-in (resume 재개)

swap이 일어날 때마다 swap device에 프로세스마다 메모리 현황을 image로 저장한다.

### Suspended Ready State

### Suspended Blocked State

## 프로세스 관리를 위한 메모리 구조

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/fa07898b-4c1f-4161-bb01-ef335d8cb4bc)
- Ready state : 스케줄러에 의해 선택되는 프로세스들의 Queue
- Blocked/asleep state : CPU 이외에 자원 요청 상황. 자원 및 device 별로 요청을 기다리는 Queue

## 인터럽트 Interrupt

**개념**

예상치 못한, 외부에서 발생한 이벤트

**종류**

- I/O interrupt : 키보드,마우스 등에 의한
- Clock interrupt : cpu clock에 의한
- Console interrupt : 콘솔 입력에 의한
- Program check interrupt/ Machine check interrupt : 프로그램이나 hw 이슈에 의한
- Inter-Process interrupt : 다른 프로세스 간
- System call interrupt : system call 에 의한

**처리 과정**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/eb339764-93b6-485b-9744-91f3485f3e33)

인터럽트 발생 → 프로세스 중단 → 인터럽트 처리 (Interrupt handling + Interrupt service)

- 프로세스 중단 : PCB에 context saving 작업

- Interrupt handling : 인터럽트 발생 장소 및 원인 파악, interrupt service 여부 결정
- Interrupt service : 인터럽트 서비스 루틴 호출. 인터럽트도 하나의 프로세스이기 때문에 프로세서에게 인터럽트 프로세스 처리 요청

인터럽트 처리가 끝나면, 다시 ready state에 있는 기존 프로세스들이 자신의 PCB에 있는 context 정보 가져와서 수행 시작

## Context 란?

**정의**

프로세스가 현재 어떤 상태에서 수행되고 있는지 정확히 규명하기 위해 필요한 정보

**종류**

프로세스와 관련된 정보들의 집합

- HW context : cpu 수행상태. PC와 각종 레지스터에 저장하고 있는 값들
- 프로세스의 주소공간 context : code,data,stack 으로 구성된 프로세스마다의 독자적인 주소 공간
- 커널상의 context : PCB와 Kernel Stack(커널내의 주소)

**context saving**

향후에 프로세스가 Running state로 실행될 때, 프로세스의 문맥을 불러와서 실행하기 위해 context 정보들을 해당 프로세스의 PCB에 저장하는 것

**context restoring**

프로세스를 다시 수행할 때 saving된 context를 PCB로부터 불러오는 것

**context(process) switching**

실행 중인 프로세스의 context saving, 실행할 프로세스의 context restoring이 일어남.

커널의 개입으로 context switching이 일어나게 된다.

불필요한 context switching은 OS 성능에 영향을 끼칠 수 있으므로 최소화 하는 것이 중요하다. → 쓰레드 사용 등으로 해결
