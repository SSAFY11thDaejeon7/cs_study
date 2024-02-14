# Virtual Memory

- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시, 필요한 block들만 메모리에 적재
    - 나머지 block들은 swap device에 존재
- 기법들
    - Paging system
    - Segmentation system
    - Hybrid paging/segmentation system
    

## Address Mapping

- Continuous allocation
    - Relative address(상대 주소)
        - 프로그램의 시작 주소를 0으로 가정한 주소
    - Relocation(재배치)
        - 메모리 할당 후, 할당된 주소(allocation address)에 따라 상대 주소들을 조정하는 작업
- Non-continuous allocation
    - Virtual address(가상 주소) = relative address
        - Logical address(논리 주소)
        - 연속된 메모리 할당을 가정한 주소
    - Real address(실제 주소) = absolute(physical)
        - 실제 메모리에 적재된 주소
    - Address mapping
        - Virtual address → real address
    - 사용자/프로세스는 실행 프로그램 전체가 메모리에 연속적으로 적재되었다고 가정하고 실행할 수 있음
    

## Block Mapping

- 사용자 프로그램을 block 단위로 분할/관리
    - 각 block에 대한 address mapping 정보 유지
- Virtual address : v = (b, d)
    - b: block number
    - d: displacement(offset) in a block
- Block map table(BMT)
    - Address mapping 정보 관리
        - Kernel 공간에 프로세스마다 하나의 BMT를 가짐
        
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/9d5eddc7-cc18-4b56-aaf7-a212f1e1cbc1)

        
        - Residence bit: 해당 블록이 메모리에 적재되었는지 여부(0 or 1)
        
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/5e5d237b-49fb-49da-bdfa-bae922239347)

        
    - Block Mapping
        1. 프로세스의 BMT에 접근
        2. BMT에서 block b에 대한 항목(entry)를 찾음
        3. Residence bit 검사
            1. Residence bit = 0인 경우,
            swap device에서 해당 블록을 메모리로 가져옴
            BTM 업데이트 후 3-b 단계 수행
            2. Residence bit = 1인 경우,
            BMT에서 b에 대한 real address 값 a 확인
        4. 실제 주소 r 계산(r = a + d)
        5. r을 이용하여 메모리에 접근
    

## Virtual memory methods

- Paging system
- Segmentation system
- Hybrid paging/segmentation system

## Paging System

- 프로그램을 같은 크기의 블록으로 분할(Pages)
- Terminologies
    - Page: 프로그램의 분할된 block
    - Page frame: 메모리의 분할 영역, Page와 같은 크기로 분할
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/2c81ce86-f5fb-4ea4-85b9-ddfdcb3fb0b0)

    
- 특징
    - 논리적 분할이 아님(크기에 따른 분할)
        - Page 공유 및 보호 과정이 복잡함(Segmentation 대비)
        
    - Simple and Efficient(Segmentation 대비)
    - No external fragmentation
        - Internal fragmentation 발생 가능
- Address Mapping
    - Virtual address : v = (p, d)
    - Address mapping: PMT(Page Map Table 사용)
    - Address mapping mechanism
        - Direct mapping(직접 사상)
        - Associative mapping(연관 사상)
        - Hybrid direct/associative mapping
- Page Map Table(PMT)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/8ec6b285-e9c2-4884-8ba7-86f849389ea5)


## Direct Mapping

- Block mapping 방법과 유사
- 가정
    - PMT를 커널 안에 저장
    - PMT entry size = entrySize
    - Page size = pageSize
- 과정
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/9fede1dd-5667-403c-b26b-8dc497971356)

    
    1. 해당 프로세스의 PMT가 저장되어 있는 주소 b에 접근
    2. 해당 PMT에서 page p에 대한 entry 찾음
        - p의 entry의 위치 = b + p * entrySize
    3. 찾아진 entry의 존재 비트 검사
        1. Residence bit = 0인 경우(page fault),
        swap device에서 해당 page를 메모리로 적재
        PMT를 갱신한 후 3-b 단계 수행
        2. Residence bit = 1인 경우
        해당 entry에서 page frame 번호 p’를 확인
    4. p’와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
        - r = p’ * pageSize + d
    5. 실제 주소 r로 주기억장치에 접근
- 문제점
    - 메모리 접근 횟수가 2배: 성능 저하
    - PMT를 위한 메모리 공간 필요
- 해결 방안
    - Associative mapping(TLB)
    - PMT를 위한 전용 기억장치(공간) 사용
    - Hierarchical paging
    - Hashed page table
    - Inverted page table

## Associate Mapping

- TLB(Translation Look-aside Buffer)에 PMT 적재
- PMT를 병렬 탐색
- Low overhead, high speed
- Expensive hardware
    - 큰 PMT를 다루기가 어려움

## Hybrid Direct/Associative Mapping

- 두 기법을 혼합하여 사용
    - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
    - PMT: 메모리(커널 공간)에 저장
    - TLB: PMT중 일부 entry들을 적재
        - 최근에 사용된 apge들에 대한 entry 저장
    - Locality(지역성) 활용
        - 프로그램의 수행 과정에서 한번 접근한 영역을 다시 접근(temporal locality), 또는 인접 영역을 다시 접근(spatial locality)할 가능성이 높음
- 과정
    - 프로세스의 PMT가 TLB에 적재되어 있는지 확인
        1. TLB에 적재되어 있는 경우,
            1. residence bit를 검사하고 page frame 번호 확인
        2. TBL에 적재되어 있지 않은 경우,
            1. Direct mapping으로 page frame 번호 확인
            2. 해당 PMT entry를 TLB에 적재함
        
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/8375dd24-4459-4a8b-9a40-bd46c1bf541f)

        

## Memory Management

- Page와 같은 크기로 미리 분할하여 관리/사용
    - Page frame
    - FPM 기법과 유사
- Frame table
    - Page frame당 하나의 entry
    - 구성
        - Allocated/available field
        - PID field
        - LInk field: For free list(사용 가능한 fp들을 연결)
        - AV: Free list header(free list의 시작점)

## Page sharing

- 여러 프로세스가 특정 page를 공유 가능
    - Non-continuous allocation
- 공유 가능 page
    - Procedure pages: Pure code(reenter code)
    - Data page
        - Read-only data
        - Read-write data
            - 병행성(concurrency) 제어 기법 관리 하에서만 가능

## Page Protection

- 여러 프로세스가 page를  공유 할 때
    - Protection bit 사용
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/ccf98952-51fa-4fe3-b524-ba6ef0bd1606)

    

## Paging System 요약

- 프로그램을 고정된 크기의 block으로 분할(page),
 메모리를 block size로 미리 분할(page frame)
    - 외부 단편화 문제 x
    - 메모리 통합/압축 불필요
    - 프로그램의 논리적 구조 고려하지 않음
        - Page sharing/protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
    - 메모리의 효율적 활용
- Page mapping overhead
    - 메모리 공간 및 추가적인 메모리 접근이 필요
    - 전용 HW 활용으로 해결 가능
        - 하드웨어 비용 증가

## Segmentation System

- 프로그램을 논리적 block으로 분할(segment)
    - Block의 크기가 서로 다를 수 있음
    - ex) stack, heap, main procedure…
- 특징
- 메모리를 미리 분할 하지 않음
- Segment sharing/protection이 용이함
- Address mapping 및 메모리 관리의 overhead가 큼
- 내부 단편화 발생x
    - 외부 단편화 발생 가능

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/7264bc1c-0483-441c-8e50-d2cb1d38ec2d)


- Address mapping
    - Virtual address: v = (s, d)
        - s: segment number
        - d: displacement in a segment
    - Segment Map Table(SMT)
    - Address mapping mechanism
        - Paging system과 유사
- 과정
    
   ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/aa5cbfb2-e64d-4a25-b89c-228ba861b4f4)

    
    1. 프로세스의 SMT가 저장되어 있는 주소 b에 접근
    2. SMT에서 segment s의 entry를 찾음
        1. s의 entry 위치 = b + s * entrySize
    3. 찾아진 entry에 대해 아래 과정 수행
        1. 존재 비트가 0인 경우(segment fault),
        swap device로부터 해당 segment를 메모리로 적재, SMT 갱신
        2. 변위(d)가 segment 길이보다 큰 경우,
        segment overflow exception 처리 모듈 호출
        3. 허가되지 않은 연산일 경우,
        segment protection exception 처리 모듈 호출
    4. 실제 주소 r 계산(r = a + d_
    5. r로 메모리에 접근
- Memory management
    - VPM과 유사, segment 적재 시, 크기에 맞추어 분할 후 적재
- Segment sharing/protection
    - 논리적으로 분할되어 있어, 공유 및 보호가 용이

## Segmentation System 요약

- 프로그램을 논리 단위로 분할(segment), 메모리를 동적으로 분할
    - 내부 단편화 문제 x
    - segment sharing/protection이 용이
    - Paging system 대비 관리 overhead가 큼
- 필요한 segment만 메모리에 적재하여 사용
- segment mapping overhead
    - 메모리 공간 및 추가적인 메모리 접근이 필요
    - 전용 HW 활용으로 해결 가능
    

## Hybrid paging/segmentation system

- Paging과 Segmentation의 장점 결합
- 프로그램 분할
    - 논리 단위의 segment로 분할
    - 각 segment를 고정된 크기의 page들로 분할
- Page 단위로 메모리에 적재
- 

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/077468b7-1853-42ae-9fa9-6dbf1b3cebed)


- Address mapping
    - Virtual address: v = (s, pd)
        - s: segment number
        - p: page number
        - d: offset in a page
    - SMT와 PMT 모두 사용
    - Address mapping
        - Direct, associated 등
    - 메모리 관리: FPM과 유사
- Direct(address) mapping

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/d23803ca-2785-4f44-bc5d-2d24f1e8f2b2)


- 요약
    - 논리적 분할(segment)와 고정 크기 분할(page)을 결합
        - Page sharing/protection이 쉬움
        - 메모리 할당/관리 overhead가 작음
        - No external fragmentation
    - 전체 테이블의 수 증가
        - 메모리 소모가 큼
        - Address mapping 과정이 복잡
    - Direct mapping의 경우, 메모리 접근이 3배
        - 성능이 저하될 수 있음
