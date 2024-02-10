### 메모리의 종류

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/46bb7e9b-b547-44a2-9b88-6e5591d60f75)

### 메모리(기억장치) 계층구조

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/84febd3f-faf3-49c5-955e-e0c0720d4a4e)
- Block
    - 보조기억장치와 주기억장치 사이의 데이터 전송 단위
    - Size: 1 ~ 4KB
- Word
    - 주기억장치와 레지스터 사이의 데이터 전송 단위
    - Size: 16 ~ 64bits
 
### Address Binding

- 프로그램의 논리 주소를 실제 메모리의 물리 주소로 매핑(mapping)하는 작업
- Binding 시점에 따른 구분
    - Compile time binding
    - Load time binding
    - Run time binding
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/20029121-3a57-4b63-804b-c43fe5f416a4)

### Address Binding - Compile time binding

- 프로세스가 메모리에 적재될 위치를 컴파일러가 알 수 있는 경우
    - 위치가 변하지 않음
- 프로그램 전체가 메모리에 올라가야 ㅎㅎ함

### Address Binding - Load time binding

- 메모리 적재 위치를 컴파일 시점에서 모르면, 대체 가능한 상대 주소를 생성
- 적재 시점에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설ㅈ정
- 프로그램 전체가 메모리에 올라가야 함

### Address Binding - Run time binding

- Address Binding 을 수행시간까지 연기
    - 프로세스가 수행 도중 다른 메모리 위치로 이동할 수 있음
- HW의 도움이 필요
    - MMU: Memory Management Unit
- 대부분의 OS가 사용

### Dynamic Loading

- 모든 루틴을 교체 가능한 형태로 디스크에 저장
- 실제 호출 전까지는 루틴을 적재하지 않음
    - 메인 프로그램만 메모리에 적재하여 수행
    - 루틴의 호출 시점에 address binding 수행
- 장점
    - 필요한 것만 메모리에 적재 → 메모리 공간의 효율적 사용

### Swapping

- 프로세서 할당이 끝나고 수행 완료 된 프로세스는 swap-device로 보냄(swap-out)
- 새롭게 시작하는 프로세스는 메모리에 적재(swap-in)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/8ee0ba06-6a5f-4a21-9d7d-343c518c8ad4)

# Memory Allocation

### Continuous Memory Allocation

- 프로세스(context)를 하나의 연속된 메모리 공간에 할당하는 정책
    - 프로그램, 데이터, 스택 등
- 메모리 구성 정책
    - 메모리에 올라갈 수 있는 프로세스 수
    - 각 프로세스에게 할당되는 메모리 공간 크기
    - 메모리 분할 방법

### Continuous Memory Allocation - Uni Programming

- 하나의 프로세스만 메모리 상에 존재
- 가장 간단한 메모리 관리 기법
- 문제점
    - 프로그램의 크기 > 메모리 크기
        - Overlay structure 사용으로 메모리에 현재 필요한 영역만 적재한다. 사용자가 프로그램의 흐름 및 자료구조를 모두 알고 있어야 함
    - 커널의 보호
        - 경계레지스터 사용
    - Low System Resource Utilization & Low System Performance
        - Multi Programming 사용
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/bb9c3076-245c-4981-8c31-5e48013da5b3)

### Continuous Memory Allocation - Multi Programming (Fixed Partition Multi Programming)

- 메모리 공간을 고정된 크기로 분할
    - 미리 분할되어 있음
- 각 프로세스는 하나의 partition(분할)에 적재
    - Process : Partition = 1 : 1
- Partition의 수 = K
    - Multi Programming degree = K
- Boundary Register 사용으로 커널 및 사용자 영역 보호
- Internal fragmentation
    - 내부 단편화
    - Partition 크기 > Process 크기
        - 메모리가 낭비 됨
- External fragmentation
    - 외부 단편화
    - 남은 메모리 크기 > Process 크기, 하지만 연속된 공간이 아님
        - 메모리가 낭비 됨
- 요약
    - 고정된 크기로 메모리를 미리 분할
    - 메모리 관리가 간편하여 오버헤드가 적음
    - Internal / External fragmentation으로 시스템 자원이 낭비 될 수 있음

### Continuous Memory Allocation - Multi Programming (V ariablePartition Multi Programming)

- 초기에는 전체가 하나의 영역
- 프로세스를 처리하는 과정에서 메모리 공간이 동적으로 분할
- No Internal fragmentation

### 배치 전략 (Placement Strategies)

- First-fit(최초 적합)
    - 충분한 크기를 가진 첫 번째 partition을 선택
    - Simple and low overhead
    - 공간 활용률이 떨어질 수 있음
- Best-fit(최적 적합)
    - Process가 들어갈 수 있는 partition 중 가장 작은 곳 선택
    - 탐색시간이 오래 걸림
        - 모든 partition을 살펴봐야 함
    - 크기가 큰 partition을 유지 할 수 있음
    - 작은 크기의 partition이 많이 발생
- Worst-fit(최악 적합)
    - Process가 들어갈 수 있는 partition 중 가장 큰 곳 선택
    - 탐색시간이 오래 걸림
        - 모든 partition을 살펴 봐야 함
    - 작은 크기의 partition 발생을 줄일 수 있음
    - 큰 크기의 partition 확보가 어려움
- Next-fit(순차 최초 적합)
    - 최초 적합 전략과 유사
    - State table에서 마지막으로 탐색한 위치부터 탐색
    - 메모리 영역의 사용 빈도 균등화
    - Low overhead

### 외부 단편화 문제

- Coalescing holes (공간 통합)
    - 인접한 빈 영역을 하나의 partition으로 통합
        - Process가 Memory를 release하고 나가면 수행
        - Low overhead
- Storage Compaction (메모리 압축)
    - 모든 빈 공간을 하나로 통합
    - 프로세스 처리에 필요한 적재 공간 확보가 필요할 때 수행
    - High overhead
        - 모든 Process 재배치 (Process 중지)
        - 많은 시스템 자원을 소비
