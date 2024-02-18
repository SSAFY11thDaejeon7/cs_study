# 가상 메모리 관리

- 가상 메모리 (기억장치)
   - Non-continuous allocation
     - 사용자 프로그램을 block으로 분할하여 적재 및 실행
   - Pageing / Segmentation system
- 가상 메모리 관리의 목적
  - 가상 메모리 시스템 성능 최적화
  - 이를 위해서는 page fault 발생 빈도가 적어야한다.
  - page fault : cpu가 호출하려는 데이터가 메모리에 없는 경우 swap device에서 메모리로 데이터를 올리는 작업

## Hardware Components를 활용한 최적화 방법
- Address translation device (주소 사상 장치)
  - 주소 사상을 효율적으로 수행하여 성능 최적화
  - 이전의 TLB 등을 사용한 방법들
 
- Bit Vectors
  - Page 사용 상황에 대한 정보를 기록하는 비트들
    - Reference bits (참조 비트) : 메모리에 적재된 각각의 page가 최근에 참조됐는지 기록
    - Update bits (갱신 비트) : 메모리에 적재된 각각의 page가 수정됐는지 기록
      - 해당 page가 수정되었다면 swap device에 write-back(변화 내용 재작성)이 필요하다.
<br>

## Software Components를 활용한 최적화 방법
- 가상 메모리 성능 향상을 위한 소프트웨어 관리 기법
  - Allocation strategies(할당 기법)
  - Fetch strategies(메모리 적재 기법)
  - Placement strategies(배치 기법)
  - Replacemenet strategies(교체 기법)
  - Cleaning strategies(정리 기법)
  - Load control strategies(부하 조절 기법)
 <br>

 <h2>1. Allocation strategies</h2>
 
    - 각 프로세스에게 메모리를 얼마나 할당할 것인가
    - 기법
      - Fixed allocation(고정 할당) : 프로세스의 실행 동안 고정된 크기의 메모리 할당
      - Variable allocation(가변 할당) : 프로세스의 실행 동안 할당하는 메모리의 크기가 유동적
    - 고려사항
      - 프로세스 실행에 필요한 메모리 양을 예측해야 함
      - 너무 큰 메모리 할당은 메모리가 낭비
      - 너무 적은 메모리 할당은 page fault rate 상승으로 시스템 성능 저하 야기
<br>

<h2>2. Fetch Strategies</h2>

- 특정 page를 메모리에 언제 적재할 것인가?
  - Demand fetch(demanding paging) : 프로세스가 참조하는 페이지들만 적재
  - Anticipatory fetch(pre-paging) : 참조될 가능성이 높은 page 예측하여 미리 적재
    - 예측 성공 시, page fault가 거의 없어 성능 향상
    - 단, 예측 overhead가 크고, 예측 실패시 자원 낭비 발생
- 실제 대부분의 시스템은 Demand fetch 기법을 사용
  - 일반적으로 준수한 성능을 보여주며, Anticipatory fetch는 잘못된 예측 시 자원 낭비가 크기에 잘 사용안함
<br>

<h2>3. Placement Strategis</h2>

- page/segment를 어디에 적재할 것인가?
- Paging system에는 불필요(차례대로 적재할 것이기 때문이다.)
- Segmentation system에서의 배치 기법
  - First fit
  - Best fit
  - Worst fit
  - Next fit
<br>

<h2>4. Replacements Strategis</h2>

- 빈 page frame이 없는 경우 새로운 page를 어떤 page와 교체할 것인가?
  - Fixed allocation을 위한 교체 기법
  - Variable allocation을 위한 교체 기법
  - 위 2가지로 나뉘며, 각각의 알고리즘은 하단에 자세히 설명하겠음
<br>

<h2>5. Cleaning Strategies</h2>

- 변경된 page를 언제 write-back할 것인가?
  - write-back : 변경된 내용을 swap device에 반영하는 것
  - Demand cleaning : 해당 page에 메모리에서 내려올 때 write back함
  - Anticipatory cleaning(pre-cleaning) : 더 이상 변경된 가능성이 없다고 판단할 때, 미리 write back
    - Write back 이후, page 내용 수정이 없으면 page 교체 시 발생하는 write-back 시간이 절약됨.
    - Write back 이후, page 내용이 수젇되면 overehead 발생
- 실제 대부분의 시스템은 Demnad cleaning 기법 사용
  - 일반적으로 준수한 성능 보여줌
  - Anticipatory cleaning : 잘못된 예측시 자원 낭비가 크기 때문임
 <br>

 <h2>6. Load Control Strategies</h2>
 
- 시스템의 multi-programming degree 조절
  - Allocation strategies와 연계됨
- 적정 수준의 multi-programming degree를 유지해야 함
  - Plateau(고원) 영역으로 유지
  - 저부하 상태 (Under-loaded) : 시스템 자원 낭비, 성능 저하
  - 고부하 상태 (Over-loaded) : 자원에 대한 경쟁 심화, 성능 저하
    - Thrashing(스레싱) 현상 발생 : 과도한 page fault가 발생하여 성능이 저하되는 현상
  ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/bd5d90af-2089-4ed7-bc27-ada9df27eb15)
