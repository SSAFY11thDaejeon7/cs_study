# 8강

# Memory

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/11431e44-44ba-493b-8f26-1ce95d4fdcc7)

- 레지스터, 캐시 : HW가 관리
- 메인 메모리, 보조기억장치 : SW가 관리

**Block, Word 개념**

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/5c0932f3-2939-4975-ae78-84f5c05ed70b)

**Address Binding**

프로그램의 논리 주소를 메모리 실제 물리주소로 mapping 하는 작업

- Compile time Binding
- Load time Binding
- Run time Binding

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/857a9632-f54c-4a84-a938-882742106e59)

**Compile Time Binding**

프로세스가 메모리에 적재될 위치를 **컴파일러**가 알고 있는 경우

위치가 변하지 않음. 프로그램 전체가 메모리에 올라가야함.

**Load Time Binding**

컴파일 시점에서 모르니까, 대체 가능한 상대주소를 작성

메모리 적재 시점(load time)에 시작 주소를 반영하여 사용자 코드상의 주소를 재설정

프로그램 전체가 메모리에 올라가야함

Dynamic loading

모든 루틴을 교체가능한 형태(함수처럼 생각)로 디스크에 저장

실제 호출전까지는 해당 루틴을 메모리에 loading하지 않음

루틴의 호출 시점에 address binding 수행(run time binding)

메모리 공간을 효율적으로 사용할 수 있음

**Run Time Binding**

address binding을 수행시간까지 연기 → 프로세스가 수행 도중 다른 메모리 위치로 이동할 수 있음

HW의 도움이 필요 (MMU : Memory Management Unit)

현재 대부분의 OS가 사용

swap

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/48beb6aa-8f58-4a41-8f52-b8755fa83bf8)

프로세서 할당이 끝나고 수행 완료된 프로세스는 swap device로 보내고

새롭게 시작하는 프로세스는 메모리에 적재

### Memory Allocation

- Continuous Memory Allocation (연속 할당)
    
    uni-programming, multi-programming(FPM,VPM)
    
- Non-continuous Memory Allocation (비연속 할당)

**Continuous Memory Allocation (연속 할당)**

프로세스를 하나의 연속된 메모리 공간에 할당하는 정책

고려해야하는 메모리 구성 정책

메모리에 동시에 올라갈 수 있는 프로세스 수 (multi-programming degree고려)

각 프로세스에게 할당되는 메모리 공간 크기

메모리 분할 방법

- uni-programming
    
    하나의 프로세스만 메모리 상에 존재
    
    **문제점**
    
    프로그램 크기 > 메모리 크기
    
    **해결법**
    
    overlay structure로 공통된 부분은 빼고, 현재 필요한 영역만 메모리에 적재
    
    but 사용자가 프로그램 흐름 및 자료구조를 모두 알고 있어야함
    
    **문제점**
    
    Kernel 을 보호해야함. Kernel 영역 침범해서 메모리 할당하면 안됨
    
    **해결법**
    
    kernel 영역 밑에 boundary register를 놓아서 kernel 경계를 표시
    
- multi-programming
    1. FPM(Fixed Partition Multiprogramming)
        
        메모리 공간을 고정된 크기로 분할 (partition table관리)
        
        미리 분할되어 있고 각 프로세스는 하나의 분할(partition)에 적재
        
        partition의 수 : k 라면 multiprogramming degree : k이다.
        
        **Fragmentation(단편화)**
        
        - Internal fragmentation (내부 단편화)
            
            partition의 크기 > process의 크기 일때 발생. 그만큼 메모리 낭비
            
        - External fragmentation (외부 단편화)
            
            남은 메모리 크기 > process의 크기. **하지만 연속된 공간이 아닐때.** 메모리 낭비
            
    2. VPM(Variable Partition Multiprogramming)
        
        프로세스를 처리하는 과정에서 메모리 공간이 동적으로 분할
        
        그래서 internal fragmentation이 발생하지 않는다.
        
        프로세스들이 종료되어서 남은 공간들이 여러 공간에 존재할 때, 어디에 배치할 것인가?에 대한 문제
        
        **배치 전략**
        
        - First-fit(최초 적합)
            
            충분한 크기를 가진 첫번째 partition을 만나면 바로 배치
            
            simple, low overhead but 공간 활용률 떨어짐
            
        - Best-fit(최적 적합)
            
            프로세스가 들어갈 수 있는 partition중 가장 작은 곳을 선택(최적)
            
            모든 partition을 다 살펴봐야 하므로 탐색 시간 오래걸림
            
            크기가 큰 partition들을 유지할 수 있지만 그만큼 작은 partition도 많이 생기는 문제 발생
            
        - Worst-fit(최악 적합)
            
            프로세스가 들어갈 수 있는 partition 중 가장 큰 곳 선택
            
            탐색 오래걸림. 작은 크기 partition을 줄일 수 있지만 그만큼 큰 규모의 partition 확보 어려워지게 됨.
            
        - Next-fit(순차 최초 적합)
            
            first-fit과 비슷하지만, 맨 앞부터 살피는게 아니라 직전 할당한 위치 다음에서 찾아서 first-fit 적용 → 메모리 영역 사용 균등화
            
        
        **VPM 해결방안**
        
        - Coalescing holes(공간 통합)
            
            **인접한** 빈 영역을 하나의 partition으로 합침. 통합. (process가 작업 마치고 memory를 release하고 나갈때마다 수행)
            
        - Storage Compilation(메모리 압축)
            
            **모든** 빈 공간을 하나로 통합
            
            overhead 크다는 단점. 모든 process를 재배치하는 것은 중지 및 재배치.
