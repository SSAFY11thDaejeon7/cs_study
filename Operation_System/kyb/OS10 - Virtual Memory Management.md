## Virtual Memory Management

- 가상 메모리(기억장치)
    - Non-continuous allocation
        - 사용자 프로그램을 block으로 분할하여 적재/실행
    - Paging/Segmentation system
- 가상 메모리 관리의 목적
    - 가상 메모리 시스템 성능 최적화
        - Cost model
        - 다양한 최적화 기법

## Cost Model for Virtual Mem. Sys

- Page fault frequency(발생 빈도)
- Page fault rage(발생률)
- Page fault rate를 최소화 할 수 있도록 전략들을 설계해야 함
    - Context switch 및 Kernel 개입을 최소화
    - 시스템 성능 향상

- Page reference string(d)
    - 프로세스의 수행 중 참조한 페이지 번호 순서
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/09cee06a-8945-4b71-a4f4-86e094177c5c)

    
- Page fault rate = F(w)
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/e2172035-6e3f-42be-bfbb-8df53e09ce61)

    

## Hardware Components

- Address translation device(주소 사상 장치)
    - 주소 사상을 효율적으로 수행하기 위해 사용
- Bit Vectors
    - Page 사용 상황에 대한 정보를 기록하는 비트들
    - Reference bits(참조 비트)
        - page frame이 참조되었는지 기록
    - Update bits(갱신 비트)
        - page frame에 있는 데이터가 갱신 되었는지 기록
        
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/f49b8270-43c9-4155-a889-c18989586e8c)

        

## Software components

- 가상 메모리 성능 향상을 위한 관리 기법들
    - Allocation strategies(할당 기법)
    - Fetch strategies
    - Placement strategies(배치 기법)
    - Replacement strategies(교체 기법)
    - Cleaning strategies(정리 기법)
    - Load control strategies(부하 조절 기법)
    
### Allocation Strategies

- 각 프로세스에게 메모리를 얼마만큼 줄 것인가?
    - 고정 할당(fixed): 프로세스의 실행 동안 고정된 크기의 메모리 할당
    - 가변 할당(Variable): 프로세스의 실행 동안 할당하는 메모리의 크기가 유동적
- 고려 사항
    - 프로세스 실행에 필요한 메모리 양을 예측해야 함
    - 너무 큰 메모리 할당: 메모리가 낭비됨
    - 너무 작은 메모리 할당: Page fault rate 증가, 시스템 성능 저하

  
### Fetch Strategies

- 특정 page를 메모리에 언제 적재할 것인가?
    - Demand fetch(demand paging)
        - 프로세스가 참조하는 page들만 적재
        - Page fault overhead
    - Anticipatory fetch(pre-paging)
        - 참조될 가능성이 높은 page 예측, 해당 page를 미리 적재
        - 예측 성공 시, page fault overhead가 없음
        - 잘못된 예측 시 자원 낭비가 큼
        - Prediction overhead(Kernel의 개입), Hit ratio에 민감함
    
- 실제 대부분의 시스템은 Demand fetch 기법 사용
    - 일반적으로 준수한 성능을 보여 줌
    

  
### Placement Strategies

- Page/segment를 어디에 적재할 것인가?
- Paging system에는 불필요

- Segmemtation system에서의 배치 기법
    - First-fit
    - Best-fit
    - Worst-fit
    - Next-fit

  
### Replacement Strategies

- 빈 page frame이 없는 경우, 새로운 page를 어떤 page와 교체 할 것인가?
    - 고정 할당을 위한 교체 기법
    - 가변 할당을 위한 교체 기법 존재
    

  
### Cleaning Strategies

- 변경 된 page를 언제 write-back 할 것인가?
    - 변경된 내용을 swap device에 반영
    - Demand cleaning
        - 해당 page에 메모리에서 내려올 때 write-back
    - Anticipatory cleaning(pre-cleaning)
        - 더 이상 변경될 가능성이 없다고 판단 할 때, 미리 write-back
        - Page 교체 시 발생하는 write-back 시간 절약
        - Write-back 이후, page 내용이 수정되면 overhead 발생
    
- 실제 대부분의 시스템은 Demand cleaning 기법 사용
    - 일반적으로 준수한 성능을 보여 줌
    - Anticipatory cleaning
        - 잘못된 예측 시 자원 낭비가 큼

  
### Load Control Strategies

- 시스템의 multi-programming degree 조절
    - Allocation strategies와 연계 됨
- 적정 수준의 multi-programming degree를 유지해야 함
    - Plateau(고원) 영역으로 유지
    - 저부하 상태(Under-loaded)
        - 시스템 자원 낭비, 성능 저하
    - 고부하 상태(Over-loaded)
        - 자원에 대한 경쟁 심화, 성능 저하
        - Thrashing: 과도한 page fault가 발생하는 현상
        
        ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/0dc6e330-bf4f-4fe4-892c-ea660ba437ff)


# Replacement Strategies

- 고정 할당 기법
    - MIN(OPT, B0) algorithm
    - Random algorithm
    - FIFO algorithm
    - LRU(Least Recently Used) algorithm
    - LFU(Least Frequently Used) algorithm
    - NUR(Not Used Recently) algorithm
    - Clock algorithm
    - Second chance algorithm
- 가변 할당 기법
    - WS(Working Set) algorithm
    - PFF(Page Fault Frequency) algorithm
    - VMIN(Variable MIN) algorithm

## Min Algorithm(OPT algorithm)

- Minimize page fault frequency(proved)
    - 최적의 솔루션
- **앞으로 가장 오랫동안 참조되지 않을 page 교체**
    - Tie-breaking rule(동률일 때): page 번호가 가장 큰/작은 page 교체
- 실현 불가능한 기법(Unrealizable)
    - Page reference string을 미리 알고 있어야 함
- 교체 기법의 성능 평가 도구로 사용 됨

## Random Algorithm

- 무작위로 교체할 page 선택
- Low overhead
- No policy

## FIFO Algorithm

- First In First Out
    - **가장 오래된 page를 교체**
- page가 적재 된 시간을 기억하고 있어야 함
- 자주 사용되는 page가 교체 될 가능성이 없음
    - Locality에 대한 고려가 없음
- FIFO 이상현상
    - FIFO 알고리즘의 경우, 더 많은 page frame을 할당 받음에도 불구하고
    page falut의 수가 증가하는 경우가 있음
- **예제**
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/3285d7ba-c891-4d39-802e-ccd29d941ee6)

    

## LRU(Least Recently Used) Algorithm

- **가장 오랫동안 참조되지 않은 page를 교체**
- Page 참조 시 마다 시간을 기록해야 함
- Locality에 기반을 둔 교체 기법
- MIN algorithm에 근접한 성능을 보여줌
- 실제로 가장 많이 활용되는 기법
- 단점
    - 참조 시 마다 시간을 기록해야 함(Overhead)
        - 간소화된 정보 수집으로 해소 가능
            - ex) 정확한 시간 대신, 순서만 기록
    - Loop 실행에 필요한 크기보다 작은 수의 page frame이 할당 된 경우,
    page fault 수가 급격히 증가함
        - Allocation 기법에서 해결해야 함
- **예제**
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/a23751b5-2554-422b-b878-2f2e44031116)

    

## LFU(Least Frequently Used) Algorithm

- **가장 참조 횟수가 적은 page를 교체**
    - Tie-breaking rule(동률일 때): LRU
- Page 참조 시 마다, 참조 횟수를 누적 시켜야 함
- Locality 활용
    - LRU 대비 적은 overhead
- 단점
    - 최근 적재된 참조될 가능성이 높은 page가 교체 될 가능성이 있음
    - 참조 횟수 누적 overhead
- **예제**
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/58d0ec84-0597-4fa9-9a9a-873b1586fafe)

    

## NUR(Not Used Recently) Algorithm

- LRU approximation scheme
    - LRU보다 적은 overhead로 비슷한 성능 달성 목적
- Bit vector 사용
    - Reference bit vector(r)
    - Update bit vector(m)
- 교체 순서
    1. r, m = 0, 0
    2. r, m = 0, 1
    3. r, m = 1, 0
    4. r, m = 1, 1
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/13b353e3-26f5-4473-aa7a-231a37c50248)

    

## Clock Algorithm

- IBM VM/370 OS
- Reference bit 사용함
    - 주기적인 초기화 없음
- Page frame들을 순차적으로 가리키는 pointer(시계바늘)를 사용하여 교체될 page 선정
- Pointer를 돌리면서 교체 page 결정
    - 현재 가리키고 있는 page의 reference bit(r) 확인
    - r = 0인 경우, 교체 page로 결정
    - r = 1인 경우, 0으로 초기화 후 pointer 이동
- 먼저 적재된 page가 교체될 가능성이 높음
    - FIFO와 유사
- Reference bit를 사용하여 교체 페이지 결정
    - LRU(or NUR)과 유사

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/111f7b96-c8e3-442d-9c63-82c68fb4fea2)


- **예제**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/97747f55-a453-4c1e-b75e-9bf32145e4ae)


## Second Chance Algorithm

- Clock algorithm과 유사
- Update bit(m)도 함께 고려 함
    - 현재 가리키고 있는 page의 r, m 확인
    - 0 ,0 : 교체 page로 결정
    - 0 ,1 : → 0, 0, write-back(cleaning) list에 추가 후 디오
    - 1, 0 : → 0, 0 후 이동
    - 1, 1 : → 0, 1 후 이동

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/8ee5a058-b849-4646-b0d7-0237c1aac25f)


- **예제**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/a3267419-92dd-44b7-b306-b8250766fb63)


## Other  Algorithm

- Additional-reference-bits algorithm
    - LRU approximation
    - 여러 개의 reference bit를 가짐
        - 각 time-interval에 대한 참조 여부 기록
        - History register for each page
- MRU(Most Recently Used) algorithm
    - LRU와 정반대 기법
- MFU(Most Frequently Used) algorithm
    - LFU와 정반대 기법
