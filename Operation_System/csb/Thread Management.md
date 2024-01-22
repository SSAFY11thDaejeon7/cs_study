# 스레드의 개념
### 스레드(Thread)
- Light Weight Process(LWP)
    - 프로세스는 원래 자원과 제어를 갖지만 스레드는 자원을 공유하고 제어만을 가지기 때문에 일반 프로세스보다 가벼움
- 프로세서(CPU) 활용의 기본 단위
- 구성요소
    - Thread ID
    - Register Set(PC, SP 등)
    - Stack
- 제어 요소 외 코드, 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유
- 전통적 프로세스 = 단일 스레드 프로세스

### Single-thread vs Multi-threads
- Single-thread
<img width="1026" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/62d35b8c-d434-46ab-9063-ee5e02283ba8">

- Multi-threads
<img width="1112" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/247a04bc-65d8-4fcc-8c55-f6c2fd1d1cbc">

### 스레드의 장점
- 사용자 응답성(Responsiveness)
    - 일부 스레드의 처리가 지연되어도, 다른 스레드는 작업을 계속 처리 가능
- 자원 공유(Resource sharing)
    - 자원을 공유해서 효율성 증가(커널의 개입을 피할 수 있음)
    - 예) 동일 address space에서 스레드 여러 개
- 경제성(Economy)
    - 프로세스의 생성, context switch에 비해 효율적
- 멀티 프로세서(multi-processor)활용
    - 병렬처리를 통해 성능 향상
 
### 스레드 사용의 예
<img width="676" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/c6d47624-0efb-4848-a94f-abbff9fc0f2e">

- FPS 게임을 하고 있을 때 화면 출력을 위한 모니터, 사용자 입력을 위한 키보드/마우스, 소리를 듣고 말하기 위한 스피커/마이크 등의 장치들을 활용하는 상황
    - 스레드가 하나만 있다면? → 다른 장치의 사용을 위해 원래 사용중이던 장치의 동작이 멈춤
    - 스레드가 여러개 있다면? → 자원의 공유로 장치들을 동시에 사용이 가능하고 일부 스레드의 처리가 지연되어도 다른 스레드의 동작은 진행됨
 
# 스레드의 구현
### 사용자 수준 스레드(User Thread)
- 사용자 영역의 스레드 라이브러리로 구현 됨
    - 스레드의 생성, 스케줄링 등
    - POSIX threads, Win32 threads, Java thread API 등
- 커널은 스레드의 존재를 모름
- 장점: 커널의 관리(개입)를 받지 않음
    - 생성 및 관리의 부하가 적음, 유연한 관리 가능
    - 이식성이 높음
- 단점: 커널은 프로세스 단위로 자원 할당
    - 하나의 스레드가 block 상태가 되면, 모든 스레드가 대기(single-threaded kernel의 경우)
<img width="1050" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/eebbcb8e-b52a-48fb-84dc-8ba470c50276">

### 커널 수준 스레드(Kernel Threads)
- OS(Kernel)가 직접 관리
- 단점: 커널 영역에서 스레드의 생성, 관리 수행
    - Context switching 등 부하(Overhead)가 큼
- 장점: 커널이 각 스레드를 개별적으로 관리
    - 프로세스 내 스레드들이 병행 수행 가능
        - 하나의 스레드가 block 상태가 되어도, 다른 스레드는 계속 작업 수행 가능
<img width="1010" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/0e660e6d-f2ff-47a2-9900-916d22de5f69">


### Multi-Threading Model
- 다대일(n:1) 모델: 사용자 수준 스레드
- 일대일(1:1) 모델: 커널 수준 스레드
- 다대다(n:m) 모델: 혼합형 스레드(사용자 수준 스레드 + 커널 수준 스레드)

### 혼합형 (n:m) 스레드
- n개 사용자 수준 스레드 - m개의 커널 스레드 (n ≥ m)
    - 사용자는 원하는 수만큼 스레드 사용
    - 커널 스레드는 자신에게 할당된 하나의 사용자 스레드가 block 상태가 되어도 다른 스레드 수행 가능 (병행 처리 가능)
- 효율적이면서도 유연함
<img width="946" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/d92f3e4c-a81c-455a-8f70-6995ce02a72f">
