# 1. HW components

### 가상 메모리 (기억장치)

- **Non-continuous allocation**: 사용자 프로그램을 **block**으로 분할하여 적재/실행
- Paging/Segmentation system

### 가상 메모리 관리의 목적

- **가상 메모리 시스템 성능 최적화**
- Cost model, 다양한 최적화 기법

### Cost Model for Virtual Meomory System

- Page fault
    - 전체 프로그램의 메모리는 Swap device에 저장되어 있고, 필요한 메모리만 Memory에 올려놓는데, CPU가 참조해야 하는 프로세스가 메모리에 없을 경우 발생!
- Page fault frequency (발생 빈도)
- Page fault rate (발생률)
- Page fault rate를 최소화 할 수 있도록 전략들을 설계해야 함
    - Context switch 및 Kernel 개입을 최소화 해야함 
    (Running → asleep → ready)
    - 시스템 성능을 향상 시킴

### 용어 정리
<img width="464" alt="pagereferencestring" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/4a5ccd1e-90f3-47ac-bff6-9d0634a93ce3">


### Hardward Components

- Address translation device (주소 사상 장치)
    - 주소 사상을 효율적으로 수행하기 위해 사용한다.
- Bit Vectors
    - Page 사용 상황에 대한 정보를 기록하는 비트들
    - Reference bits (used bit): 참조 비트
    - Update bits (modified bits, write bits, dirty bits): 갱신 비트
<img width="458" alt="bitvectors" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/b024a4b7-a761-4604-8135-6625eb7e83e5">


- Reference bit vector
    - 메모리에 적재된 각각의 page가 최근에 참조 되었는지를 표시
    - 운영
        1. 프로세스에 의해 참조되면 해당 page의 Reference bit를 1로 설정
        2. 주기적으로 모든 reference bit를 0으로 초기화
    - Reference bit를 확인함으로서 최근에 참조된 page 들을 확인 가능
 <img width="461" alt="referencebitvector" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/e677e10e-c28b-4d0e-abe1-a010824902a8">
   
    
- Update bit vector
    - Page가 메모리에 적재된 후, 프로세스에 의해 수정 되었는지를 표시
    - 주기적 초기화 없음
    - Update bit = 1
        - 해당 page의 (Main memory 상 내용 ≠ swap device의 내용)
        - 데이터가 바뀌었음을 표시한다.

# 2. SW components

### Software Components

- 가상 메모리 성능 향상을 위한 관리 기법들
    - Allocation Strategies (할당 기법)
    - Fetch Strategies
    - Placement strategies (배치 기법)
    - Replacement strategies (교체 기법)
    - Cleaning strategies(정리 기법)
    - Load control strategies (부하 조절 기법)

### Allocation Strategies

- 각 프로세스에게 메모리를 얼마 만큼 줄 것인가?
    - Fixed allocation (고정 할당): 프로세스의 실행 동안 고정된 크기의 메모리 할당
    - Variable allocation (가변 할당): 프로세스의 실행 동안 할당하는 메모리의 크기가 유동적
- 고려 사항
    - 프로세스 실행에 필요한 메모리의 양을 예측해야 함
    - 너무 큰 메모리 할당: 메모리가 낭비됨
    - 너무 적은 메모리 할당: Page Fault rate 상승, 시스템 성능 저하

### Fetch Strategies

- 특정 Page를 메모리에 언제 적재할 것인가?
    - Demand fetch (demand paging)
        - 프로세스가 참조하는 페이지들만 적재
        - Page fault overhead
    - Anticipatory fetch (pre-paging)
        - 참조될 가능이 높은 page 예측
        - 가까운 미래에 참조될 가능성이 높은 page를 미리 적재
        - 예측 성공 시, page fault overhead가 없음
        - Prediction overhead(Kernel의 개입), Hit ratio에 민감함
- 실제 대부분의 시스템은 Demand fetch 기법 사용
    - 일반적으로 준수한 성능을 보여 줌
    - Anticipatory fetch: Predictino overhead 잘못된 예측 시 자원 낭비가 큼

### Placement Strategies

- Page/segment를 어디에 적재할 것인가?
- Paging System에는 불필요
- Segmentation system에서의 배치 기법
    - First-fit
    - Best-fit
    - Worst-fit
    - Next-fit

### Replacement Strategies

- 새로운 page를 어떤 page와 교체 할 것인가? (빈 page frame이 없는 경우)
    - Fixed allocation을 위한 교체 기법
        - MIN(OPT, BO) algorithm
        - Random algorithm
        - FIFO algorithm
        - LRU algorithm
        - LFU algorithm
        - NUR algorithm
        - Clock algorithm
        - Second chance algorithm
    - Variable allocation을 위한 교체 기법
        - VMIN (Variable MIN) algorithm
        - WS algorithm
        - PFF(Page Fault Frequency) algorithm

### Cleaning Strategies

- 변경된 page를 언제 write-back 할 것인가?
    - 변경된 내용을 swap device에 반영
    - Demand cleaning: 해당 page에 메모리에서 내려올 때 write-back
    - Anticipatory cleaning(pre-cleaning)
        - 더 이상 변경될 가능성이 없다고 판단 할 때, 미리 write-back
        - Page 교체 시 발생하는 write-back 시간 절약
        - Write-back 이후, page 내용이 수정되면, overhead!
- 실제 대부분의 시스템은 Demand cleaning 기법 사용
    - 일반적으로 준수한 성능을 보여줌
    - Anticipatory cleaning: Prediction overhead 잘못된 예측 시 자원 낭비가 큼

### Load Control Strategies

- 시스템의 multi-programming degree 조절
    - Allocation strategies와 연계됨
- 적정 수준의 multi-programming degree를 유지해야 함
    - Plateau 영역으로 유지
    - 저부하 상태 : 시스템 자원 낭비, 성능 저하
    - 고부하 상태
        - 자원에 대한 경쟁 심화, 성능 저하
        - Thrashing(스레싱) 현상 발생: 과도한 Page Fault가 발생하는 현상

# 3. Replacement Strategies for Fixed Alloc.1

### Locality

- 프로세스가 프로그램/데이터의 특정 영역을 집중적으로 참조하는 현상
- 원인
    - Loop Structure in program
    - Array, structure 등의 데이터 구조
- 공간적 지역성 (Spatial locality): 참조한 영역과 인접한 영역을 참조하는 특성
- 시간적 지역성 (Temporal locality): 한 번 참조한 영역을 곧 다시 참조하는 특성

### Locality (Example)

- 가정
    - Paging system
    - Page size = 1000words
    - Machine instruction size = 1 word
    - 주소 지정은 word 단위로 이루어짐
    - 프로그램 4번 page에 continuous allocation 됨
    - n = 1000
- w = 494944
- 11000번의 메모리 참조 중 5개의 page만을 집중적으로 접근하게 됨

### Replacement Strategies

- Fixed allocation
    - MIN(OPT, B0) algorithm
    - Random algorithm
    - FIFO(First In First Out) algorithm
    - LRU(Least Recently Used) algorithm
    - LFU(Least Frequently Used) algorithm
    - NUR(Not Used Recently) algorithm
    - Clock algorithm
    - Second change algorithm
- Variable allocation
- WS algorithm
- PFF algorithm
- VMIN algorithm

### FIFO Algorithm

- First In First Out: 가장 오래된 page를 교체
- Page가 적재 된 시간을 기억하고 있어야 함
- 자주 사용되는 page가 교체될 가능성이 높음
    - Locality에 대한 고려가 없음
- FIFO anomaly (Belady’s anomaly)
    - FIFO 알고리즘의 경우, 더 많은 page frame을 할당 받음에도 불구하고 page fault의 수가 증가하는 경우가 있음

### LRU Algorithm

- 가장 오랫동안 참조되지 않은 page를 교체
- Page 참조 시 마다 시간을 기록해야함
- Locality에 기반을 둔 교체 기법
- MIN Algorithm에 근접한 성능을 보여줌
- 실제로 가장 많이 활용되는 기법
- 단점
    - 참조 시 마다 시간을 기록해야 함 (Overhead)
    - 간소화된 정보 수집으로 해소 가능
    - Loop 실행에 필요한 크기보다 작은 수의 page frame이 할당된 경우, page fault 수가 급격히 증가함
    - loop를 위한 |Ref.stsring| = 4 / 할당된 page frame이 3개
    - Allocation 기법에서 해결 해야 함

### LFU Algorithm

- 가장 참조 횟수가 적은 Page를 교체 : Tie-breaking rule: LRU
- Page 참조 시 마다, 참조 횟수를 누적 시켜야 함
- Locality 활용: LRU 대비 적은 overhead
- 단점
    - 최근 적재된 참조될 가능성이 높은 page가 교체 될 가능성이 있음
    - 참조 횟수 누적 overhead

### NUR Algorithm

- LRU approximation scheme: LRU보다 적은 overhead로 비슷한 성능 달성 목적
- Bit vector 사용: Reference bit vector, Update bit vector
- 교체 순서
    - (r, m) = (0, 0)
    - (r, m) = (0, 1)
    - (r, m) = (1, 0)
    - (r, m) = (1, 1)

### Clock Algorithm

- IBM VM/370 OS
- Reference bit 사용함: 주기적인 초기화 없음
- Page frame들을 순차적으로 기리키는 pointer(시계 바늘)을 사용하여 교체될 page 결정
- Pointer를 돌리면서 교체 Page 결정
    - 현재 가리키고 있는 page의 reference bit(확인)
    - r = 0인 경우, 교체 page로 결정
    - r = 1인 경우, reference bit 초기화 후 pointer 이동
- 먼저 적재된 Page가 교체될 가능성이 높음: FIFO와 유사
- Reference bit를 사용하여 교체 페이지 결정
    - LRU (or NUR)과 유사

### Second Chance Algorithm

- Clock Algorithm과 유사
- Update bit(m)도 함께 고려함
    - 현재 가리키고 있는 page의 (r, m) 확인
    - (0, 0); 교체 page로 결정
    - (0, 1); (0, 0) write-back(cleaning) list에 추가 후 이동
    - (1, 0); (0, 0) 후 이동
    - (1, 1);  (0, 1) 후 이동

### Other Algorithms

- Additional-reference-bits algorithm
    - LRU approximation
    - 여러 개의 reference bit를 가짐
        - 각 time-interval에 대한 참조 여부 기록
        - History register for each page
- MRU (Most Recently Used) Algorithm: LRU 정반대 기법
- MFU (Most Frequently Used) Algorithm: LFU 정반대 기법
