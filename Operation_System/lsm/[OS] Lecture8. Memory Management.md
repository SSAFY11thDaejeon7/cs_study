# 1. background

### 메모리(기억장치)의 종류

- 위로 갈수록 속도가 빠르다.
- 용량은 위로 올라갈수록 작아진다.
- I/O Bottleneck
- 캐시 & 래지스터 - HW(CPU)가 관리
- 메인메모리 & 보조기억장치 - SW가 관리
<img width="504" alt="Memory" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/beb9a2d7-7aac-4057-bb55-18f6b8fd9f1b">


### 메모리(기억장치) 계층 구조

<img width="492" alt="MemoryConstructure" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/13985036-723a-4696-9484-6c64929ec249">

- **Block**
    - 보조기억장치와 주기억장치 사이의 데이터 전송 단위
    - Size: 1 ~ 4KB
- **Word**
    - 주기억장치와 레지스터 사이의 데이터 전송 단위
    - Size: 16 ~ 64 bits
    - 컴퓨터 성능 지표

### Address Binding

- 프로그램의 논리 주소를 실제 메모리의 물리 주소로 매핑하는 작업
- Binding 시점에 따른 구분
    - **Compile time binding**
        - 프로세스가 메모리에 적재될 위치를 컴파일러가 알 수 있는 경우
        - 프로그램 전체가 메모리에 올라가야 함
    - **Load time binding**
        - 메모리 적재 위치를 컴파일 시점에서 모르면, 대체 가능한 상대 주소를 생성한다.
        - 적재 시점(load time)에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설정한다.
        - 프로그램 전체가 메모리에 올라가야 함.
    - **Run time binding**
        - 실행 시간에 주소를 할당
        - Address binding을 수행시간까지 연기
        - 프로세스가 수행 도중 다른 메모리 위치로 이동할 수 있음
        - 하드웨어의 도움이 필요 (MMU)
        - 대부분의 OS가 사용한다.
        - (Ready → Running 일 때)
<img width="540" alt="UserProgramProcessingSteps" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/8aa3f6e6-ce52-4cd3-ac95-928ade314a1e">


### Dynamic Loading

- 모든 루틴을 교체 가능한 형태로 디스크에 저장
- 실제 호출 전까지는 루틴을 적재하지 않는다.
- 메인 프로그램만 메모리에 적재하여 수행한다.
- 루틴의 호출 시점에 address binding을 수행한다.
- 메모리 공간의 효율적인 사용이 가능하다는 장점이 있다.

### Swapping

(Swap-out 메모리를 빼앗기는 것)

- 프로세서 할당이 끝나고 수행 완료 된 프로세스는 swap-device로 보내고 (Swap-out)
- 새롭게 시작하는 프로세스는 메모리에 적재한다. (Swap-in)
<img width="519" alt="Swapping" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/9d643c91-5d69-44c2-a4b7-c5cbeb59569b">


# 2. Memory Allocation

### 1) 연속할당 (Continuous Memory Allocation)

- 프로세스를 하나의 연속된 메모리 공간에 할당하는 정책 (프로그램, 데이터, 스택 등)
- 메모리 구성 정책
    - 메모리에 동시에 올라갈 수 있는 프로세스 수
    - 각 프로세스에게 할당되는 메모리 공간 크기
    - 메모리 분할 방법

(1) Uni-Programming

- Multiprogramming degree가 1인 경우
- 하나의 프로세스만 메모리 상에 존재하는 경우
- 가장 간단한 메모리 관리 기법
- 문제점
    - 메모리 크기보다 프로그램의 크기가 더 크다는 단점이 존재한다.
        - Overlay structure로 해결: 메모리에 현재 필요한 영역만 적재한다.
    - 커널을 보호해야 된다 → 경계 레지스터 사용으로 해결
    - Low System Resource Utilization & Low System Performance → Multi-programming으로 해결 (시스템에 여러 개의 프로그램을 올린다.)

(2) Muti-Programming

- Fixed Partition Multiprogramming
    - 메모리 공간을 고정된 크기로 분할한다. (미리 분할되어 있음)
    - 각 프로세스는 하나의 partition에 적재된다
    - Partition의 수는 K ( = MultiProgramming Degree)
    - 메모리 관리가 간편하지만 Internal/ External fragmentation이 발생
    - 자료구조의 예
    <img width="584" alt="Multiprogramming1" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/07d6d4ac-e0dd-46ae-8043-1c870afd409d">

        
    - 커널 및 사용자 영역 보호 (커널 침범하는 경우 방어)
    <img width="293" alt="Multiprogramming2" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/97c90c2c-8b5d-453f-a575-f94dba504b13">

    

### Fragmentation (단편화)

- Internal Fragmentation: 내부 단편화
    - Partition의 크기 > Process의 크기 (메모리가 낭비됨)
- External Fragmentation: 외부 단편화
    - 남은 메모리 크기 > Process의 크기지만 연속된 공간이 아니라서 메모리가 낭비됨

### Variable Partition Multiprogramming

- 초기에는 전체가 하나의 영역이다
- 프로세스를 처리하는 과정에서 메모리 공간이 동적으로 분할된다.
- No internal Fragmentation (필요한 만큼 사용하므로)

### 배치 전략

- First Fit(최초 적합)
    - 충분한 크기를 가진 첫 번째 partition을 선택
    - Simple and low overhead
    - 공간 활용률이 떨어질 수 있음
- Best Fit (최적 적합)
    - Process가 들어갈 수 있는 partition 중 가장 작은 곳을 선택한다.
    - 탐색 시간이 오래 걸림 (모든 partition을 살펴봐야 한다.)
    - 크기가 큰 partition을 유지할 수 있음
    - 작은 크기의 partition이 많이 발생 (활용하기 너무 작은) → 들어갈 수 있는 곳 중 가장 큰 곳 (worstfit으로 해결)
- Worst Fit (최악 적합)
    - Process가 들어갈 수 있는 partition 중 가장 큰 곳 선택
    - 탐색 시간이 오래 걸림
    - 작은 크기의 partition 발생을 줄일 수 있음
    - 큰 크기의 partition 확보가 어려움
- Next Fit(순차 최초 적합)
    - 최초 적합 전략과 유사
    - State table에서 마지막으로 탐색한 위치부터 탐색
    - 메모리 영역의 사용 빈도 균등화
    - Low overhead
![VariableParititionary](https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/164fbd23-c624-4042-a741-2841f1436050)


### External Fragmentation Issue 해결 방안

- Coalescing holes (공간 통합)
    - 인접한 빈 영역을 하나의 partition으로 통합
        - Process가 memory를 release하고 나가면 수행
        - Low overhead
- Storage Compaction (메모리 압축)
    - 모든 빈 공간을 하나로 통합
    - 프로세스 처리에 필요한 적재 공간 확보가 필요할 때 수행한다.
    - High overhead (모든 Process 재배치, 많은 시스템 자원을 소비)
    - 자주 수행하지는 않고 일정 오랜 기간에 한번씩 수행한다.
