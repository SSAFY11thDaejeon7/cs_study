## Job vs Process
1. 작업/ 프로그램
  - 실행 할 프로그램 + 데이터
  - 컴퓨터 시스템에 실행 요청 전의 상태

2. 프로세스 ( Process )
  - 실행을 위해 시스템(커널)에 등록된 작업
  - 시스템 성능 향상을 위해 커널에 의해 관리 됨

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/8faf99e4-fb7f-4ee7-afd1-6e2ad25b358f" width="800px;" alt=""/>
<br>

## 프로세스의 정의
- 실행중인 프로그램
  - 커널에 등록되고 커널에 관리하에 있는 작업
  - 각종 자원들을 요청하고 할당 받을 수 있는 개체
  - 프로세스 관리 블록(PCB)을 할당 받은 개체
  - 능동적인 객체 (active entity)

※ Process Control Block (PCB) 
  - 커널 공간 내에 존재하며 각 프로세스들에 대한 정보를 관리함

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/7400291b-1435-4e2d-98b0-8df2d1049e8a" width="800px;" alt=""/>
<br>

## 자원의 개념
커널의 관리 하에 프로세스에게 할당/반납 되는 수동적 개체 (passive entity)
<br>

- 자원의 분류
    1) H/W resources : Processor, Momory, Disk, Monitor etc
    2) S/W resources : Message, signal, files etc
<br>
 
## Process Control Block (PCB)
OS가 프로세스 관리에 필요한 정보 저장하고, 프로세스 생성시 생성 됨<br>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/327d79e6-0880-41de-b1c9-5abea9949d45" width="400px;" alt=""/>
<br>

## PCB가 관리하는 정보
  1. PID : Process Identification Number
  2. 스케줄링 정보 : 프로세스 우선순위와 같은 정보
  3. 프로세스 상태 : 자원 할당 , 요청 정보
  4. 메모리 관리 정보 : Page table, segment table 등
  5. 입출력 상태 정보 : 할당 받은 입출력 장치, 파일 등에 대한 정보
  6. 문맥 저장 영역 (context save area) : 프로세스의 레지스터 상태 저장 공간
  7. 계쩡 정보 : 자원 사용 시간 등
  - PCB 정보는 OS 마다 다름
  - PCB 참조 및 갱신 속도는 OS의 성능을 결정 짓는 중요한 요소 중 하나
<br>

## 프로세스 상태 (Process States)
자원 간의 상호 작용에 의해 결정된다.
<br>

<h3>1. Created State</h3>
  - 작업(Program)이 커널에 등록되어 PCB를 할당받고 프로세스를 생성한 상태
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/d5735f0d-79f1-4313-87dc-673e5ebaced6" width="400px;" alt=""/>
<br><br>

<h3>2. Ready State</h3>
  - 프로세서 외에 다른 모든 자원(Memory)을 할당 받은 상태
  - 프로세서 할당 대기 상태로, 즉시 실행 가능한 형태
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/51b00d1a-b84e-4ad3-9133-97a67163214d" width="400px;" alt=""/>
<br><br>

  - Dispatch ( or Schedule) : 프로세서를 할당받음
<br><br>

<h3>3. Running State </h3>
  - 프로세서와 필요한 자원을 모두 할당 받은 상태
  <br>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/c4836d96-17f3-43db-b0f8-d84400ce3599" width="400px;" alt=""/>
<br><br>

  - Preemption : 할당된 시간이 충족되어 프로세서를 뺏기는 것
  - Block/Sleep : I/O 등의 자원 할당 요청에 프로세서를 뺏기고 자원이 도착할 때까지 대기
<br><br>

<h3>4. Blocked/Asleep State</h3>
  - 프로세서 외에 다른 자원을 기다리는 상태
  - System call에 의해 자원할당 이뤄짐
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/bf8c2b60-5a0e-4d78-b079-fe186f9f9dbe" width="400px;" alt=""/>
<br><br>

  - Wake-up : I/O 등의 대기하던 자원을 할당받음
<br><br>

<h3>5. Suspended State</h3>
  - 메모리를 할당 받지 못한(빼앗긴) 상태
  - Memory image를 swap device에 보관
  - 커널 또는 사용자에 의해 발생
  ( swap device : 프로그램 정보 저장을 위한 특별한 파일 시스템 )
  <br>
  
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/a47b8b34-fab9-4669-b7b3-18271ea414ab" width="400px;" alt=""/>
<br><br>

  - Swap-out(suspended) : memory image를 swap device에 저장하는 행동
  - Swap-in(resume) : swap device에 있는 정보를 다시 메모리에 되돌리는 행동
<br><br>
