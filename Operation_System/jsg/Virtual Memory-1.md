# Virtual Storage (Memory)

<img width="640" alt="스크린샷 2024-02-12 오후 6 27 30" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/aef4111d-cbf9-408e-bd9c-f8f84c70e1c6">

- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시 필요한 block들만 메모리에 적재
  - 나머지 block들은 swap device에 존재
- 기법들
  - Paging system
  - Segmentation system
  - Hybrid paging/segmentation system
## Address Mapping
- Continuous allocation
  - Relative address (상대 주소)
    - 프로그램의 시작 주소를 0으로 가정한 주소
  - Relocation (재배치)
    - 메모리 할당 후, 할당된 주소(allocation address)에 따라 상대 주소들을 조정하는 작업
  <img width="694" alt="스크린샷 2024-02-12 오후 5 36 27" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/ddfdeb3b-dcc2-4580-a193-c3a7222ee0b8">
- Non-continuous allocation
  - Virtual address (가장주소) = relative address
    - Logical address(논리주소)
    - 연속된 메모리 할당을 가정한 주소
  - Real address (실제주소) = absolute (physical)
    - 실제 메모리에 적재된 주소
  - Address mapping
    - Virtual address -> real address
  <img width="683" alt="스크린샷 2024-02-12 오후 5 41 03" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/a3ad298c-5848-4dd6-811c-286b67cebe8a">

## Block Mapping
- 사용자 프로그램을 block 단위로 분할/관리
  - 각 block에 대한 address mapping 정보 유지
- Virtual address : v = (b,d)
  - b = block number
  - d = displacement(offset) in a block
- Block map table (BMT)
  - Address mapping 정보 관리
    - Kernel 공간에 프로세스마다 하나의 BMT를 가짐
    <img width="569" alt="스크린샷 2024-02-12 오후 5 48 57" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/2f58606e-5f8d-4b08-8010-75713c9bf300">

<img width="654" alt="스크린샷 2024-02-12 오후 5 52 27" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/de69e628-d11f-4906-a135-04dba05ab739">

1. 프로세스의 BMT에 접근
2. BMT에서 block b에 대한 항목(entry)를 찾음
3. Residence bit 검사
  1. Residence bit = 0 경우, swap device에서 해당 블록을 메모리로 가져 옴. BtM 업데이트 후 3-2 단계 수행
  2. Residence bit = 1 경우, BMT에서 b에 대한 real address 값 a 확인 
4. 실제 주소 r 계산(r = a + d)
5. r을 이용하여 메모리에 접근

## Paging system
- 프로그램을 같은 크기의 블록으로 분할 (Pages)
- Terminologies
  - Page
    - 프로그램의 분할된 block
  - Page frame
    - 메모리의 분할 영역
    - Page와 같은 크기로 분할

<img width="694" alt="스크린샷 2024-02-12 오후 5 58 21" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/fed93622-d4a6-4995-9d26-aec1a95bba70">

- 특징
  - 논리적 분할이 아님(크기에 따른 분할)
    - Page 공유(sharing)및 보호(protection) 과정이 복잡함
      - Segmentation 대비
  - Simple and Efficient
    - Segmentation 대비
  - No external fragmentation
    - Internal fragmentation 발생 가능
## Direct mapping
- Block mapping 방법과 유사
- 가정
  - PMT를 커널 안에 저장
  - PMT entry size = entrySize
  - Page size = pageSize
<img width="678" alt="스크린샷 2024-02-12 오후 6 23 50" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/f75b5958-2a52-4910-8afa-cc5cefb63a5a">

- 문제점
  - 메모리 접근 횟수가 2배
    - 성능 저하
  - PMT를 위한 메모리 공간  필요
- 해결방안
  - Associative mapping (TLB)
  - PMT를 위한 전용 기억장치 사용
## Associative Mapping
- TLB(Translation Look-aside Buffer)에 PMT 적재
  - Associative high-speed memory
- PMT를 병렬 탐색
- Low overhead, high speed
- Expensive hrdware
  - 큰 PMT를 다루기가 어려움 
<img width="644" alt="스크린샷 2024-02-12 오후 6 27 40" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/19eeb4a4-ae2a-4e27-84c9-00cd88a63719">

## Hybrid Direct/Associative Mapping
- 두 기법을 혼합하여 사용
  - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
  - PMT : 메모리(커널 공간)에 저장
  - TLB : PMT 중 일부 entry들을 적재
    - 최근에 사용된 page들에 대한 entry 저장
- Locality (지역성)활용
  - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근또는 인접 영역을 다시 접근할 가능성이 높음
<img width="690" alt="스크린샷 2024-02-12 오후 6 33 22" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/67927fbd-0262-42cf-bca0-a2897098c7b5">

## Memory Management
- Page와 같은 크기로 미리 분할 하여 관리/사용
  - Page frame
  - FPM 기법과 유사
- Frame table
  - Page frame 당 하나의 entry
- 구성
  - Allocated/available field
  - PID field
  - Link field : For free list(사용가능 한 fp들을 연결)
  - AV : Free list header (free list의 시작점)
- Frame table
<img width="558" alt="스크린샷 2024-02-12 오후 6 38 57" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/c3429048-39a4-45d7-b723-afa8aceffbe3">

## Page Sharing
- 여러 프로세스가 특정 page를 공유 가능
  - Non-continuous allocation
- 공유 가능 page
  - Procedure pages
  - Data page
    - Read-only data
    - Read-write data
      - 병행성 제어 기법 관리하에서만 가능

## Page Protection
- 여러 프로세스가 page를 공유할 때
  - Protection bit 사용
<img width="409" alt="스크린샷 2024-02-12 오후 6 42 16" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d2c4f4ea-82d0-43aa-a7a5-f50ec96a900e">

## Paging System - Summary
- 프로그램을 고정된 크기의 block으로 분할 / 메모리를 block size로 미리 분할
  - 외부 단편화 문제 없음
  - 메모리 통합/압축 불필요
  - 프로그램의 논리적 구조 고려하지 않음
    - Page sharing/protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
  - 메모리의 효율적 활용
- Page mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
    - 하드웨어 비용 증가
