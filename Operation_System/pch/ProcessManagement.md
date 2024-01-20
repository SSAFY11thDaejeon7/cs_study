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

<h3>6. Terminated/Zombie State</h3>
  - 프로세스 수행이 끝난 상태
  - 모든 자원 반납 후, 커널 내에 일부 PCB 정보만 남아 있는 상태
    - 이후에 다시 프로세스를 사용할 수 있으므로관리를 위해 PCB 정보를 수집하기 위해 있는 과정
<br>

## 프로세스의 상태
  - 자원 간의 상호작용에 의해 결정
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/972ef768-52eb-4cf2-9371-b0fb25677046" width="800px;" alt=""/>
<br><br>

## 인터럽트
  - 예상치 못한, 외부에서 발생한 이벤트
<br>

1. 인터럽트의 종류
<br>

- I/O interrupt
- Clock interrupt : CPU의 클락이 발생할 때
- Console interrupt : 콘솔창에서 발생
- Program check interrupt : 프로그램에 문제가 있을 때
- Machine check interrupt : 하드웨어에 문제가 있을 때
- inter-process interrupt : 다른 시스템이 문제 호출
- System call interrupt : 시스템 콜 호출시 발생
<br>

2. 인터럽트 처리 과정
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/e1ae37db-3ad0-4e5a-9243-f941b3ef778d" width="800px;" alt=""/>
<br><br>

- interrupt가 발생하면 커널이 개입해 프로세스를 중단시킨다(프로세서 뺏음).
- 이 떄, context saving을 사용해 프로세스 정보를 PCB에 저장한다.
- 인터럽트가 어디서, 왜 발생했는지 원인을 파악한다. ( interrupt handling )
- 인터럽트를 처리하기 위한 서비스 호출 ( Interrupt service )
- 처리 서비스가 끝나면, Ready Queue 상태에서 기다리고 있던 우선순위의 작업을 프로세서에 할당한다.
- 중단시켰었던 작업이 다시 프로세서에 할당될 때, 이전에 저장시킨 PCB 정보를 활용하여 복구 후 실행한다.
<br>

3. Context Switching ( 문맥 교환 )
  1) Context
     - 프로세스와 관련된 정보들의 집합
       - CPU regiter context : CPU 안의 관련 정보
       - Code & data, Stack, PCB : Memory 안의 관련 정보
  2) Context saving
     - 현재 프로세스의 Register context를 저장하는 작업
  3) Context restoring
     - Register context를 프로세스로 복구하는 작업
  4) Context switching ( Context saving + Context restoring )
     - 실행 중인 프로세스의 context를 저장하고 앞으로 실행 할 프로세스이 context를 복구하는 일
    
4. Context Switch Overhead
   - OS 마다 다르지만 성능에 큰 영향을 끼침
   - 스레드 사용 등으로 불필요한 Context switching을 줄일 수 있음
