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




**Segmentation System**

프로그램을 **논리적 block** 으로 분할

page system과 달리 block 크기가 달라질 수 있다.

**특징**

메모리를 미리 분할해놓지 않는다. (VPM과 유사)
논리적인 block이기에 segment sharing/protection이 용이함.

하지만 그만큼 address mapping 및 메모리 관리의 overhead가 큼.

No internal fragmentation(짤라서 주기 때문에)

yes external fragmentation

**Address Mapping (paging system과 유사)**

virtual address : v = (s, d)

s : segment number

d : displacement(offset) in a segment

SMT(segment map table)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/c595e5b9-6eb4-45db-8648-b58c5733a6ac)

segment length : segment는 길이가 가변이므로 length 정보 추가

protection bits : segment를 보호하기 위함.

**Direct Mapping**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/ecd1df6e-af63-49f7-bb5b-0cbd9cbbe820)

1. residence bit 0인 경우 : segmentation fault. swap device로부터 segment 메모리 적재, SMT update
2. 변위 d가 ls (segment length)보다 큰 경우, segment overflow exception 처리
3. 허가되지 않은 연산(protection bit확인)일 경우, segment protection exception 처리

**segment sharing / protection**

논리적으로 분할되어 있으므로, 공유 및 보호가 용이하다.

**segmentation 장점**

프로그램을 논리 단위로 분할, 메모리를 동적으로 분할하기 때문에

internal fragmentation 없다.

segment sharing / protection 용이하다.

**segmentation 단점**

paging system 대비 overhead가 크다.

paging과 유사하게 SMT 메모리 공간과 추가적인 메모리 접근이 필요하다. → TLB같은 HW로 개선

**Hybrid paging / segmentation system**

paging, segmentation 장점 결합

프로그램 분할 : 논리적인 단위 segment로 분할

메모리 적재 : 각 segment를 고정된 크기의 page로 분할해서 메모리에 적재함.

**Address mapping**

virtual address : v = (s, p, d)

s : segment number

p : page number

d : offset in a page

SMT, PMT 둘다 사용

각 프로세스마다 하나의 SMT

각 segment마다 하나의 PMT

**SMT**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/8f5ead88-2fe0-459a-9be1-4955a468790d)

hybrid SMT에는 residence bit가 없다. → 실제 메모리에 올라가는 것은 page단위이기 때문에 SMT에는 residence bit가 없음.

**PMT**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/5b9fc6ee-09b4-4fac-83bc-b55f481f02d0)

PMT는 residence bit 존재

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/75887cf5-a92c-4d73-8ee1-ad3567d0329e)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/ecc8e95a-4cc5-4923-8800-25a723c993b7)

메모리 3번을 접근하게 되지만, 그만큼 장점이 더 많기 때문에 사용한다.

**hybrid paging / segmentation 장점**

논리적 분할 segment 와 고정크기 분할 paging 결합

page sharing, protection 용이함.(segmentation)

메모리 관리,할당 overhead 작다.(paging)

no external fragmentation, yes internal fragmentation(paging)

**hybrid paging / segmentation 단점**

메모리 소모가 크다.

address mapping 과정이 복잡하다.

direct mapping의 경우 위 그림같이 메모리 접근 3번하게 된다.
