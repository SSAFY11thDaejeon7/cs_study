# Memory Management

## 
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/9508d986-ec10-43a9-8ff9-7fb2fee641ac)
<br>

- Block : 보조기억장치와 주기억장치 사이의 데이터 전송 단위 (1~4KB)
- Work : 주기억장치와 레지스터 사이의 데이터 전송 단위 (16~64bits)
<br>

## Addrss Binding
- 프로그램의 논리 주소를 실제 메모리의 물리 주소로 매핑하는 작업
- Binding 시점에 따른 구분
  - Compile time binding : Compiler에서의 과정
  - Load time binding : Linker부터 Loader 까지의 과정
  - Run time binding : 프로세스가 메모리에 올라간 뒤 실행시간에 바인딩
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5a7d2bc2-3f89-44d1-b441-4e2da657bf1b)
<br>

<h3>바인딩 방식</h3>
  
1. Compile time binding
  - 프로세스가 메모리에 적재될 위치를 컴파일러가 알 수 있는 경우
    - 위치가 변하지 않음
  - 프로그램 전체가 메모리에 올라가야 함
<br>

2. Load time binding
   - 메모리 적재 위치를 컴파일 시점에서 모르면, 대체 가능한 삳대 주소를 생성
   - 적재 시점에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설정
   - 프로그램 전체가 메모리에 올라가야 함
<br>

3. Run time binding
  - Address binding을 수행시간까지 연기
     - 프로세스가 수행 도중 다른 메모리 위치로 이동할 수 있음
  - HW의 도움이 필요
  - 대부분의 OS가 채택하는 방식
<br>

## 용어 정리
1. Dynamic Loading
  - 모든 루틴을 교체 가능한 형태로 디스크에 저장
  - 실제 호출 전까지는 루틴을 적재하지 않음
    - 메인 프로그램만 메모리에 적재하여 수행
    - 루틴의 호출 시점에 address binding 수행
  - 메모리 공간의 효율적 사용 가능
<br>

2. Swapping
  - 프로세서 할당이 끝나고 수행 완료 된 프로세스는 swap-device로 보내고(Swap-out)
  - 새롭게 시작하는 프로세스는 메모리에 적재(Swap-in)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/7dda2842-67ea-4912-9fdb-af42d6019789)
<br>

## Memory Allocation
- Continuous Memory Allocation
- Non-continuous Memory Allocation
<br>

## Continuous Memory Allocation
  - 프로세스(context)를 하나의 연속된 메모리 공간에 할당하는 정책
    - 프로그램, 데이터, 스택 등
  - 메모리 구성 정책
    - 메모리에 동시에 올라갈 수 있는 프로세스 수
    - 각 프로세스에게 할당되는 메모리 공간 크기
    - 메모리 분할 방법
  - 종류
    - Uni-Programming
      - Multiprogramming degree = 1 ( degree : 메모리에 올린 프로그램의 수를 뜻함 )
    - Multi-Programming
      - Fixed(static) partition multi-programming(FPM) - 고정 분할
      - Variable(dynamic) partition multi-programming(VPM) - 가변 분할

<h3>Uni Programming</h3>

  - 하나의 프로세스만 메모리 상에 존재
  - 가장 간단한 메모리 관리 기법
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/38ff5d79-b4c1-4252-b64e-de72cb10a5a2)

  - 문제점1 : 프로그램의 크기 > 메모리 크기 인 경우 메모리에 프로그램을 올릴 수 없음
  - 해결법 : Overlay structure
    - 메모리에 현재 필요한 영역만 적재
    - 사용자가 프로그램의 흐름 및 자료구조를 모두 알고 있어야 함
  - 문제점2 : 메모리에 올라가 있는 커널이 침법받을 수 있음
  - 해결법 : 경계 레지스터를 사용하여 커널의 영역에 프로세스 접근을 제한함
  - 문제점3 : 메모리에 프로그램을 하나만 올리기 때문에 자원의 활용도와 성능이 떨어짐
  - 해결법 : Multi-programmming 활용
<br>

<h3>Multi-Programming</h3>

1. Fixed Partition Multiprogramming(FPM)
  - 메모리 공간을 고정된 크기로 분할
    - 미리 분할되어 있음
  - 각 프로세스는 하나의 partition에 적재
    - Process : Partition = 1:1
  - Partition의 수  = K
    - Multiprogramming degree = K
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/be372967-e0a9-4463-9e4d-2f5c2e937efc)
  - 커널 및 사용자 영역 보호를 위해 경계 레지스터를 사용하여 커널 및 파티션들이 침범받는 것을 막음

<h3> Fragmentation 단편화 </h3>

- Internal fragmentation
  - 내부 단편화
  - 'Partition 크기 > Process 크기'인 경우 발생, 메모리가 낭비됨
<br>

- External fragmentation
  - 외부 단편화
  - '남은 총 메모리 크기 > Process 크기' 이지만, 연속된 공간이 아니라 프로그램을 못 올림
    - 메모리 낭비 발생
<br>

- FPM 요약
  - 고정된 크기로 메모리 미리 분할
  - 메모리 관리가 간편함
  - 시스템 자원이 낭비 될 수 있음
  - Internal/external fragmentation 발생
<br>

2. Variable Partition Multiprogramming(VPM)
   - 초기에는 전체가 하나의 영역
   - 프로세스를 처리하는 과정에서 메모리 공간이 동적으로 분할됨
   - No internal fragmentation
<br>

<h3>배치 전략(Placement strategies)</h3>
- First-fit (최초 적합)
  - 충분한 크기를 가진 첫 번째 partition을 선택
  - Simple and low overhead
  - 공간 활용률이 떨어질 수 있음
<br>

- Best-fit (최적 적합)
  - Process가 들어갈 수 있는 partition 중 가장 작은 곳 선택
  - 탐색시간이 오래 걸림
    - 모든 partition을 살펴봐야 함
  - 크기가 큰 partition을 유지할 수 있음
  - 활용하기에는 너무 작은 크기의 partition이 많이 발생
  <br>
  
- Worst-fit (최악 적합)
  - Process가 들어갈 수 있는 partition 중 가장 큰 곳 선택
  - 모든 partition을 살펴봐야해서 탐색시간이 오래 걸림
  - 작은 크기의 partition 발생을 줄일 수 있음
  - 큰 크기의 partition 확보가 어려움
<br>

- Next-fit (순차 최초 적합)
  - 최초 적합 전략과 유사
  - State table에서 마지막으로 탐색한 위치부터 탐색 시작
  - 메모리 영역의 사용 빈도 균등화
  - overhead가 적음
<br>

<h3>VPM에서의 External fragmentation 문제 해결법</h3>

1. Coalescing holes (공간 통합)
  - 인전합 빈 영역을 하나의 partition으로 통합
    - Process가 memory를 release하고 나가면 수행
    - Low overhead
<br>

2. Storage Compaction (메모리 압축)
   - 모든 빈 공간을 하나로 통합
   - 프로세스 처리에 필요한 적재 공간 확보가 필요할 때 수행
   - 프로세스 재배치 시, 프로세스가 중지되며 많은 시스템 자원을 소비해 overhead가 크다
