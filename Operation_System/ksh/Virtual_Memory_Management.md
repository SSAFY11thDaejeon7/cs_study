# 가상 메모리 관리
- 가상 메모리(기억장치)
  - Non-continuous allocation
    - 사용자 프로그램을 block으로 분할하여 적재/실행
  - Paging/Segmentation system
- 가상 메모리 관리의 목적
  - 가상 메모리 시스템 성능 최적화
    - Cost model
    - 다양한 최적화 기법

## Cost Model for Virtual Memory System
- Page fault rate를 최소화할 수 있도록 전략들을 설계해야 함
  - Context switch 및 Kernel 개입을 최소화
  - 시스템 성능 향상

- Page reference string
  - 프로세스의 수행 중 참조한 페이지 번호 순서
- Page fault rate = page fault의 수 / 참조했던 페이지의 길이

## Hardware Components
- Address translation device (주소 사상 장치)
  - 주소 사상을 효율적으로 수행하기 위해 사용
- Bit Vectors
  - page 사용 상황에 대한 정보를 기록하는 비트들(참조 비트, 갱신 비트)

- Reference bit vector
  - 메모리에 적재된 각각의 page가 최근에 참조되었는지를 표시
  - Reference 비트를 확인함으로써 최근에 참조된 page들을 확인 가능
- Update bit vector
  - Page가 메모리에 적재된 후, 프로세스에 의해 수정되었는지를 표시
  - 주기적 초기화 없음. 메모리에서 나올 때 초기화함

## Software Components

### Allocation Strategies
- 각 프로세스에게 메모리를 얼마만큼 줄 것인가?
  - 고정 할당: 프로세스의 실행동안 고정된 크기의 메모리 할당
  - 가변 할당: 프로세스의 실행동안 할당하는 메모리의 크기가 유동적

- 고려사항
  - 프로세스 실행에 필요한 메모리 양을 예측해야 함
  - 너무 큰 메모리 할당은 메모리가 낭비됨
  - 너무 적은 메모리 할당은 Page fault rate가 증가하고, 시스템 성능 저하
 
### Fetch Strategies
- 특정 page를 메모리에 언제 적재할 것인가?
  - Demand fetch: 프로세스가 참조하는 페이지들만 적재
  - Anticipatory
    - 참조될 가능성이 높은 page 예측
    - 가까운 미래에 참조될 가능성이 높은 page를 미리 적재
    - 예측 성공 시, page fault overhead가 없음
    - Prediction overhead (Kernel의 개입), Hit ratio에 민감함
- 실제 대부분의 시스템은 Demand fetch 기법 사용
  - 일반적으로 준수한 성능을 보여줌
 
### Placement Strategies
- Page/segment를 어디에 적재할 것인가?
- Paging system에는 불필요

### Cleaning Strategies
- 변경된 page를 언제 write-back 할 것인가?
  - 변경된 내용을 swap device에 반영
  - Demand cleaning
    - 해당 page에 메모리에서 내려올 때 write-back
  - Anticipatory cleaning (pre-cleaning)
    - 더 이상 변경될 가능성이 판단할 때, 미리 write-back
    - Page 교체시 발생하는 write-back 시간 절약
    - Write-back 이후, page 내용이 수정되면 overhead!
- 실제 대부분의 시스템은 Demand cleaning 기법 사용
  - 일반적으로 준수한 성능을 보여줌

### Load Control Strategies
- 시스템의 multi-programming degree 조절
  - Allocation strategies와 연계됨
- 적정 수준의 multi-programming degree를 유지해야 함
  - 고원 영역으로 유지
  - 저부하 상태: 시스템 자원 낭비, 성능 저하
  - 고부하 상태: 자원에 대한 경쟁 심화, 성능 저하
  - Thrashing 현상 발생: 과도한 page fault 발생

### Replacement Strategies
- Fixed allocation
- Variable allocation

### Fixed Allocation
- Minimize page fault frequency (증명됨): Optimal solution이라고도 함
- 기법: 앞으로 가장 오랫동안 참조되지 않을 page 교체
  - Tie-breaking rule: page 번호가 가장 큰/작은 페이지 교체
- 실현 불가능한 기법
  - Page reference string을 미리 알고 있어야 함
- 교체 기법의 성능 평가 도구로 사용됨

- 예시
  - 4page frames for the process, initially empty

![15일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/43dcea18-017f-46bc-b718-7dfb15a394b9)

- 5번 페이지를 읽으려고 할 때 6번 페이지를 가장 나중에 보므로 6번을 5번으로 교체함
- 두번째로 6번 페이지를 읽으려고 할 때 1, 2번이 Tie-breaking이 걸리므로 임의로 규칙을 정해서 교체
- 6번의 page fault가 발생함

### Random Algorithm
- 무작위로 교체할 page 선택
- Low overhead
- No policy

### FIFO Algorithm
- First In First Out
  - 가장 오래된 page를 교체
- Page가 적재된 시간을 기억하고 있어야 함
- 자주 사용되는 page가 교체될 가능성이 높음
- Locality에 대한 고려가 없음

- 예시
  - 4page frames for the process, initially empty

![15일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/9b42d551-eee1-43c0-b9f2-262e0a1cb048)

- 5번 페이지를 읽으려고 할 때 1번이 가장 먼저 들어왔으므로 1번을 5번으로 교체함
- 다음으로 1번 페이지를 읽으려고 할 때 2번을 1번으로 교체함
- 10번의 page fault가 발생함

- FIFO Anomaly 예시

![15일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/299cb15f-0f32-4282-bcbf-5b4db3b51020)

- 자원은 늘어났는데 성능은 더 안 좋아짐

### LRU (Least Recently Used) Algorithm
- 가장 오랫동안 참조되지 않은 page를 교체
- Page 참조시마다 시간을 기록해야 함
- Locality에 기반을 둔 교체 기법
- MIN algorithm에 근접한 성능을 보여줌
- 실제로 가장 많이 활용되는 기법

- 단점
  - 참조시마다 시간을 기록해야 함 (Overhead)
    - 간소화된 정보 수집으로 해소 가능
  - Loop 실행에 필요한 크기보다 작은 수의 page frame이 할당된 경우, page fault 수가 급격히 증가함

![15일차-4](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/a0a77acb-a053-426c-aa25-15310091eda4)

- 7번의 page fault가 발생함

### LFU (Least Frequently Used) Algorithm
- 가장 참조 횟수가 적은 Page를 교체
  - Tie-breaking rule: LRU
- Page 참조시마다, 참조 횟수를 누적시켜야 함
- Locality 활용
  - LRU 대비 적은 overhead
- 단점
  - 최근 적재된 참조될 가능성이 높은 page가 교체될 가능성이 있음
  - 참조 횟수 overhead가 있음

![15일차-5](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/ab18d792-5d5e-42a8-b8b2-194e5d5da91a)

- 7번의 page fault가 발생함

### NUR (Not Used Recently) Algorithm
- LRU approximation scheme
  - LRU보다 적은 overhead로 비슷한 성능 달성 목적
- Bit vector 사용

### Clock Algorithm
- Reference bit 사용함
  - 주기적인 초기화 없음
- Page frame들을 순차적으로 가리키는 pointer를 사용하여 교체될 page 결정
- 먼저 적재된 page가 교체될 가능성이 높음: FIFO와 유사함
- Reference bit를 사용하여 교체 페이지 결정
  - LRU(or NUR)과 유사

### Second Chance Algorithm
- Clock algorithm과 유사함
- Update bit도 함께 고려함

### Working Set algorithm
- Working set
  - 프로세스가 특정 시점에 자주 참조하는 page들의 집합
  - 최근 일정시간동안 참조된 page들의 집합
  - 시간에 따라 변함
- Working set memory management
  - Locality에 기반을 둠
  - Working set을 메모리에 항상 유지
    - Page fault rate 감소
    - 시스템 성능 향상
  - Window size는 고정
    - Memory allocation은 가변
    - Window size 값이 성능을 결정짓는 중요한 요소
- Window size가 늘어날 때 초반에는 Working set size가 급격하게 늘어나지만, 이후에 locality때문에 WS size가 늘어나는 것이 완화됨
- 성능 평가
  - Page fault 수 외에 다른 지표도 함께 봐야함
  - 평균 page frame수 = 3.2
- 특성
  - 적재되는 page가 없더라도, 메모리를 반납하는 page가 있을 수 있음
  - 새로 적재되는 page가 있더라도, 교체되는 page가 없을 수 있음
- 단점
  - Working set 관리 overhead
  - 상주 집합을 page fault가 없더라도 주기적으로 관리해야함

### Page Fault Frequency algorithm
- Residence set size를 page fault rate에 따라 결정
- Resident set 갱신 및 메모리 할당: overhead가 적음
- Algorithm
  - 1. Page fault 발생시, IFT 계산
  - 2. Low page fault rate일 경우, page fault가 일어난 시간부터 현재시간동안 참조된 page들만 유지하고 나머지 page들은 메모리에서 내림
  - 3. High page fault rate일 경우, 기존 pages들 유지하고 현재 참조된 page를 추가 적재함
- 성능 평가
  - Page fault 수 외에 다른 지표도 함께 봐야 함
  - 평균 page frame수 = 3.7
- 특징
  - 메모리 상태 변화가 page fault 발생시에만 일어남 -> low overhead

### Variable MIN algorithm
- 평균 메모리 할당량과 page fault 발생 횟수 모두 고려했을 때의 Optimal
- 실현 불가능한 기법
  - Page reference string을 미리 알고있어야함
- Algorithm
  - Page r이 t시간에 참조되면, page r이 t~t+window size 사이에 다시 참조되는지 확인
  - 참조된다면 page r을 유지하고 안된다면 page r을 메모리에서 내림
- 성능 평가
  - Page fault수 외에 다른 지표도 함꼐 봐야 함
  - 평균 할당 page frame수 = 1.6

### Page Size
- 시스템 특성에 따라 다름
  - No best answer! 적당한 것이 좋음
  - 점점 커지는 경향이 있음
- 일반적인 page size
  - 128 bytes ~ 4M bytes
- Small page size
  - high overhead
  - 내부 단편화 감소
  - I/O 시간 증가
  - Locality 향상
  - Page fault 증가
- Large page size
  - low overhead
  - 내부 단편화 증가
  - I/O 시간 감소
  - Locality 저하
  - Page fault 감소
- CPU와 Memory size가 발전하고 있으므로 상대적인 Page fault 처리 비용이 올라감

### Program Restructuring
- 가상 메모리 시스템의 특성에 맞도록 프로그램을 재구성
- 사용자가 가상 메모리 관리 기법에 대해 이해하고있다면, 프로그램의 구조를 변경하여 성능을 높일 수 있음

### TLB Reach
- TLB를 통해 접근할 수 있는 메모리의 양 (entry 수 * page size)
- TLB의 hit ratio를 높이려면,
  - TLB의 크기 증가: 비쌈
  - Page 크기 증가 or 다양한 page size 지원
    - OS의 지원이 필요함
