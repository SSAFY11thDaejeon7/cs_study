# 1. Virtual Storage(Memory)

### Virtual Storage란?

- 비연속적인 할당
- 사용자 프로그램을 여러 개의 **block**으로 분할한다
- 실행 시, 필요한 block들만 메모리에 적재한다.
    - 나머지 block들은 swap device(disk)에 존재
- 기법들
    - Paging System
    - Segmentation System
    - Hybrid Paging/ Segmentation System

# 2. Address Mapping

### Continuouns allocation

- 상대주소 (Relative address)
    - 프로그램의 시작 주소를 0으로 가정한 주소
- 재배치  (Relocation)
    - 메모리 할당 후, 할당된 주소에 따라 상대 주소들을 조정하는 작업
<img width="529" alt="AddressMapping" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/b7fbe03c-97f3-4857-a3be-e0345e855c81">


### Non-Continuous allocation

- Virtual Address(가상 주소) = relative address
    - Logical Address(논리주소)
    - 연속된 메모리 할당을 가정한 주소
- Real Address(실제주소) = absolute(physical)
    - 실제 메모리에 적재된 주소
- Address mapping
    - Virtual address → real address

> 사용자/ 프로세스는 실행 프로그램 전체가 메모리에 연속적으로 적재되었다고 가정하고 실행 할 수 있음!
> 

### Block Mapping

- 특징
    - 사용자 프로그램을 block 단위로 분할/관리한다
    - 각 block에 대한 address mapping 정보 유지
    - Virtual Address: v = (b, d)
        - b = block number
        - d = displacement(offset) in a block (block으로부터 떨어져있는 거리)
- BMT(Block Map Table)
    - Address mapping 정보 관리
    - Kernal 공간에 프로세스마다 하나의 BMT를 가짐
    - Residence bit: 해당 블록이 메모리에 적재되었는지 여부(0/1)
    <img width="409" alt="BMT2" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/24a5c00b-28ae-499e-9b21-fdc2a206c70a">
    <img width="478" alt="BMT22" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/fa64f347-54cb-4e6e-ad43-ed695fd537ad">

        
    - 과정
        1. 프로세스의 BMT에 접근
        2. BMT에서 block b에 대한 항목(entry)를 찾음
        3. Residence bit 검사
        4. 실제 주소 r 계산 (r = a + b)
        5. r을 이용하여 메모리에 접근

# 3. Paging System

### 정의

- 프로그램을 같은 크기의 블록(pages)으로 분할하는 것
- Terminologies
    - page: 프로그램의 분할된 block
    - page frame: 메모리의 분할 영역/ page와 같은 크기로 분할
    <img width="502" alt="PagingSystem1" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/19bf535d-fd2f-4f0e-b5d8-b78359b6afaa">

    

### 특징

- 논리적 분할이 아님(크기에 따른 분할)
    - page 공유(sharing) 및 보호(protection) 과정이 복잡함
    - segmentation 대비
- Simple and Efficient
    - Segmentation 대비
- No External Fragmentation 외부 단편화가 발생하지 않는다.
    - Internal Fragmentation 발생 가능 → 프로세스의 작은 부분이 page frame에 들어갈 경우 내부 단편화 발생

### Address Mapping

- Virtual Address = v = (p, d)
    - p: page number
    - d: displacement (offset)
- Address mapping
    - PMT(Page Map Table) 사용
- Address mapping mechanism
    - Direct mapping (직접 사상)
    - Associative mapping(연관 사상)
        - TLB(Translation Look-aside Buffer)
    - Hybrid direct/associative mapping

### Direct mapping

- Block mapping 방법과 유사
- 가정
    - PMT를 커널 안에 저장
    - PMT entry size = entrySize
    - Page size = pageSize
 <img width="519" alt="PagingSystem2" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/03f93a1d-3690-470e-9e64-74b24671bc32">

      


- 과정
    1. 해당 프로세스의 PMT가 저장되어 있는 주소b에 접근
    2. 해당 PMT에서 page p에 대한 entry 찾음
    3. 찾아진 entry의 존재 비트 검사
        1. Residence bit = 0인 경우 **page fault**, swap device에서 해당 page를 메모리에 적재, PMT를 갱신한 후 3-2 단계 수행
        2. Residence bit = 1인 경우, 해당 entry에서 page frame 번호 p를 확인
    4. p와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
    5. 실제 주소 r로 주기억장치에 접근
- 문제점
    - 메모리 접근 횟수가 2배 (성능 저하)
    - PMT를 위한 메모리 공간이 필요하다
    - 프로그램의 논리적 구조를 고려하지 않음
- 해결방안
    - Assoicative mapping (TLB)
    - PMT를 위한 전용 기억장치(공간) 사용
- Assoicative mapping
    - TLB(Translation Look-aside Buffer)에 PMT 적재
    - PMT를 병렬 탐색
    - Low overhead, high speed (메모리 접근에서 발생했던 성능 저하를 막을 수 있음)
    - Expensive hardware (큰 PMT를 다루기가 어려움)

### Hybrid Direct/Associative Mapping

- 두 기법을 혼합하여 사용
    - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB를 사용한다.
    - PMT: 메모리에 저장(커널 공간에 저장한다)
    - TLB: PMT 중 일부 entry들을 적재한다.
        - 최근에 사용된 page들에 대한 entry를 저장한다
    - Locality(지역성) 활용
        - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근한다.
        - 또는 인접 영역을 다시 접근할 가능성이 높다.
    <img width="382" alt="PagingSystem3" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/9102249b-f1fc-4d74-9386-626aad8af93a">

    

### Memory Management

- Page와 같은 크기로 미리 분할하여 관리/사용한다.
    - Page frame
    - FPM 기법과 유사
- Frame Table
    - Page frame당 하나의 entry
    - 구성
        - Allocated/available field
        - PID field
        - Link field: For Free List
        - AV: Free list header
        <img width="386" alt="PagingSystem4" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/58d7e4cc-e4a7-4e89-a692-cf64a5c9f3b4">

        
    

### Page Sharing

- 여러 프로세스가 특정 page를 공유 가능 (Non-continuous allocation 이기 때문에 가능)
- 공유 가능 page
    - Procedure pages
        - 함수 등에서 공통 되는 부분 공유
        - Pure Code(reenter code)
    - Data page
        - Read-only data
        - Read-Write data → 병행성 관리 하에서만 가능하다

### Page Protection

- 여러 프로세스가 page를 공유 할 때, Protection bit를 사용한다.

# 4. Segmentation System

### Segmentation System

- 프로그램을 **논리적 block**으로 분할 (segment)
- Block의 크기가 서로 다를 수 있음 (stack, heap, main procedure, shared lib, Etc)
- 특징
    - **메모리를 미리 분할하지 않음** (VPM과 유사)
    - Segment sharing / protection이 용이함
    - Address mapping 및 메모리 관리의 overhead가 큼
    - No internal fragmentation
        - External fragmentation 발생 가능
    
    <img width="490" alt="segmentationsystem" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/62443c82-5084-4a9c-b547-1ae644e1050e">


### Address Mapping (Virtual Address → Real Address)

- Virtual address: v = (s, d)
    - s: segment number
    - d: displacement in a segment
- Segment Map Table (SMT)
- Address Mapping Mechanism
    - Paging System과 유사

### Memory Management

- VPM과 유사
- Segment 적재 시, 크기에 맞추어 분할 후 적재한다.
<img width="533" alt="pagingvssegmentation" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/110437548/7778fe4b-0391-4582-ba35-f17480da02e0">


# 5. Hybrid Paging/ Segmentation System

- Paging과 Segmentation의 장점 결합
- 프로그램 분할
    - 논리 단위의 segment로 분할
    - 각 segment를 고정된 크기의 page들로 분할
- Page 단위로 메모리에 적재한다.

### Address mapping

- Virtual Address: v = (s, p, d)
    - s: segment number
    - p: page number
    - d: offset in a page
- SMT와 PMT 모두 사용
    - 프로세스 마다 하나의 SMT
    - 각 Segment마다 하나의 PMT
- Address mapping: Direct, associated 등
- 메모리 관리: FPM과 유사

### Summary

- 논리적 분할와 고정 크기 분할을 결합
    - Page Sharing/ protection이 쉬움
    - 메모리 할당 및 관리의 overhead가 작음
    - No external Fragmentation (내부 단편화만 발생)
- 전체 테이블 수가 증가한다
    - 메모리의 소모가 크다
    - Address mapping 과정이 복잡하다.
- Direct mapping의 경우, 메모리 접근이 3배
    - 성능이 저하될 수 있다.
