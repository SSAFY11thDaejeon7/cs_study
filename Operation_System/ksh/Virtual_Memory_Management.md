# 가상 메모리 관리
- 가상 메모리(기억장치)
  - Non-continuous allocation
    - 사용자 프로그램을 block으로 분할하여 적재/실행
  - Paging/Segmentation system
- 가상 메모리 관리의 목적
  - 가상 메모리 시스템 성능 최적화
    - Cost model
    - 다양한 최적화 기법

## Cost Model for Virtual Memory System
- Page fault rate를 최소화할 수 있도록 전략들을 설계해야 함
  - Context switch 및 Kernel 개입을 최소화
  - 시스템 성능 향상

- Page reference string
  - 프로세스의 수행 중 참조한 페이지 번호 순서
- Page fault rate = page fault의 수 / 참조했던 페이지의 길이

## Hardware Components
- Address translation device (주소 사상 장치)
  - 주소 사상을 효율적으로 수행하기 위해 사용
- Bit Vectors
  - page 사용 상황에 대한 정보를 기록하는 비트들(참조 비트, 갱신 비트)

- Reference bit vector
  - 메모리에 적재된 각각의 page가 최근에 참조되었는지를 표시
  - Reference 비트를 확인함으로써 최근에 참조된 page들을 확인 가능
- Update bit vector
  - Page가 메모리에 적재된 후, 프로세스에 의해 수정되었는지를 표시
  - 주기적 초기화 없음. 메모리에서 나올 때 초기화함

## Software Components

### Allocation Strategies
- 각 프로세스에게 메모리를 얼마만큼 줄 것인가?
  - 고정 할당: 프로세스의 실행동안 고정된 크기의 메모리 할당
  - 가변 할당: 프로세스의 실행동안 할당하는 메모리의 크기가 유동적

- 고려사항
  - 프로세스 실행에 필요한 메모리 양을 예측해야 함
  - 너무 큰 메모리 할당은 메모리가 낭비됨
  - 너무 적은 메모리 할당은 Page fault rate가 증가하고, 시스템 성능 저하
 
### Fetch Strategies
- 특정 page를 메모리에 언제 적재할 것인가?
  - Demand fetch: 프로세스가 참조하는 페이지들만 적재
  - Anticipatory
    - 참조될 가능성이 높은 page 예측
    - 가까운 미래에 참조될 가능성이 높은 page를 미리 적재
    - 예측 성공 시, page fault overhead가 없음
    - Prediction overhead (Kernel의 개입), Hit ratio에 민감함
- 실제 대부분의 시스템은 Demand fetch 기법 사용
  - 일반적으로 준수한 성능을 보여줌
 
### Placement Strategies
- Page/segment를 어디에 적재할 것인가?
- Paging system에는 불필요

### Cleaning Strategies
- 변경된 page를 언제 write-back 할 것인가?
  - 변경된 내용을 swap device에 반영
  - Demand cleaning
    - 해당 page에 메모리에서 내려올 때 write-back
  - Anticipatory cleaning (pre-cleaning)
    - 더 이상 변경될 가능성이 판단할 때, 미리 write-back
    - Page 교체시 발생하는 write-back 시간 절약
    - Write-back 이후, page 내용이 수정되면 overhead!
- 실제 대부분의 시스템은 Demand cleaning 기법 사용
  - 일반적으로 준수한 성능을 보여줌

### Load Control Strategies
- 시스템의 multi-programming degree 조절
  - Allocation strategies와 연계됨
- 적정 수준의 multi-programming degree를 유지해야 함
  - 고원 영역으로 유지
  - 저부하 상태: 시스템 자원 낭비, 성능 저하
  - 고부하 상태: 자원에 대한 경쟁 심화, 성능 저하
  - Thrashing 현상 발생: 과도한 page fault 발생

