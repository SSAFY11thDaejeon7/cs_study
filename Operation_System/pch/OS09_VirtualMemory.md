# Virtual Memory (가상 메모리)

## Virtual Storage Memory
- Non-continuous allocation
- 사용자 프로그램을 여러 개의 block으로 분할
- 실행 시, 필요한 block들만 메모리에 적재
  - 나머지 block들은 swap device에 존재(Disk)
  - 따라서 메모리 사용 효율이 좋음
- 가상 메모리 기법들
  - Paging system
  - Segmentation system
  - Hybrid paging/segmentation system
<br>

## Address Mapping
- Continuous allocation
  - Relative address(상대 주소)
    - 프로그램의 시작 주소를 0으로 가정한 주소
  - Relocation (재배치)
    - 메모리 할당 후, 할당된 주소에 따라 상대 주소들을 조정하는 작업
![image](https://github.com/Chaeros/NFTStagram/assets/91451735/b900ce47-c2dc-40a9-bfa8-8a04d268c6cb)

- 상대주소에서 실제 물리주소 400을 더하여 재배치된 모습을 확인할 수 있다.
- Non-countinuous allocation
  - Virtual address(가상주소) = relative address
    - Logical address(논리주소)
    - 연속된 메모리 할당을 가정한 주소 (실제 메모리에는 연속적으로 할당되어 있지않음)
  - Real address(실제주소) = absolute(physical)
    - 실제 메모리에 적재된 주소
  - Address mapping : 가상주소를 실제 물리주소로 치환시켜주는 작업
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/9da2e764-0cb0-4c28-88e8-b1be373f73c8)


## Block Mapping
- 사용자 프로그램을 block 단위로 분할/관리하는 Address Mapping 기법
  - 각 block에 대한 address mapping 정보를 유지
- Vritual address : v = (b,d)
  - b = block number
  - d = displacement(offset) in block
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/2608e5db-cddc-458a-8313-378b3a1500a4)

- Block map table(BMT)
  - Address mapping 정보 관리
    - Kernel 공간에 프로세스마다 하나의 BMT를 가짐
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/f95e9aa7-d6ba-4d77-b977-33a31a788e68)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/90213066-61aa-40b0-8f45-69d307b66d93)

- Block Mapping 과정
  - 프로세스의 BMT에 접근
  - BMT에서 block b에 대한 항목(entry)를 찾음
  - Residence bit 검사
    - Residence bit = 0 인 경우, swap device에서 해당 블록을 메모리로 가져옴, 아래 bit = 1 인 과정 수행
    - Redidence bit = 1 인 경우, BMT에서 b에 대한 real address 값 a 확인
  - 실제 주소 r 계산 ( r = a + d )
  - r을 이용하여 메모리에 접근

## Paging system
- 프로그램을 같은 크기의 블록으로 분할(Pages)하는 가상 메모리 기법
- Terminologies(용어들)
  - Page : 프로그램의 분할된 block
  - Page frame : 메모리의 분할 영역, Page와 같은 크기로 분할
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/d6992a0f-7623-417a-98da-24667841e417)

- 특징
  - 논리적 분할이 아님 (크기에 따른 분할)
    - Segmentation 대비 Page 공유 및 보호 과정이 복잡함
  - Segmentation 대비 간단하고 효율적임
    - 모두 같은 크기의 페이지로 나누어 사용하기 때문
  - External fragmentation 발생 x, Internal fragmentation 발생 가능
 
<h2>Paging system에서의 Address Mapping 기법</h2>

- Virtual address : v = (p,d)
   - p : page number
   - d : displacement(offset)
- Address mapping
  - PMT(Page Map Table) 사용
- Address mapping mechanism
  - Direct mapping (직접 사상)
  - Associative mapping (연관 사상)
  - Hybrid direct/associative mapping
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/e36763c7-a7e8-4bb2-a45f-35ee614da6d5)

1. Direct mapping
   - Block mapping 방법과 유사
   - 가정
     - PMT를 커널 안에 저장
     - PMT entry size = entrySize
     - Page size = pageSize
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/be71d9b6-05f6-4326-a2d9-076267fdcbc7)

- Direct Mapping 과정
  - 해당 프로세스의 PMT가 저장되어 있는 주소 b에 접근
  - PMT에서 page p에 대한 항목(entry)을 찾음
  - Residence bit 검사
    - Residence bit = 0 인 경우, swap device에서 해당 page를 메모리로 가져옴, PMT 갱신 후 아래 bit = 1 인 과정 수행
    - Redidence bit = 1 인 경우, 해당 entry에서 page frame 번호 p'를 확인
  - p'와 가상 주소의 변위 d를 사용하여 실제 주소 r 형성 ( r = p` * pageSize + d )
  - 실제 주소 r로 주기적장치에 접근
<br>

- 문제점
   - 메모리 접근 회수가 2배 -> 성능 저하
   - PMT를 위한 메모리 공간 필요
- 해결방안
  - Associative mapping(TLB) 기법 사용
  - PMT를 위한 전용 기억장치(공간) 사용
 
2. Associative Mapping
   - TLB(Translation Look-aside Buffer)에 PMT 적재
   - PMT를 병렬 탐색
   - Low overhead, high spead
   - 비싼 하드웨어로 큰 PMT를 다루기가 어려움
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/48bc03ff-a9f1-4410-93e6-a3f66ab546d5)

3. Hybrid Direct / Associative Mapping
  - 두 기법을 혼합하여 사용
    - HW 비용은 줄이고, Associative mapping의 장점 활용
  - 작은 크기의 TLB 사용
    - PMT : 메모리(커널 공간)에 저장
    - TLB : PMT 중 일부 entry들을 적재
      - 최근에 사용된 page들에 대한 entry 저장
  - Locality (지역성) 활용
    - 프로그램의 수행과정에서 한번 접근한 영역을 다시 접근(temporal locality) 또는 인접 영역을 다시 접근(spatial locality)할 가능성이 높음
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5dde3b14-5e70-4aaa-9c5b-ae5755ba8ff5)

- Hybrid Direct / Associative Mapping 과정
  - 프로세스의 PMT가 TLB에 적재되어 있는지 확인
    - TLB에 적재되어 있는 경우, residence bit를 검사하고 page frame 번호 확인
  - TLB에 적재되어 있지 않은 경우
    - Direct mapping으로 page frame 번호 확인
    - 해당 PMT entry를 TLB에 적재함
<br>

## Paging system의 메모리 관리 기법
- Page와 같은 크기로 미리 분할하여 관리 및 사용
- Frame table
  - Page frame당 하나의 entry
  - 구성
    - Allocated/available field
    - PID field
    - Link field : 사용 가능한 fp들을 연결
    - AV : free list의 시작점 가리킴
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/81c0c687-df6b-45d2-aa8c-27db39d9b7b4)

<h2>Page Sharing</h2>

- 여러 프로세스가 특정 page를 공유 가능
  - Non-continnuous allocation이기 때문
- 공유 가능한 페이지
  - Procedure pages
    - 순수 코드 영역
    - 코드 페이지 공유시, shared page에 대한 정보를, 각 프로세스들의 PMT에 동일한 entry명을 사용해야한다.
    - 동일 entry명을 사용하지 않으면, 같은 코드를 사용하려고 하면서 다른 주소의 값을 불러와 서로 다른 코드를 참조하는 문제가 발생함
  - Data page
    - Read-only data는 상관없으나 Read-write data는 병행성 제어 기법 관리하에서만 가능하다
    - protection bit를 사용하여, 페이지에 대한 접근 권한을 설정하고, 필요시 병행성 제어를 적용한다.
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/352c364a-efc4-4cac-9342-42e26024c191)
<br>

## Paging system 요약
- 프로그램을 고정된 크기의 block으로 분할(page)
- 메로리를 block size로 미리 분할(page frame)
  - 외부 단편화 문제 없음
  - 메모리 통합 및 압출 불필요
  - 프로그램의 논리적 구조 고려하지 않음
    - Page sharing/protection이 복잡
- 필요한 page만 page frame에 적재하여 사용
  - 메로리의 효율적 활용
- Page mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
     - 하드웨어 비용 증가

## Segmentation System
- 프로그램을 논리적 block으로 분할
  - Block의 크기가 서로 다를 수 있음
- 특징
  - 메모리를 미리 분할하지 않음
  - 논리적으로 segment를 나누었기 때문에 Segment sharing/protection이 용이함
  - 메모리 관리 overhead가 큼
  - 내부 단편화 발생x, 외부 단편화는 발생 가능
 ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/a1d027da-65b9-4c2b-800c-bad469a0d332)
<br>

## Segmetation system Address mapping
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5b9539d1-8553-4a09-a735-1ef2fef6d442)
- 프로세스의 SMT가 저장되어 있는 주소 b에 접근
- SMT에서 segment s의 entity를 찾음 ( s entity = b + s * entrySize )
- 찾아진 ENtry에 대해 다음 단계 수행
  - 존재 비트가 0인 경우 : swap device로부터 해당 segment를 메모리로 적재, SMT 갱신
  - 변위(d)가 segment 길이보다 큰 경우 segment overflow exception 호출
  - 허가되지 않은 연산일 경우 segment  protection exception 호출
- 실제 주소 r 계산(r = a +d)
- r로 메모리에 접근
<br>

## Segment sharing/protection
- 논리적으로 분할되어 있어, 공유 및 보호가 용이함
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/c9bea866-21fb-49a9-b938-223f3f713a3c)

## Segmentation System summary
- 프로그램을 논리 단위로 분할/ 메모리를 동적으로 분할
  - 내부 단편화 문제 없음
  - Segment sharing/protection이 용이함
  - Paging system 대비 관리 overhead가 큼
- 필요한 segmentㅁㄴ 메모리에 적재하여 사용
- Segment mapping overhead
  - 메모리 공강 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
 
## Paging vs Segmentation
1. Paging system
   - 간단하고 적은 overhead
   - 논리적이지 않은 분할로 페이지를 공유하고 보호하는 과정이 복잡하다
2. Segmentation system
   - 높은 관리 overhead
   - 논리적인 분할로 페이지를 공유하고 보호하는 과정이 간단하다.
  
## Hybrid Paging/Segmentation
- Paging과 Segmentation의 장점 결합
- 프로그램 분할
  - 논리 단위의 segment로 분할
  - 각 segment를 고정된 크기의 page들로 분할
- Page 단위로 메모리에 적재
- SMT와 PMT 모두 사용
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/e6eddb8d-1c1e-45b6-af0b-b5eb2640bf1c)

- Summary
  - 논리적 분할(segment)와 고정 크기 분할(page)을 결합
    - page sharing/protection이 쉬움
    - 메모리 할당/관리 overhead가 적음
    - 외부 단편화 x, 내부 단편화 가능
  - 전체 테이블 수 증가
    - 메모리 소모 큼
    - Address mapping과정 복잡
  - Direct mapping의 경우 메모리 접근 3배
    - 성능 저하될 수 있음
