### Virutal Memory Management

- 가상 메모리(기억 장치)
    - Non-continuous allocation
        - 사용자 프로그램을 block으로 분할하여 적재/실행
    - Paging/Segmentation System
- 가상 메모리 관리 목적
    - 가상 메모리 시스템 성능 최적화
        - Cost model
        - 다양한 최적화 기법

### Cost Model for Virtaul Memory System

- Page fault frequency(발생 빈도)
- Page fault rate(발생률)
- Page fault rate를 최소화 할 수 있도록 전략들을 설계해야 함
    - Context switch 및 kernel 개입을 최소화
    - 시스템 성능 향상
- Page reference string(d)
    - 프로세스의 수행 중 참조한 페이지 번호 순서
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/f2a57fe8-a1dc-49bc-8614-319a127d41dc)
    - Page fault rate = F(w)    
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/bfda4523-3186-4c10-b4d5-505f9dc24911)

### Hardware Components

- Address translation device(주소 사상 장치)
    - 주소 사상을 효율적으로 수행하기 위해 사용
        - E.g., TLB(associated memories), Dedicated page-table register, Cache memories
- Bit Vectores
    - Page 사용 상황에 대한 정보를 기록하는 비트들
    - Reference bits(used bit)
        - 참조 비트
    - Update bits(modified bits, write bits, dirty bits)
        - 갱신 비트

### Bit Vectors
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/dc1ccfa6-a4f0-408b-b613-dc6836957ac4)

### Bit Vector - Reference Bit Vector

- 메모리에 적재된 각각의 page가 최근에 참조 되었는지를 표시
- 운영
    - 프로세스에 의해 참조되면 해당 page의 Refenece bit를 1로 설정
    - 주기적으로 모든 reference bit를 0으로 초기화
- Reference bit를 확인함으로서 최근에 참조된 page들을 확인 가능

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/86b16ee7-5dfc-43cc-bae2-84540c577138)

### Bit Vector - Update Bit Vector

- Page가 메모리에 적재된 후, 프로세스에 의해 수정 되었는지를 표시
    - Why? → swap device에 있는 페이지를 메모리에 올려놓으면 프로세서는 메모리 상에서 작업을 하고 변경이 발생할 때swap device ≠ 메모리이므로 변경이 되었다는 상태를 저장하여 작업이 끝난 후 swap device에 변경 사항을 적용함
- 주기적으로 초기화를 하지 않음
- Update bit = 1
    - 해당 page의 Main memory 상의 내용과 Swap device의 내용이 다름
    - 해당 page에 대한 write-back (to swap device)이 필요
    

### Software Components

- 가상 메모리 성능 향상을 위한 관리 기법들
    - Allocation Strategies (할당 기법)
    - Fetch Strategies (페치 기법)
    - Placement Strategies (배치 기법)
    - Replacement Strategies (교체 기법)
    - Cleaning Strategies (정리 기법)
    - Load Control Strategies (부하 조절 기법)

### Software Components - Allocation Strategies

- 각 프로세스에게 메모리를 얼마만큼 줄 것인가?
    - Fixed Allocation (고정 할당)
        - 프로세스의 실행동안 고정된 크기의 메모리 할당
    - Variable Allocation (가변 할당)
        - 프로세스의 실행동안 할당하는 메모리의 크기가 유동적
- 고려 사항
    - 프로세스 실행에 필요한 메모리 양을 예측해야 함
    - 너무 큰 메모리 할당 (Too much allocation)
        - 메모리가 낭비됨
    - 너무 적은 메모리 할당 (Too small allocation)
        - Page fault rate 높음
        - 시스템 성능 저하

### Software Components - Fetch Strategies

- 특정 page를 메모리에 언제 적재할 것인가?
    - Demand fetch (demand paging)
        - 프로세스가 참조하는 페이지들만 적재
        - Page fault overhead
    - Anticipatory fetch (pre-paging)
        - 참조될 가능이 높은 page를 예측
        - 가까운 미래에 참조될 가능성이 높은 page를 미리 적재
        - 예측 성공 시, page fault overhead가 없음
        - Prediction overhead (Kernel의 개입), Hit ratio에 민감함 → 잘못된 예측 시 자원 낭비가 큼
- 실제 대부분의 시스템은 Demand fetch 기법 사용
    - 일반적으로 준수한 성능을 보여줌

### Software Components - Placement Strategies

- Page / Segment 를 어디에 적재할 것인가?
- Paging SYstem에는 불필요ㅕ
- Segmentation System에서의 배치 기법
    - First-fit
    - Best-fit
    - Worst-fit
    - Next-fit

### Software Components - Replacement Strategies

- 새로운 page를 어떤 page와 교체 할 것인가? (빈 page frame이 없을 경우)
    - Fixed Allocation을 위한 교체 기법
    - Variable Allocation을 위한 교체 기법

### Software Components - Cleaning Strategies

- 변경된 page를 언제 write-back할 것인가?
    - 변경된 내용을 swap device에 반영
    - Demand cleaning
        - 해당 page에 메모리에서 내려올 때 write-back
    - Anticipatory cleaning(pre-cleaning)
        - 더 이상 변경될 가능성이 없다고 판단할 때, 미리 write-back
        - 예측 성공 → page 교체 시 발생하는 write-back 시간 절약
        - 예측 실패 → write-back 이후, page 내용이 수정된다면 overhead 발생
- 실제 대부분의 시스템은 Demand Cleaning 기법 사용
    - 일반적으로 준수한 성능을 보여주고 Anticipatory cleaning 기법 사용할 때, 잘못된 예측시 자원 낭비가 큼

### Software Components - Load Control Strategies

- 시스템의 multi-programming degree 조절
    - Allocation Strategies와 연계 됨
- 적정 수준의 multi-programming degree를 유지해야함
    - Plateau(고원) 영역을 유지
    - 저부하 상태 (Under-loaded)
        - 시스템 자원 낭비, 성능 저하
    - 고부하 상태 (Over-loaded)
        - 자원에 대한 경쟁 심화, 성능 저하
        - Thrashing(스레싱) 현상 발생
            - 과도한 page fault가 발생하는 현상

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/39b12bb8-fd0b-4bc1-83bf-00c3101e0eb2)


# Replacement Strategies - Fixed Allocation

### Min Algorithm (OPT Algorithm)

- 1966년 Belady에 의해 제시
- Minimize page fault frequency (proved)
    - Optimal solution
- 기법
    - 앞으로 가장 오랫동안 참조되지 않을 page 교체
    - Tie-breaking rule : page 번호가 가장 큰 / 작은 페이지 교체
- 실현 불가능한 기법(Unrealizable)
    - Page reference string을 미리 알고 있어야 함
    - 교체 기법의 성능 평가 도구로 사용됨
- Example

<img width="1084" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/88bb53a2-0cab-462f-9e63-a9b5fc2d149a">

### Random Algorithm

- 무작위로 교체할 page 선택
- Low overhead
- No policy

### FIFO Algorithm

- First In First Out
    - 가장 오래된 page를 교체
- Page가 적재 된 시간을 기억하고 있어야 함
- 자주 사용되는 page가 교체 될 가능성이 높음
    - Locality에 대한 고려가 없음
- FIFO anomaly (Belady’s anomaly)
    - FIFO 알고리즘의 경우, 더 많은 page frame을 할당 받음에도 불구하고 page fault의 수가 증가하는 경우가 있음 → Locality에 대해 고려하지 않았기 때문
- Example

<img width="967" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/53f3eccd-3673-43a2-805e-14ee9e8c0458">

### LRU (Least Recently Used) Algorithm

- 가장 오랫동안 참조되지 않은 page를 교체
- page 참조 시마다 시간을 기록해야 함
- Locality에 기반을 둔 교체 기법
- MIN Algorithm에 근접한 성능을 보여줌
- 실제로 가장 많이 활용되는 기법
- 단점
    - 참조 시마다 시간을 기록해야 하므로 Overhead가 발생
        - 간소화된 정보 수집으로 해소 가능 (정확한 시간 대신에 순서만 기록)
    - Loop 실행에 필요한 크기보다 작은 수의 page frame이 할당 된 경우, page fault수가 급격히 증가함
        - Allocation기법에서 해결 해야 함
- Example

<img width="992" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/515b1c3a-8f16-445e-8171-10e9ef8d75a7">

### LFU (Least Frequently Used) Algorithm

- 가장 참조 횟수가 적은 page를 교체
    - Tie-breaking rule: LRU
- Page 참조 시마다, 참조 횟수를 누적 시켜야 함
- Locality 활용
    - LRU 대비 적은 overhead
- 단점
    - 최근 적재된 참조될 가능성이 높은 page가 교체 될 가능성이 있음
    - 참조 횟수 누적 overhead 발생
- Example

<img width="918" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/03a810f6-d38a-4523-8099-a2a074697859">

### NUR (Not Used Recently) Algorithm

- LRU approximation scheme
    - LRU보다 적은 overhead로 비슷한 성능 달성 목적
- Bit Vector 사용
    - Reference Bit Vector(r), Update Bit Vector(m)
- 교체 순서
    1. (r, m) = (0, 0) (일정 시간 참조되지 않고, 수정되지 않음)
    2. (r, m) = (0, 1) (일정 시간 참조되지 않고, 수정 되었음)
    3. (r, m) = (1, 0) (일정 시간 참조되고 수정되지 않음)
    4. (r, m) = (1, 1) (일정 시간 참조되고 수정 되었음)

### Clock Alogrithm

- IBM VM/370 OS
- Reference Bit 사용함
    - 주기적인 초기화 없음
- Page frame들을 순차적으로 가리키는 pointer(시계바늘)를 사용하여 교체될 page를 결정
- Pointer를 돌리면서 교체 Page 결정
    - 현재 가리키고 있는 Page의 Reference Bit(r) 확인
    - r = 0 인 경우, 교체 Page로 결정
    - r = 1 인 경우, Reference Bit 초기화 후 Pointer 이동
- 먼저 적재된 Page가 교체될 가능성이 높음
    - FIFO와 유사
- Reference Bit를 사용하여 교체 페이지 결정
    - LRU(or NUR)과 유사
- Example

<img width="1229" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/028a8e1f-9b04-4eb4-82fa-f4188ab372c7">

### Second Chance Algorithm

- Clock Algorithm과 유사
- Update Bit(m)도 함께 고려 함
    - 현재 가리키고 있는 page의 (r, m) 확인
    - (0, 0) → 교체 Page로 결정
    - (0, 1) → (0, 0), write-back(cleaning) list에 추가 후 이동
    - (1, 0) → (0, 0) 후 이동
    - (1, 1) → (0, 1) 후 이동
- Example

<img width="1211" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/e7aa641e-2dd1-4fd8-9b2a-fa804d01ab5d">

### Other Algorithms

- Additional-reference-bits algorithm
    - LRU approximation
    - 여러 개의 reference bit를 가짐
        - 각 time-interval에 대한 참조 여부 기록
        - History register for each page
- MRU (Most Recently Used) Algorithm
    - LRU와 정반대 기법
- MFU (Most Frequently Used) Algorithm
    - LFU와 정반대 기법
