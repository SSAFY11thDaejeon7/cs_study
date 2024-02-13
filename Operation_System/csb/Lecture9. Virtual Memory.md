# Virtual Memory

### Virtual Storage(Memory)

- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시, 필요한 block들만 메모리에 적재
    - 나머지 block들은 swap device에 존재
- 기법들
    - Paging system
    - Segmentation system
    - Hybrid paging/segmentation system

### Address Mapping

- Continuous allocation
    - Relative address(상대 주소)
        - 프로그램의 시작 주소를 0으로 가정한 주소
    - Relocation(재배치)
        - 메모리 할당 후, 할당된 주소(allocation address)에 따라 상대 주소들을 조정하는 작업
- Non-continuous allocation
    - Virtual address(가상 주소) = relative address
        - Logical address(논리 주소)
        - 연속된 메모리 할당을 가정한 주소
    - Real address(실제주소) = absolute(physical)
        - 실제 메모리에 적재된 주소
    - Address mapping
        - Virtual address → real address
<img width="1032" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/84f12352-a1a2-42ad-8e4d-47b029ced22e">

- 사용자 / 프로세스는 실행 프로그램 전체가 메모리에 연속적으로 적재되었다고 가정하고 실행할 수 있음

### Block Mapping

- 사용자 프로그램을 block 단위로 분할 / 관리
    - 각 block에 대한 address mapping 정보 유지
- Virtual address : v = (b, d)
    - b = block number
    - d = displacement(offset) in a block
- Block map table(BMT)
    - Address mapping 정보 관리
        - Kernel 공간에 프로세스마다 하나의 BMT를 가짐
        - Residence bit: 해당 블록이 메모리에 적재되었는지 여부 (0 / 1)

<img width="899" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/76b98880-1694-455b-b09e-d7a932dd87ce">

### Block Mapping 절차

1. 프로세스의 BMT에 접근
2. BMT에서 block b에 대한 항목(entry)를 찾음
3. Residence bit 검사
    1. Residence bit = 0인 경우, swap device에서 해당 블록을 메모리로 가져 옴 BMT 업데이트 후 3-b단계 수행
    2. Residence bit = 1인 경우, BMT에서 b에 대한 real address 값 a 확인
4. 실제 주소 r 계산(r = a + d)
5. r을 이용하여 메모리에 접근

<img width="1001" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/142e8674-8b44-4c29-b2ef-bc45e97872ca">

# Virtual Storage Methods

### Paging System

- 프로그램을 같은 크기의 블록으로 분할(Pages)
- Terminologies
    - Page
        - 프로그램의 분할된 blcok
    - Page frame
        - 메모리의 분할 영역
        - Page와 같은 크기로 분할

<img width="1091" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/bf794bab-f717-47b1-a982-09666b107730">

- 특징
    - 논리적 분할이 아님(크기에 따른 분할)
        - Segmentation 대비 Page 공유(sharing) 및 보호(protection)과정이 복잡함
    - Simple and Efficient
        - Segmentation 대비
    - No external fragmentation
        - Internal fragmentation 발생 가능

### Address Mapping

- Virtual adress : v = (p, d)
    - p : page number
    - d : displacement(offset)
- Address mapping
    - PMT(Page Map Table) 사용
- Address mapping mechanism
    - Direct mapping(직접 사상)
    - Associative mapping(연관 사상)
        - TLB(Translation Look-aside Buffer)
    - Hybrid direct/associative mapping
- Page Map Table(PMT)

<img width="980" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/b2b8a988-95b8-479c-b10e-fdb53bcae094">

### Direct Mapping

- Block mapping 방법과 유사
- 가정
    - PMT를 커널 안에 저장
    - PMT entry size = entrySize
    - Page size = pageSize

### Direct Mapping 절차

1. 해당 프로세스의 PMT가 저장되어 있는 주소 b에 접근
2. 해당 PMT에서 page p에 대한 entry 찾음
    1. p의 entry 위치 = b + p * entrySize
3. 찾아진 entry의 존재 비트 ㄱ머사
    1. Residence bit = 0인 경우(page fault), swap device에서 해당 page를 메모리로 적재 PMT를 갱신한 후 3-b단계 수행
    2. Residence bit = 1인 경우, 해당 entry에서 page frame 번호 p’를 확인
4. p’와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
    1. r = p’ * pageSize + d
5. 실제 주소 r로 주기억장치에 접근

<img width="1090" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/1c11824b-753a-433f-9fb1-bfa547da95e4">

### Direct Mapping 문제점

- 메모리 접근 횟수가 2배 → 성능 저하
- PMT를 위한 메모리 공간 필요
- 해결 방안
    - Associative mapping(TLB)
    - PMT를 위한 전용 기억장치 (공간) 사용
        - Dedicated register or cache memory

### Associative Mapping

- TLB(Translation Look-aside Buffer)에 PMT 적재
- PMT를 병력 탐색
- Low overhead, High speed
- Expensive hardware → 큰 PMT를 다루기가 어려움

<img width="1014" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/78a63aa6-f04b-48d5-8dc8-95d5e1ba9bf0">

### Hybrid Direct/Associative Mapping

- 두 기법을 혼합하여 사용
    - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
    - PMT : 메모리(커널 공간)에 저장
    - TLB: PMT 중 일부 entry들을 적재
        - 최근에 사용된 page들에 대한 entry저장
    - Locality(지역성) 활용
        - 프로그램의 수행과정에서 한번 접근한 영역 또는 인접 영역을 다시 접근할 가능성이 높음

### Hybrid Direct/Associative Mapping 절차

1. 프로세스의 PMT가 TLB에 적재되어 있는지 확인
    1. TLB에 적재되어 있는 경우, residence bit를 검사하고 page frame번호 확인
    2. TLB에 적재되어 있지 않은 경우, Direct Mapping 으로 page frame번호 확인하고 해당 PMT entry를 TLB에 적재함

<img width="1090" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/1bcb71bf-97a8-4960-9cb9-52dfd24d0707">

### Memory Management

- Page와 같은 크기로 미리 분할하여 관리/사용
    - Page frame
    - FPM 기법과 유사
- Frame table
    - Page frame당 하나의 entry
    - 구성
        - Allocated/available field
        - PID field
        - Link field: For free list(사용가능한 fp들을 연결)
        - AV: Free list header(Free list의 시작점)

### Page Sharing

- 여러 프로세스가 특정 page를 공유 가능
    - Non-continuous allocation
- 공유 가능 page
    - Procedure pages
        - pure code(reenter code)
    - Data page
        - Read-only data
        - Read-write data
            - 병행성(concurrency) 제어 기법 관리하에서만 가능

### Page Sharing(Example)

- Editor 프로그램을 3명이 사용하는 경우

<img width="955" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/6ce24a31-147a-43cd-9af1-efd85fa4261f">

### Data page sharing

<img width="968" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/5ae84f4a-cfd9-471f-95f2-3ca592596ef6">

### Procedure Page Sharing(Problem)

<img width="1015" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/f9485f59-393b-4da9-b096-3ce9f1e013c8">

- 서로 지칭하는 이름이 다를 때 프로시저를 공유할 경우 문제가 발생할 수 있음!!

### Procedure Page Sharing(Solution)

<img width="993" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/855aac71-33ea-4048-be01-6a178fb64029">

- 프로세스들이 shared page에 대한 정보를 PMT의 같은 entry에 저장하도록 함

### Page Protection

- 여러 프로세스가 page를 공유 할 때 → Protection bit 사용

<img width="648" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/86e463ef-04ab-4b2a-9f49-c1905802a6ec">

### Paging System - Summary

- 프로그램을 고정된 크기의 block으로 분할(page) / 메모리를 block size로 미리 분할 (page frame)
    - 외부 단편화 문제 없음
    - 메모리 통합 / 압축 불필요
    - 프로그램의 논리적 구조 고려하지 않음
        - Page sharing / protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
    - 메모리의 효율적 활용
- Page mapping overhead
    - 메모리 공간 및 추가적인 메모리 접근이 필요
    - 전용 HW활용으로 해결 가능
        - 하드웨어 비용 증가
