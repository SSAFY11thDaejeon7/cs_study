# [OS] Lecture 8. Memory Management

## 메모리(기억장치)의 종류
<img width="540" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/01b9eac2-e6e4-4794-b6a3-4042ca2aa355">

- 레지스터, 캐시, 메인 메모리, 보조기억장치
  - 왼쪽에 있을 수록 속도가 빠르고 비싸진다.
## 메모리(기억장치) 계층구조
<img width="540" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/4bb0ca60-bd0c-48c0-8166-7903c7f11ee3">

- Block
  - 보조기억장치와 주기억장치 사이의 데이터 전송 단위
  - size: 1 ~ 4KB
- Word
  - 주기억장치와 레지스터 사이의 데이터 전송 단위
  - size: 16 ~ 64 bits

## Address Binding
<img width="329" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/31a76009-0ca1-4de8-b87e-988a610836e5">

- 프로그램의 논리 주소를 실제 메모리의 물리 주소로 매핑(mapping)하는 작업
<img width="580" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/066abcae-c506-4979-a0d6-b4603c7b0d98">

- Binding 시점에 따른 구분
  - Compile time binding
    - 프로세스가 메모리에 적재될 위치를 컴파일러가 알 수 있는 경우
      - 위치가 변하지 않음
    - 프로그램 전체가 메모리에 올라가야 함
  - Load time binding
    
    <img width="542" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/0d4fb9c3-7464-4e68-8c8a-280bb2e21bc0">

    - 메모리 적재 위치를 컴파일 시점에서 모르면, 대체 가능한 상대 주소를 생성
    - 적재 시점에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설정
    - 프로그램 전체가 메모리에 올라가야 함
  - Run time binding
    
    <img width="289" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/481bddfd-898d-4e1f-b476-69a38d4c9018">

    - Address binding을 수행시간까지 연기
      - Address binding을 수행시간까지 연기
        - 프로세스가 수행 도 다른 메모리 위치로 이동할 수 있음
      - HW의 도움이 필요
        - MMU: Memory Management Unit
      - 대부분의 OS가 사용

## Dynamic Loading
- 모든 루틴을 교체 가능한 형태로 디스크에 저장
- 실제 호출 전까지는 루틴을 적재하지 않음
  - 메인 프로그램만 메모리에 적재하여 수행
  - 루틴의 호출 시점에 address binding 수행
- 장점
  - 메모리 공간의 효율적 사용

## Swapping
- 프로세서 할당이 끝나고 수행 완료된 프로세스는 swap-device로 보내고(Swap-out)
- 새롭게 시작하는 프로세스는 메모리에 적재(Swap-in)
  
<img width="555" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/9b170452-731a-42d2-9989-74d217dcae6e">


## Memory Allocation
- Continuous Memory Allocation(연속할당)
  - 프로세스(context)를 하나의 연속된 메모리 공간에 할당하는 정책
    - 프로그램, 데이터, 스택 등
  - 메모리 구성 정책
    - 메모리에 동시에 올라갈 수 있는 프로세스 수
      - Multiprogramming degree
    - 각 프로세스에게 할당되는 메모리 공간 크기
    - 메모리 분할 방법
  - Uni-programming
    - Multiprogramming degree = 1
    - 하나의 프로세스만 메모리 상에 존재
    - 가장 간단한 메모리 관리 기법
      
    <img width="347" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/d0fcfd9e-f7b4-4a60-9c70-c4c1f81d5bf5">

    - 문제점
      1. 프로그램의 크기 > 메모리 크기
      2. 커널 보호
      3. Low system resource utilization, Low system performance
    - 해결법
      1. Overlay structure
        - 메모리에 현재 필요한 영역만 적재
        - 사용자가 프로그램의 흐름 및 자료구조를 모두 알고 있어야함
      2. 경계 레지스터(boundary register) 사용

        <img width="344" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/be41964f-09b4-41fe-85e5-81efb0057696">

      3. Multi-programming
  - Multi-programming
    - Fixed Partition Multiprogramming
      - 메모리 공간을 고정된 크기로 분할
        - 미리 분할되어 있음
      - 각 프로세스는 하나의 partition(분할)에 적재
        - Process : Partition = 1 : 1
      - Partition의 수 = K
        - Multiprogramming degree = K
          
      <img width="253" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/b417a27f-ca79-435b-895f-ffd45463e2c8">
      <img width="370" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/184ceab0-166d-46ea-95e1-bc66ec5f5482">

      - 자료구조의 예
      - 커널 및 사용자 영역 보호
        
      <img width="370" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/306d837f-2a22-4f2b-a503-3f9c5de4a5e7">

      - **Fragmentation(단편화)**
        - Internal fragmentation
          - 내부 단편화
          - Partition 크기 > Prodess 크기
            - 메모리가 낭비됨
              
              <img width="250" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/d2069b2c-8045-4df8-b156-4cf1ef713f06">

        - External fragmentation
          - 외부 단편화
          - (남은 메모리 크기 > Process 크기)지만 연속된 공간이 아님
      - 요약
        - 고정된 크기로 메모리 미리 분할
        - 메모리 관리가 간편함
          - Low overhead
        - 시스템 자원이 낭비될 수 있음
        - Internal/External fragmentation
    - Variable Partition Multiprogramming
      - 초기에는 전체가 하나의 영역
      - 프로세스를 처리하는 과정에서 메모리 공간이 동적으로 분할
      - No internal fragment
      - VPM Example
        - Memory space: 120MB
          
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/eae45011-ed32-42c0-9d8a-93a198862156">
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/2796a6a7-5e61-4e88-9c35-69c3581ff1de">
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/c1afe78b-a4f6-45a6-9554-1fd47bb91aa9">
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/49cde6f2-42d3-4d01-94ae-fa09186acbbe">
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/a99c527b-b27b-411a-9060-84926a9481b5">
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/152f6712-d79e-44b9-80c7-cbf01da7d046">
          <img width="549" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/adaef307-7102-4210-ac6a-0b98d140f7db">

          1. 초기 상태
          2. 프로세스A(20MB)가 적재된 후
          3. 프로세스B(10MB)가 적재된 후
          4. 프로세스C(25MB)가 적재된 후
          5. 프로세스D(20MB)가 적재된 후
          6. 프로세스B가 주기억장치를 반납한 후
          7. 프로세스E(15MB)가 적재된 후
          8. 프로세스D가 주기억장치를 반납한 후
      - 배치 전략 (Placement strategies)
        - First-fit(최초 적합)
          - 충분한 크기를 가진 첫 번째 partition을 선택
          - Simple and low overhead
          - 공간 활용률이 떨어질 수 있음
            
            <img width="321" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/356a00db-b137-4b3b-91ed-b366060e5754">
            
        - Best-fit(최적 적합)
          - Process가 들어갈 수 있는 partition 중 가장 작은 곳 선택
          - 탐색시간이 오래 걸림
            - 모든 partition을 살펴봐야 함
          - 크기가 큰 partition을 유지할 수 있음
          - 작은 크기의 partition이 많이 발생
            - 활용하기 너무 작은
              
              <img width="328" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/57a736f7-aae8-4bb2-a5cf-868b4081f8fe">

        - Worst-fit(최악 적합)
          - Process가 들어갈 수 있는 partition 중 가장 큰 곳 선택
          - 탐색시간이 오래 걸림
            - 모든 partition을 살펴봐야 함
          - 작은 크기의 partition 발생을 줄일 수 있음
          - 큰 크기의 partition 확보가 어려움
            - 큰 프로세스에게 필요한
              
              <img width="312" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/23dd1558-139f-4b59-86f3-7bbcc9c62a3e">

        - Next-fit(순차 최초 적합)
          - 최초 적합 전략과 유사
          - State table에서 마지막으로 탐색한 위치부터 탐색
          - 메모리 영역의 사용 빈도 균등화
          - Low overhead
      - External fragmentation issue
        - Coalescing holes(공간 통합)
          - 인접한 빈 영역을 하나의 partition으로 통합
            - Process가 memory를 release하고 나가면 수행
            - Low overhead
              
              <img width="537" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/0c5ac69a-e090-43fb-b3a1-b8c5046a93c6">

        - Storage Compaction(메모리 압축)
          - 모든 빈 공간을 하나로 통합
          - 프로세스 처리에 필요한 적재 공간 확보가 필요할 때 수행
          - High overhead
            - 모든 Process 재배치 (Process 중지)
            - 많은 시스템 자원을 소비
              
              <img width="537" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/8813bc6d-c429-42da-8081-15c501a937fc">
