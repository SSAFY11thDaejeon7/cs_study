# 프로세스 (Process)
## Job VS Process
- 작업 (Job) / 프로그램 (Program)
  - 실행 할 프로그램 + 데이터
  - 컴퓨터 시스템에 실행 요청 전의 상태
- 프로세스 (Process)
  - 실행을 위해 시스템(커널)에 등록된 작업
  - 시스템 성능 향상을 위해 커널에 의해 관리 됨
    
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/35c9f6c1-62e2-41f9-88a3-5e6c90d68498)

## 프로세스의 정의
- 실행중인 프로그램
  - 커널에 등록되고 커널의 관리하에 있는 작업
  - 각종 자원들을 요청하고 할당 받을 수 있는 개체
  - 프로세스 관리 블록(PCB)을 할당 받은 개체
  - 능동적인 개체(active entity)
    - 실행 중에 각종 자원을 요구, 할당, 반납하며 진행
   
  - Process Control Block (PCB)
    - 커널 공간 (kernel space) 내에 존재
    - 각 프로세스들에 대한 정보를 관리

## 프로세스의 종류
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/9ba05439-e741-4282-b785-e64f24c4415c)

## 자원(Resource)의 개념
- 커널의 관리 하에 프로세스에게 할당/반납 되는 수동적 개체(passive entity)
- 자원의 분류
  - H/W resources
    - Processor, memory, disk, monitor, keyboard, Etc.
  - S/W resources
    - Message, signal, files, installed SWs, Etc.

# Process Control Block (PCB)
## PCB란?
- OS가 프로세스 관리에 필요한 정보 저장
- 프로세스 생성 시 생성됨
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/997eef6f-cd40-4796-a4e4-df62d3508d08)

## PCB가 관리하는 정보
- PID : Procesas Identification Number
  - 프로세스 고유 식별 번호
- 스케줄링 정보
  - 프로세스 우선순위 등과 같은 스케줄링 관련 정보들
- 프로세스 상태
  - 자원 할당, 요청 정보 등
- 메모리 관리 정보
  - Page table, setgment table 등
- 입출력 상태 정보
  - 할당 받은 입출력 장치, 파일 등에 대한 정보 등
- 문맥 저장 영역 (context save area)
  - 프로세스의 레지스터 상태를 저장하는 공간 등
- 계정 정보
  - 자원 사용 시간 등을 관리
> - PCB 정보는 OS 별로 서로 다름
> - PCB 참조 및 갱신 속도는 OS의 성능을 결정 짓는 중요한 요소 중 하나

# 프로세스의 상태 (Process States)
- 프로세스 - 자원 간의 상호작용에 의해 결정
##  프로세스 상태 및 특성
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/1d94f5a1-3efc-460f-b16f-1df53da3e1a4)

## 프로세스 상태 변화 다이어그램
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/2b0b8fb3-02ad-40e1-821b-9829ef098c0a)

## Created State
- 작업을 커널에 등록
- PCB 할당 및 프로세스 생성
- 커널
  - 가용 메모리 공간 체크 및 프로세스 상태 전이 (Ready or Suspended ready)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/4885d822-ddc1-4be0-864f-fffc3ce903de)

## Ready State
- 프로세서 외에 다른 모든 자원을 할당 받은 상태
  - 프로세서 할당 대기 상태
  - 즉시 실행 가능 상태
- Diospatch (or Schedule)
  - Ready state -> running State
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/fa1cf5eb-fa93-41ba-9f33-f4100103e6e4)

## Running State
- 프로세서와 필요한 자원을 모두 할당 받은 상태
- Preemption
  - Ruinning state -> ready states
  - 프로세서 스케줄링(time-out, priority changes)
- Block/sleep
  - Running state -> asleep state
  - I/O 등 자원 할당 요청
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/4c081b5b-2f08-4e35-8c2f-bf04e4666963)

## Blocked/Asleep State
- 프로세서 외에 다른 자원을 기다리는 상태
  - 자원 할당은 System call에 의해 이루어짐
- Wake-up
  - Asleep state -> ready state
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/112ceb53-55f7-45a9-afe8-434a32209cf7)

## Suspended State
- 메모리를 할당 받지 못한(빼앗긴) 상태
  - Memory image를 swap device에 보관(Swap device: 프로그램 정보 저장을 위한 특별한 파일 시스템)
- 커널 또는 사용자에 의해 발생
- Swap-out(suspended), Swap-in(resume)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/79f0a2ac-6531-4760-b118-4dbcdd46e087)

## Terminated/Zombie State
- 프로세스 수행이 끝난 상태
- 모든 자원 반납 후 커널 내에 일부 PCB정보만 남아 있는 상태
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/228ee7ac-b6b7-4d0f-892b-4ae434fece88)

## 프로세스 관리를 위한 자료 구조
- Ready Queue
- I/O Queue
- Device Queue
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/a40d104c-9e55-4113-8f97-57730097639e)

# 인터럽트(Interrupt)
- 예상치 못한(Unbexpected), 외부에서(external events) 발생한 이벤트
- 인터럽트의 종류
  - I/O interrupt, Clock interrup, Console interrupt, Program check interrupt, Machine check interrupt, Inter-process interrupt, System call interrupt

## 인터럽트의 처리 과정
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/e316e4b3-6e13-4779-bee1-ff0838e8fdea)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/20576793-a3b7-468c-b341-69f81fab03d3)

# Context Switching(문맥 교환)
- Context
  - 프로세스와 관련된 정보들의 집합
- Context saving
  - 현재 프로세스의 Register context를 저장하는 작업
- Context restoring
  - 현재 Register context를 프로세르로 복구하는 작업
- Context switching (= Process switching)
  - 실행 중인 프로세스의 context를 저장하고 앞으로 실행 할 프로세스의 context를 복구 하는 일
- Context Switch Overhead
- Context switching에 소요되는 비용
  - OS마다 다름
  - OS의 성능에 큰 영향을 줌
- 불필요한 Context switching을 줄이는 것이 중요
  - 스레드 사용 등
