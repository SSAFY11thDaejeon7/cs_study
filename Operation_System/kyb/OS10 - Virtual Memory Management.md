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
    
- 실제 대부분의 시스템은 Demand fetch 기법 사용
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
