# [OS] Lecture 9. Non-continuous allocation / Virtual memory

## Virtual Storage (Memory)
- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시, 필요한 block들만 메모리에 적재
  - 나머지 block들은 swap device에 존재
- 기법들
  - Paging system
  - Segmentation system
  - Hybrid paging / segmentation system
- Continuous allocation
  
  <img width="562" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/cd8b567f-4c1d-4924-94d5-ff31602d778e">

  - Relative address(상대 주소)
    - 프로그램의 시작 주소를 0으로 가정한 주소
  - Relocation(재배치)
    - 메모리 할당, 후 할당된 주소에 따라 상대 주소들을 조정하는 작업
- Non-continuous allocation
  
  <img width="562" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/4c624a30-d5a5-4729-92dc-310fc06ad89d">

  - Virtual address(가상주소) = relative address
    - Logical address(논리주소)
    - 연속된 메모리 할당을 가정한 주소
  - Real address(실제주소) = absolute (physical)
    - 실제 메모리에 적재된 주소
  - Address mapping
    - Virtual address -> real address

## Block Mapping
- 사용자 프로그램을 block 단위로 분할/관리
  - 각 block에 대한 address mapping 정보 유지
- Virtual address : v = (b, d)
  - b = block number
  - d = displacement(offset) in a block
    
  <img width="374" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/0a68e096-813f-4ae4-a1e4-0c0c688e7973">

- Block map table (BMT)
  - Address mapping 정보 관리
    - Kernel 공간에 프로세스마다 하나의 BMT를 가짐
      
    <img width="442" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/77044a56-a331-4787-888d-c8c69b69480a">

    - Residence bit: 해당 블록이 메모리에 적재되었는지 여부 (0/1)
<img width="475" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/749687d8-3faa-414b-84d0-e7005c6ce06f">

1. 프로세스의 BMT에 접근
2. BMT에서 block b에 대한 항목(entry)을 찾음
3. Residence bit 검사
   1) Residence bit = 0 경우,
   swap device에서 해당 블록을 메모리로 가져옴
   BMT 업데이트 후 3.2) 단계 수행
   2) Residence bit = 1 경우,
   BMT에서 b에 대한 real address 값 a 확인
4. 실제 주소 r 계산 (r = a + d)
5. r을 이용하여 메모리에 접근

## Virtual Storage Methods
- Paging system
- Segmentation system
- Hybrid paging/segmentation system

## Paging system
- 프로그램을 같은 크기의 블록으로 분할 (Pages)
- Terminologies
  - Page
    - 프로그램의 분할된 block
  - Page frame
    - 메모리의 분할 영역
    - Page와 같은 크기로 분할
      
<img width="553" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/a6daebf9-b290-4ec2-90e8-4456a653a0c4">

- 특징
  - 논리적 분할이 아님(크기에 따른 분할)
    - Page 공유(sharing) 및 보호(protection) 과정이 복잡함
      - segmentation 대비
  - Simple and Efficient
    - Segmentation 대비
  - No external fragmentation
    - Internal fragmentation 

## Address Mapping
- Virtual address : v = (p, d)
    - p: page number
    - d: displacement(offset)
- Address mapping
  - PMT(Page Map Table) 사용
    
    <img width="496" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/5f369b94-7772-4f80-af5c-843803d80eee">

- Address mapping mechanism
  - Direct mapping(직접 사상)
  - Associative mapping(연관 사상)
    - TLB(Translation Look-aside Buffer)
  - Hybrid direct/associative mapping
## Direct mapping
  - Block mapping 방법과 유사
  - 가정
    - PMT를 커널 안에 저장
    - PMT entry size = entrySize
    - Page size = pageSize
      
  <img width="528" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/2b5d9406-fcf5-4183-a717-14f23742e9f1">

1. 해당 프로세스의 PMT가 저장되어 있는 주소 b에 접근
2. 해당 PMT에서 page p에 대한 entry 찾음
    - p의 entry 위치 = b + p * entrySize
3. 찾아진 entry의 존재 비트 검사
   1) Residence bit = 0 인 경우 (page fault),
   swap device에서 해당 page를 메모리로 적재
   PMT를 갱신한 후 3-2)단계 수행
   2) Residence bit = 1인 경우,
   해당 entry에서 page frame 번호 p'를 확인
4. p'와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
   r = p' * pageSize + d
5. 실제 주소 r로 주기억장치에 접근
- 문제점
  - 메모리 접근 횟수가 2배
    - 성능 저하(performance degradation)
  - PMT를 위한 메모리 공간 필요
- 해결 방안
  - Associative mapping(TLB)
  - PMT를 위한 전용 기억장치(공간) 사용
    - Dedicated register or cache memory
  - Hierarchical paging
  - Hashed page table
  - Inverted page table
## Associative Mapping
<img width="526" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/028f2eb7-2e3d-440b-857f-ad2fc319093d">

  - TLB(Translation Look-aside Buffer)에 PMT 적재
    - Associative high-speed memory
  - PMT를 병렬 탐색
  - Low overhead, high speed
  - Expensive hardware
    - 큰 PMT를 다루기가 어려움
## Hybrid direct/associative mapping
- 두 기법을 혼합하여 사용
  - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
  - PMT : 메모리(커널 공간)에 저장
  - TLB : PMT 중 일부 entry들을 적재
    - 최근에 사용된 page들에 대한 entry 저장
  - Locality(지역성) 활용
    - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근(temporal locality) 또는 인접 영역을 다시 접근 (spatial locality)할 가능성이 높음
- 프로세스의 PMT가 TLB에 적재되어 있는지 확인
  1. TLB에 적재되어 있는 경우,
     - residence bit를 검사하고 page frame번호 확인
  2. TLB치에 적재되어 있지 않은 경우,
     - Direct mapping으로 page frame 번호 확인
     - 해당 PMT entry를 TLB에 적재함
       
  <img width="560" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/aded9c52-c2cf-4d43-9a84-135a17e9e3a3">

## Memory Management
- Page와 같은 크기로 미리 분할하여 관리/사용
  - Page frame
  - FPM 기법과 유사
- Frame table
  - Page frame당 하나의 entry
  - 구성
    - Allocated/available field
    - PID field
    - Link field: For free list (사용 가능한 fp들을 연결)
    - AV: Free list header (free list의 시작점)
      
  <img width="408" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/5809d2ab-9504-49a4-af42-ab191a3cc391">

## Page Sharing
- 여러 프로세스가 특정 page를 공유 가능
  - Non-continuous allocation!
- 공유 가능 page
  - Procedure pages
    - Pure code (reenter code)
  - Data page
    - Read-only data
    - Read-write data
      - 병행성(concurrency) 제어 기법 관리하에서만 가능
        
  <img width="518" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/bfaaeb80-cfa5-43e1-8eb7-cafabdcaa802">
  <img width="518" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/7b787814-1391-4bcf-9c66-5c4fb52c1e57">
  <img width="546" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/a07d6ebd-116d-43a1-9df3-8e3b51eaccb3">
  <img width="558" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/2629d028-fb7c-4fa0-b08b-d8c04ec82954">


## Paging System - Summary
- 프로그램을 고정된 크기의 block으로 분할 (page) / 메모리를 block size로 미리 분할 (page frame)
  - 외부 단편화 문제 없음
  - 메모리 통합/압축 불필요
  - 프로그램의 논리적 구조 고려하지 않음
    - Page sharing / protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
  - 메모리의 효율적 활용
- Page mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
    - 하드웨어 비용 증가

## Segmentation System
- 프로그램을 논리적 block으로 분할(segment)
  - Block의 크기가 서로 다를 수 있음
  - 예) stack, heap, main procedure, shared lib, Etc.
- 특징
  - 메모리를 미리 분할하지 않음
    - VPM과 유사
  - Segment sharing/protection이 용이함
  - Address mapping 및 메모리 관리의 overhead가 큼
  - No internal fragmentation
    - External fragmentation 발생 가능
      
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/5902aece-e991-44d7-b2a3-89e66ad62130)

- Address mapping
  - Virtual address: v = (s, d)
    - s: segment number
    - d: displacement in a segment
  - Segment Map Table (SMT)
    
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/f2224a88-3a67-4620-aab0-def094433b38)

  - Address mapping mechanism
    - Paging system과 유사
      
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/e035efd5-414f-4a47-b20a-19294961a1b8)

1. 프로세스의 SMT가 저장되어 있는 주소 b에 접근
2. SMT에서 segment s의 entry 찾음
   - s의 entry 위치 = b + s * entrySize
3. 찾아진 Entry에 대해 다음 단계들을 순차적으로 실행
   1. 존재 비트가 0인 경우
      // missing segment fault
      swap device로부터 해당 segment를 메모리로 적재
      SMT를 갱신
   2. 변위(d)가 segment 길이보다 큰 경우 (d > l_s),
      segment overflow exception 처리 모듈을 호출
   3. 허가되지 않은 연산일 경우 (protection bit field 검사),
      segment protection exception 처리 모듈을 호출
4. 실제 주소 r 계산 (r = a_s+d)
5. r로 메모리에 접근

- Memory management
  - VPM과 유사
    - Segment 적재 시, 크기에 맞추어 분할 후 적재
      
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/89f8e89f-fe43-47a5-83a4-139b0fe3d112)

- Segment sharing/protection
  - 논리적으로 분할되어 있어, 공유 및 보호가 용이함
    
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/d7412e97-c9ae-4a54-8d46-e68ff854bcac)

## Segmentation System - Summary
- 프로그램을 논리 단위로 분할(segment) / 메모리를 동적으로 분할
  - 내부 단편화 문제 없음
  - Segment sharing/protection이 용이함
  - Paging system 대비 관리 overhead가 큼
- 필요한 segment만 메모리에 적재하여 사용
  - 메모리의 효율적 활용
- Segment mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능

## Paging vs Segmentation
### Paging System
- Simple
- Low overhead
- No logical concept for partitioning
- Complex page sharing mechanism
### Segmentation System
- High management overhead
- Logical concept for partitioning
- Simple and easy sharing mechanism

## Hybrid Paging/Segmentation
- Paging과 Segmentation의 장점 결합
- 프로그램 분할
  - 논리 단위의 segment로 분할
  - 각 segment를 고정된 크기의 page들로 분할
- Page 단위로 메모리에 적재
  
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/49dd6198-ce3a-45aa-be70-a49a146b4aea)

- Address mapping
  - Virtual address : v = (s, p, d)
    - s: segment number
    - p: page number
    - d: offset in a page
  - SMT와 PMT 모두 사용
    - 각 프로세스마다 하나의 SMT
    - 각 segment마다 하나의 PMT
  - Address mapping
    - Direct, associated 등
  - 메모리 관리
    - FPM과 유사
- SMT in hybrid mechanism
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/8d2b3280-bb7e-4d95-9716-d4205b910f36)

- PMT for a segment k in hybrid mechanism
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/06204028-70f7-47fb-9038-cc338fa06d35)

- Address mapping tables
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/83ede07a-d81c-47b7-8050-6346286b7573)

- Direct (address) mapping
  
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/8b0a406b-b0f2-4c2f-9580-f45cb8d0e33a)

## Hybrid Paging/Segmentation - Summary
- 논리적 분할과 고정크기 분할을 결합
  - Page sharing/protection이 쉬움
  - 메모리 할당/관리 overhead가 작음
  - No external fragmentation
- 전체 테이블의 수 증가
  - 메모리 소모가 큼
  - Address mapping 과정이 복잡
- Direct mapping의 경우, 메모리 접근이 3배
  - 성능이 저하될 수 있음
