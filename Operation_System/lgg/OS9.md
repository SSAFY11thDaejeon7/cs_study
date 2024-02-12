# 9강

# Virtual Memory

**Non-continuous allocation**

사용자 프로그램을 여러 block으로 분할

실행시 필요한 block만 메모리에 적재

나머지 block들은 swap device에 존재.

**Non-continuous allocation 기법**

- paging system
- segmentation system
- hybrid paging/segmentation system

**Address Mapping**

virtual address → real address

**Virtual address(가상주소)**

relative address, logical address

연속된 메모리 할당을 가정한 주소(아직 메모리에 올라가지 않음)

**Real address(실제주소)**

absolute address, physical address

실제 메모리에 적재된 주소

사용자/프로세스는 실행 프로그램 전체가 메모리에 연속적으로 적재되었다고 **가정**하고 실행할 수 있게 된다.

Address Mapping 기법

1. Block Mapping
    
    프로그램을 block 단위로 분할,관리
    
    virtual address : v = (b, d)
    
    b : block number
    
    d : displacement(offset) in a block
    
    Block Map Table(BMT) : address mapping 정보 가짐, kernel 공간에 **프로세스마다 BMT 1개씩 존재.** block number, residence bit, real address 정보 존재
    
    residence bit : 해당 block이 메모리에 적재되었는지 여부.
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/0ead28f4-5d75-43e1-a4d5-26e3bee592b9)
    
    residence bit 1이면 real address 값 계산
    
    0이면 swap device에서 해당 block을 메모리로 가져오고, BMT update
    

**Paging System**

프로그램을 같은 크기의 block으로 분할 기법

page : 프로그램의 분할된 block(가상)

page frame : 메모리(실제)의 분할 영역. page와 같은 크기로 분할

**특징**

- 크기에 따른 분할(논리적 분할이 아님)
    
    그래서 page 공유 및 보호 과정이 복잡하다. → segmentation 등장
    
- simple and efficient
- No external fragmentation. Yes internal fragmentation

**Address Mapping**

virtual address : v = (p,d)

p : page number

d : displacement(offset) in a page

PMT(page map table) 사용

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/af3ceb95-93d9-4f24-8949-19132e28fd0f)

기법

1. Direct Mapping
2. Associative Mapping (TLB 사용)
3. Hybrid direct/associative mapping

**Direct Mapping**

block mapping과 유사

PMT를 커널 안에 저장

entry : PMT 사진에서 한줄에 대한 정보를 entry라고 한다.

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/fa638935-601e-4dc5-810c-2e7d468dd6db)

**page fault : page table을 봤더니 residence bit이 0인경우(메모리에 아직 적재되지 않은 경우)**

context switching이 발생하므로 overhead가 생기게 된다. 따라서 page fault를 최소화 하는 것이 중요함.

**문제점**

메모리 접근 횟수가 2번(page table 보고, 실제 메인 메모리 접근)

PMT를 위한 메모리 공간이 별도로 필요

**해결방안**

Associative mapping (TLB)

PMT를 위한 별도의 전용 저장장치 사용

**Associative Mapping**

**TLB(Translation Look-aside Buffer)** 에 PMT 적재

TLB의 HW적 도움으로 PMT를 병렬로 탐색하게 해주어서 page table을 한번에 빠르게 접근할 수 있게 해줌. low overhead. high speed

하지만 비싸다. 큰 PMT를 다루기 어려움.

**Hybrid direct/associative mapping**

작은 크기의 TLB사용

PMT 여전히 사용 : 메모리(커널)에 저장

TLB : PMT중 일부 entry들만 적재(자주, 최근 상용되는 entry들)

**지역성 Locality 활용**

프로그램의 한번 접근한 영역 다시 접근 (시간적 지역성)

프로그램 인접 영역 다시 접근(공간적 지역성)

TLB 찾고, 없으면 PMT 찾고 없으면 swap device 찾고

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/a08d9588-a4e6-4b5e-a4d1-788fe08ef626)

Non-continuous allocation의 특성상 여러 프로세스가 **특정 page를 공유** 가능하게 됨.

**공유 가능 page**

1. Procedure pages : 함수 등의 특정 기능 수행 코드
2. Data page : 공유 data(write경우 mutual exclusion 필요)

공유 Procedure page에서 문제점 발생

main memory 에서 같은 주소인데 여러 프로세스가 공유하다보니 지칭하는 page number가 다를 수 있다. 혼돈 발생

해결법 : 공유하는 주소는 프로세스들마다 같은 page number를 쓰게함. (프로세스들 각 PMT의 같은 entry에 저장하게 한다.)

공유 data page에서 문제점 발생

read,write할 때 문제점 발생 → protection bit 사용
