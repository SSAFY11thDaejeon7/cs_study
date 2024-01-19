- 작업 (Job) / 프로그램
  - 실행 할 프로그램 + 데이터
  - 컴퓨터 시스템에 실행 요청 전의 상태
- 프로세스
  - 실행을 위해 시스템(커널)에 등록된 작업
  - 시스템 성능 향상을 위해 커널에 의해 관리됨
<br>
 
# 프로세스의 정의
- 실행중인 프로그램
  - 커널에 등록되고 커널의 관리하에 있는 작업
  - 각종 자원들을 요청하고 할당받을 수 있는 개체
  - 프로세스 관리 블록(PCB)을 할당 받은 객체
  - 능동적인 객체
    - 실행 중에 각종 자원을 요구, 할당, 반납하며 진행
<br>

# 프로세스의 종류

## 역할에 따른 구분
- 시스템(커널) 프로세스
  - 모든 시스템 메모리와 프로세서의 명령에 액세스할 수 있는 프로세스
  - 프로세스 실행 순서를 제어하거나 다른 사용자 및 커널(운영체제) 영역을 침범하지 못하게 감시하고, 사용자 프로세스를 생성하는 기능을 함
- 사용자 프로세스
  - 사용자 코드를 수행하는 프로세스

## 병행 수행 방법
- 독립 프로세스
  - 다른 프로세스에 영향을 주지 않거나 다른 프로세스의 영향을 받지 않으면서 수행하는 병행 프로세스이다.
- 협력 프로세스
  - 다른 프로세스에 영향을 주거나 다른 프로세스에서 영향을 받는 병행 프로세스이다.
<br>

# 자원(Resource)의 개념
> 커널의 관리 하에 프로세스에게 할당/반납되는 수동적 객체(passive entity)

- 자원의 분류
  - H/W resources
  - S/W resources
 
## Prcoess Controll Block (PCB)
- OS가 프로세스 관리에 필요한 정보 저장
- 프로세스 생성 시, 생성됨
- 커널이 관리하는 영역에 저장됨

### PCB가 관리하는 정보
- PID: 프로세스 고유 식별 번호
- 스케줄링 정보: 프로세스 우선순위 등과 같은 스케줄링 관련 정보들
- 프로세스 상태
  - 자원 할당, 요청 정보 등
- 메모리 관리 정보
  - Page table, segment table 등
- 입출력 상태 정보
  - 할당받은 입출력 장치, 파일 등에 대한 정보등
- 문맥 저장 영역 (context save area)
  - 프로세스의 레지스터 상태를 저장하는 공간등
- 계정 정보
  -자원 사용 시간 등을 관리
<br>

# 프로세스의 상태
- 프로세스 - 자원 간의 상호작용에 의해 결정
- 프로세스 상태 및 특성
<br>

![3일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/37d4c0c5-b680-4d39-a560-5a57fc467086)

![3일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/9334ec9e-4586-433d-84e5-7a50f4d7c05f)

## Created State
- 작업(Job)을 커널에 등록
- PCB 할당 및 프로세스 생성
- 커널
  - 가용 메모리 공간 체크 및 프로세스 상태 전이
- 내가 쓸 수 있는 공간이 있는지에 따라 이후 상태가 변화함
- 
## Ready State
> 프로세서 외에 다른 모든 자원을 할당 받은 상태
- 프로세서 할당 대기 상태
- 즉시 실행 가능 상태
- Dispatch (or Schedule)
  - CPU를 할당받은 경우
  - Ready state -> reunning state

## Running State
> 프로세서와 필요한 자원을 모두 할당 받은 상태

- Preemption
  - 프로세서를 뺏겨서 쫓겨나는 상황
  - Running state -> ready states
  -  프로세서 스케줄링
- Block/sleep
  - 작업을 잠자면서 I/O가 끝나기를 대기하는 상황
  - Running state -> asleep state
  - I/O 등 자원 할당 요청

## Blcoked / Asleep State
> 프로세서 외에 다른 자원을 기다리는 상태

- 자원 할당은 System call에 의해 이루어짐
- Wake-up
  - Asleep state -> ready state
 
## Suspended Stae
> 메모리를 할당 받지 못한(빼앗긴) 상태

- 메모리가 없는 경우: suspended ready
- 메모리를 뺏긴 상태: suspended blocked
- Memory image(메모리의 상태를 기록해놓은것)를 swap device에 보관
  - Swap-device: 프로그램 정보 저장을 위한 특별한 파일 시스템
- 커널 또는 사용자에 의해 발생
- Swap-out: 메모리를 뺏기는 것
- Swap-in: 메모리를 할당받아 복구하는 것

## Terminated/Zombie State
> 프로세스 수행이 끝난 상태

- 모든 자원을 반납 후, 커널 내에 일부 PCB 정보만 남아 있는 상태
  - 이후 프로세스 관리를 위해 정보 수집

<br>
# 인터럽트(Interrupt)
> 예상치 못한, 외부에서 발생한 이벤트

- 인터럽트의 종류
  - I/O interrupt
  - Clock interrupt
  - Console interrupt
  - Program check interrupt
  - Machine check interrupt
  - Inter-process interrupt
  - System call interrupt

## 인터럽트 처리 과정
![3일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/a4c362b3-1af1-4409-889c-95d8bfcf8b7b)

## Context Switching (문맥 교환)
- Context
  - 프로세스와 관련된 정보들의 집합
- Context saving
  - 현재 프로세스의 Register context를 저장하는 작업
- Context restoring
  - Register context를 프로세스로 복구하는 작업
- Context switching 
  - 실행 중인 프로세스의 context를 저장하고, 앞으로 실행할 프로세스의 context를 복구하는 일
 
### Context Switch Overhead
> Context switching에 소요되는 비용
- OS마다 다름
- OS의 성능에 큰 영향을 줌
- 불필요한 Context switching을 줄이는 것이 중요
  - 예) 쓰레드 사용 등
