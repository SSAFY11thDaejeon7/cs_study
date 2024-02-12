# 가상 메모리
- Non-continuous allocation
- 사용자 프로그램을 여러개의 blcok으로 분할
- 실행 시, 필요한 block들만 메모리에 적재하고, 나머지 block들은 swap device에 존재
- 기법들: Paging system, Segmentation system, Hybrid paging/segmentation system

## Address Mapping
- Non-continuous allocation
  - 가상 주소 = relative address
    - 논리 주소
    - 연속된 메모리 할당을 가정한 주소
  - 실제 주소 = absolute (physical)
    - 실제 메모리에 적재된 주소
  - Vitual address를 real address로 바꿔주는 것을 주소 매핑이라고 함
 
## Blcok Mapping
- 사용자 프로그램을 block 단위로 분할/관리하는 주소 매핑 기법
  - 각 block에 대한 주소 매핑 정보 유지

![12일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/a1ea4f6e-e265-4213-b8c6-919409ae0d71)

- Block map table (BMT)
  - 주소 매핑 정보 관리
  - Kernel 공간에 프로세스마다 하나의 BMT를 가짐

![12일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/9c9b0708-07fb-41e9-9f77-b53dffda597a)

- 과정
1. 프로세스의 BMT에 접근
2. BMT에서 block b에 대한 항목(entry)를 찾음
3. Resudence bit 검사
   - Residence bit = 0인 경우, swap device에서 해당 블록을 메모리로 가져옴
   - Residence bit = 1인 경우, BMT에서 b에 대한 real address값 a 확인
4. 실제 주소 r 계싼 (r = a + d)
5. r을 이용하여 메모리에 접근

## Paging System
- 프로그램을 같은 크기의 블록으로 분할(Pages)
- Page: 프로그램의 분할된 blcok
- Page frame: 메모리의 분할 영역, Page와 같은 크기로 분할
- 특징
  - 논리적 분할이 아님 (크기에 따른 분할)
    - Page 공유 및 보호 과정이 복잡합 -> Segmentation 대비
  - Simple and Efficient -> Segmentation 대비
  - No external framgmentation - Internal fragmentation 발생 가능

## Address Mapping
- Page Map Table(PMT) 사용
- Direct mapping (직접 사상)
- Associative mapping (연관 사상)

## Direct mapping
- Block mapping 방법과 유사함
- PMT를 커널 안에 저장한다고 가정

![12일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/111f0a86-f8b4-42a2-98f0-425178a5e63b)

1. 해당 프로세스의 PMR가 저장되어 있는 주소 b에 접근
2. 해당 PMT에서 page p에 대한 entry 찾음
3. 찾아진 entry의 존재 비트 검사
- Residence bit = 0인 경우 **(page fault)** , swap device에서 해당 page를 메모리로 적재
  - Context switcing 발생, Overhead가 큼
- Residence bit = 1인 경우, 해당 entry에서 page frame 번호 p'를 확인
4. p'와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성
5. 실제 주소 r로 주기억장치에 접근

문제점
- 메모리 접근 횟수가 2배: 성능 저하 발생 가능성 있음
- PMT를 위한 메모리 공간 필요

해결 방안
- Associative mapping (TLB)
- PMT를 위한 전용 기억장치(공간) 사용

## Associative Mapping
- TLB(Translation Look-aside Buffer)에 PMT 적재
- PMT를 병렬 탐색
- Overhead가 적고, speed가 빠르다
- Expensive hardware: 큰 PMT를 다루기가 어려움

![12일차-4](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/5940f894-0d09-4a89-b675-a6f2159ee043)

## Hybrid Direct/Associative Mapping
- 두 기법을 혼합하여 사용
  - HW 비용은 줄이고, Associative mapping의 장점 활용
- 작은 크기의 TLB 사용
  - PMT: 메모리(커널 공간)에 저장
  - TLB: PMT중 일부 entry들을 적재
    - 최근에 사용된 page들에 대한 entry 저장
  - Locality(지역성) 활용
    - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근 또는 인접 영역을 다시 접근할 가능성이 높음

과정
- 프로세스의 PMT가 TLB에 적재되어있는지 확인하는 과정이 추가됨

![12일차-5](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/d0abc88a-8ed3-4a06-a69e-f4d6992676de)

## Memory Management
- Page와 같은 크기로 미리 분할하여 관리/사용
  - FPM 기법과 유사함
- Frame table
  - Page frame당 하나의 entry를 가짐

## Page Sharing
- 여러 프로세스가 특정 page를 공유 가능함
  - Non-continuous allocation
- 공유 가능 page
  - Procedure pages: 순수 코드
  - Data page: 읽기 전용, 읽기-쓰기 데이터(병행성 제어 기법 관리하에서만 가능)
 
- Procedure Page Sharing 문제
  - 프로세스들이 shared page에 대한 정보를 PMT의 같은 entry에 저장하도록 함

## Page Protection
- 여러 프로세스가 page를 공유할 때, Protection bit 사용
