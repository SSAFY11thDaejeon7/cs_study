# [OS] Lecture 3. Process Management

## Job vs Process
* **작업 / 프로그램**
    * 실행할 **프로그램** + **데이터**
    * 컴퓨터 시스템에 실행 요청 전의 상태
* **프로세스**
    * 실행을 위해 시스템(커널)에 등록된 작업
    * 시스템 성능 향상을 위해 커널에 의해 관리됨
<img width="709" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/bfb0444e-6bdd-4e62-a4c2-e12d24ce15bc">

<img width="754" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/9db42394-4496-41b6-b0da-ae2337181a36">


## 프로세스의 정의
* **실행중인 프로그램**
  * 커널에 등록되고 커널의 관리하에 있는 작업
  * 각종 자원들을 요청하고 할당 받을 수 있는 개체
  * 프로세스 관리 블록(PCB)을 할당 받은 개체
  * 능동적인 개체(active entity)
    * 실행 중에 각종 자원을 요구, 할당, 반납하며 진행
* **Process Control Block**
  * 커널 공간 내에 존재
  * 각 프로세스 들에 대한 정보를 관리

## 프로세스의 종류
* 역할로 구분
  * 시스템(커널) 프로세스
    * 모든 시스템 메모리와 프로세서의 명령에 액세스할 수 있는 프로세스
    * 프로세스 실행 순서를 제어하거나 다른 사용자 및 커널(운영체제) 영역을 침범하지 못하게 감시
    * 사용자 프로세스를 생성하는 프로세스이다.
  * 사용자 프로세스
    * 사용자 코드를 수행하는 프로세스이다.
* 병행 수행 방법으로 구분
  * 독립 프로세스
    * 다른 프로세스에 영향을 주지 않거나 다른 프로세스의 영향을 받지 않으면서 수행하는 병행 프로세스이다.
  * 협력 프로세스
    * 다른 프로세스에 영향을 주거나 다른 프로세스에서 영향을 받는 병행 프로세스이다.

## 자원(Resource)의 개념
* **커널의 관리 하에 프로세스에게 할당/반납 되는 수동적 개체(passive entity)**
* **자원의 분류**
  * H/W resources
    * Processor, memory, disk, monitor, keyboard, Etc.
  * S/W resources
    * Message, signal, files, installed SWs, Etc.

## Process Control Block (PCB)
* **OS가 프로세스 관리에 필요한 정보 저장**
* 메모리에 프로세스가 생성되면 커널에 PCB가 생성되었다는 것
  * 각 프로세스들에 대한 상태정보 저장
<img width="587" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/0d472e51-9bd5-4e20-9e3a-7d964926a79f">

## PCB가 관리하는 정보
* **PID: Process Identification Number**
  * 프로세스 고유 식별 번호
* **스케줄링 정보**
  * 프로세스 우선순위 등과 같은 스케줄링 관련 정보들
* **프로세스 상태**
  * 자원 할당, 요청 정보 등
* **메모리 관리 정보**
  * Page table, segment table 등
* **입출력 상태 정보**
  * 할당 받은 입출력 장치, 파일 등에 대한 정보 등
* **문맥 저장 영역 (context save area)**
  * 프로세스의 레지스터 상태를 저장하는 공간 등
* **계정 정보**
  * 자원 사용 시간 등을 관리
* *PCB 정보는 OS별로 서로 다름*
* *PCB 참조 및 갱신 속도는 OS의 성능을 결정 짓는 중요한 요소 중 하나*

## 프로세스의 상태 (Process Status)
* 프로세스-자원 간의 상호작용에 의해 결정
* 프로세스 상태 및 특성
<img width="677" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/ca7e462f-c99e-47ac-ae21-8a2b213091bb">

## Process State Transition Diagram
<img width="750" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/f03ce062-d2cf-4be8-8eef-8d47b7309266">

### Created State
* 작업을 커널에 등록
* PCB 할당 및 프로세스 생성
* 커널
  * 가용 메모리 공간 체크 및 프로세스 상태 전이
    * Job이 Submit 되어 created 상태가 되면
      * Memory allocated 되어 ready 상태가 되거나
      * Waiting memory allocation이 되면 suspended ready가 된다.
<img width="466" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/435b22a1-4a6b-4f1c-bdf5-733d11d7b011">


### Ready State
* 프로세서 외에 다른 모든 자원을 할당 받은 상태
  * 프로세서 할당 대기 상태
  * **즉시 실행 가능 상태**
* Dispatch (or Schedule)
  * Processor allocated 되는 것을 dispatch, schedule이라 함
  * running 상태가 된다.
<img width="294" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/1d535d40-fb7b-4c91-ad0a-5cc92fe9f867">

### Running State
* 프로세서와 필요한 자원을 모두 할당 받은 상태
* Preemption
  * running -> ready
  * 프로세서 스케줄링 (e.g, time-out, priority changes)
* Block/sleep
  * running -> asleep
  * I/O 등 자원 할당 요청
<img width="418" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e1f3da09-81e3-4091-a23c-1c95c37a8ee7">

### Blocked/Asleep State
* 프로세서 외에 다른 자원을 기다리는 상태
  * 자원 할당은 System call에 의해 이루어짐
* Wake-up
  * asleep -> ready
<img width="448" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/56cae345-89ab-4d50-b764-3fdd1b080fd5">

### Suspended State
* 메모리를 할당 받지 못한(빼앗긴) 상태
  * Memory image를 swap device에 보관
    * swap device: 프로그램 정보 저장을 위한 특별한 파일 시스템
  * 커널 또는 사용자에 의해 발생
* Swap-out(suspended), Swap-in(resume)
  * create -> suspended ready
    * 메모리가 없을 때
  * asleep -> suspended blocked
    * I/O 대기 중, 메모리를 뺏겼을 때
    * 다시 할당했을 때, 지금까지의 진행상황(메모리)을 기억할 별도의 이미지가 필요함.
      * swap divice에 저장
<img width="622" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e5d8de08-ee89-4ea7-beed-d90affaa79ad">

### Terminated/Zombie State
* 프로세스 수행이 끝난 상태
* 모든 자원 반납 후,
* 커널 내에 일부 PCB 정보만 남아 있는 상태
  * 이후 프로세스 관리를 위해 정보 수집
  * 정보 수집이 끝나면 이후 완전 삭제, 소멸
<img width="418" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/2e51df99-38ae-4c52-aaa6-7db02750e14e">

## 프로세스 관리를 위한 자료구조
* Ready Queue
* I/O Queue
* Device Queue
  * asleep 상태에서는 자원별로 queue가 따로 존재 (blocked lists)
  * ready 상태에서는 스케줄러에 의해 선택됨 (ready list)
<img width="606" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/170138e6-970d-42eb-a615-44a5266e6de5">

## 인터럽트(Interrupt)
* 예상치 못한, 외부에서 발생한 이벤트
  * Unexpected, external events
* 인터럽트의 종류
  * I/O, Clock, Console, Program check, Machine check, Inter-process, System call interrupt

## 인터럽트 처리 과정
* 최초 interrupt 발생
* 커널 개입 -> 프로세스 중단 
  * context saving 발생
    * 작업을 어디까지 진행했는지 PCB에 저장
* 인터럽트 처리(Interrupt handling)
  * 인터럽트 핸들러(handler)가 인터럽트 발생 장소, 원인 파악
  * 인터럽트 서비스 할 것인지 결정
* 인터럽트 서비스(Interrupt service)
  * 인터럽트 서비스 루틴(routine) 호출
  * 인터럽트 서비스를 프로세서에 넣어줌
* 서비스가 끝나면 ready 상태였던 프로세스를 넣어줌 (중단됐던 프로세스라는 보장 없음)
<img width="768" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/2b121dd0-a4a8-4cfb-b857-40458017fcc2">
<img width="706" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/eb9fda87-54a2-4200-90d0-3e827e1b9aec">


## Context Switching (문맥 교환)
* Context
  * 프로세스와 관련된 정보들의 집합
    * CPU register context -> in CPU
    * Code & data, Stack, PCB -> in memory
* Context saving
  * 현재 프로세스의 Register context를 저장하는 작업
* Context restoring
  * Register context를 프로세스로 복구하는 작업
* Context switching(~= Process switching)
  * 실행 중인 프로세스의 context를 저장하고, 앞으로 실행할 프로세스의 context를 복구하는 일
    * 커널의 개입으로 이루어짐

## Context Switch Overhead
* Context switching에 소요되는 비용
  * OS마다 다름
  * OS의 성능에 큰 영향을 줌
* 불필요한 Context switching을 줄이는 것이 중요
  * 예, 스레드(thread) 사용 등
