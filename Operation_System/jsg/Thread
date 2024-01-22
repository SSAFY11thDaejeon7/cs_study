# 쓰레드(Thread)

#### 프로세스는 자원을 할당 받고, 그 자원을 제어한다. 그 중 "제어"가 쓰레드이다.
<img width="680" alt="스크린샷 2024-01-22 오후 9 24 41" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/ca29d18c-3e84-4c0c-a799-e17a51bc51b7">

<img width="621" alt="스크린샷 2024-01-22 오후 9 26 43" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/328befe0-a53d-44d3-bcd1-940a78d96a38">

- 쓰레드는 여러개가 존재하고 이를 제어하는 리소스(Resource)가 존재한다

<img width="634" alt="스크린샷 2024-01-22 오후 9 33 07" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/8bd6f846-2b14-4b72-b4c9-f7604b982a85">

- 각 쓰레드마다 자기만의 영역이 존재
- 자기 프로그램을 제어하는 코드 영역이 존재(PC)

- Light Weight Process(LWP) -> 프로세서 = 자원 + 제어, 쓰레드 = 제어(가볍다)
- 프로세서(CPU) 활용의 기본 단위
- 구성요소
  - Thread ID
  - Register set (PC, SP등)
  - Stack
  - 제어 요소 외 코드 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유
  - 전통적 프로세스 = 단일 스레드 프로세스
 
### 쓰레드의 장점
- 사용자 응답성(Responsiveness)
  - 일부 스레드의 처리가 지연되어도, 다른 스레드는 작업을 계속 처리 가능
- 자원공유 (Resource sharing)
  - 자원을 공유해서 효율성 증가(커널의 개입을 피할 수 있음)
    - 예)동일 address space에서 스레드 여러개
  - context switching 안해도 됨
- 경제성
  - 프로세스의 생성, context switch에 비해 효율적
- 멀티 프로세서 활용
  - 병렬처리를 통해 성능 향상
### 쓰레드 사용의 예
<img width="670" alt="스크린샷 2024-01-22 오후 9 44 14" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d3dc77b5-ebfc-47a2-b8b6-7e78d1824ab0">

- 3개의 쓰레드를 자원 공유하면서 게임을 진행할 수 있다.

## 쓰레드의 구현
- 사용자 수준 쓰레드(User thread)
- 커널 수준 쓰레드(Kernel thread)

### 사용자 수준 쓰레드 n:1
<img width="600" alt="스크린샷 2024-01-22 오후 9 51 10" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/7bc4ae66-b31e-4aa2-b9f4-c532dd6e3abf">

- 사용자 영역의 쓰레드 라이브러리로 구현 됨
  - 쓰레드의 생성, 스케줄링 등
  - POSIX threads, Win32 threads, Java thread API 등
- 커널은 쓰레드의 존재를 모름
  - 커널의 관리(개입)를 받지 않음(장점
    - 생성 및 관리의 부하가 적음, 유연한관리 가능
    - 이식성(portability)이 높음
  - 커널은 프로세스 단위로 자원 할당(단점)
    - 하나의 스레드가 block 상태가 되면 모든 스레드가 대기 (single-threaded kernel의 경우)

### 커널 수준 쓰레드 1:1
<img width="557" alt="스크린샷 2024-01-22 오후 9 51 48" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/e6a8c971-5aec-4ea2-859e-f9889b61cb7b">

- OS(kernel)가 직접 관리
- 커널 영역에서 쓰레드의 생성, 관리 수행
  - Context switching 등 부하(Overhead)가 큼 (단점)
- 커널이 각 쓰레드를 개별적으로 관리
  - 프로세스 내 스레드들이 병행 수행 가능 (장점)
    - 하나의 쓰레드가 block 상태가 되어도, 다릍ㄴ 쓰레드는 계속 작업 수행 가능
   
### Multi - Threading Model (혼합형 쓰레드) n:m
<img width="505" alt="스크린샷 2024-01-22 오후 9 55 43" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d16277d1-0d18-4c7b-9607-411b02bb8f6a">

- n개의 사용자 수준 쓰레드 - m개의 커널 쓰레드(n>m)
  - 사용자는 원하는 수만 큼 쓰레드 사용
  - 커널 쓰레드는 자신에게 할당된 하나의 사용자 쓰레드가 blcok 상태가 되어도, 다른 쓰레드 수행가능
    - 병렬 처리가능
- 효율적이면서도 유연함
