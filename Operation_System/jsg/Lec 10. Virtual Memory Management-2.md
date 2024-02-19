# Locality
- 프로세스가 프로그램/데이터의 특정 영역을 집중적으로 참조하는 현상
- 원인
  - Loop structure in program
  - Array, structure 등의 데이터 구조
- 공간적 지역성 (Spatial locality)
  - 참조한 영역과 인접한 영역을 참조하는 특성
- 시간적 지역성 (Temporal locality)
  - 한 번 참조한 영역을 곧 다시 참조하는 특성

## Min Algorithm (OPT algorithm)
- 1966년 Belady에 의해 제시
- Minimize page fault frequency (proved)
  - Optimal solution
- 기법
  - 앞으로 가장 오랫동안 참조되지 않을 page 교체
- 실현 불가능한 기법 (Unrealizable)
  - Page referenct string을 미리 알고 있어야 함
- 교체 기법의 성능 평가 도구로 사용 됨 

<img width="461" alt="스크린샷 2024-02-19 오후 8 57 48" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/ff004a60-1e40-42d4-9928-e2cf7e3c10e7">


## Random Algorithm
- 무작위로 교체할 page 선택
- Low overhead
- No policy

## FIFO Algorithm
- First In First Out
  - 가장 오래된 page를 교체
- Page가 적재된 시간을 기억하고 있어야 함
- 자주 사용되는 page가 교체 될 가능성이 높음
  - Locality에 대한 고려가 없음
- FIFO anomaly (Belady's anomaly)
  - FIFO 알고리즘의 경우, 더 많은 page frame을 할당 받음에도 불구하고 page fault의 수가 증가하는 경우가 있음
<img width="541" alt="스크린샷 2024-02-19 오후 8 36 27" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/a587c3c7-5de1-4dfd-87d9-5328675b97fd">

## LRU (Least Recently Used) Algorithm
- 가장 오랫동안 참조되지 않음 page를 교체
- Page 참조 시 마다 시간을 기록해야 함
- Locality에 기반을 둔 교체 기법
- MIN algorithm에 근접한 성능을 보여줌
- 실제로 가장 많이 활용되는 기법
- 단점
  - 참조 시 마다 시간을 기록해야 함 (Overhead)
    - 간소화된 정보 수집으로 해소 가능
      - 예) 정확한 시간 대신, 순서만 기록
  - Loop 실행에 필요한 크기보다 작은 수의 page frame이 할당 된 경우, page fault 수가 급격히 증가함
    
<img width="496" alt="스크린샷 2024-02-19 오후 8 39 53" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/c3f3831d-0efb-49e3-8fbd-49690f6f1d11">

<img width="405" alt="스크린샷 2024-02-19 오후 8 42 22" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/f88ce78b-df53-446c-b50f-a81cb90799f0">

## LFU (Least Frequently Used) Alogorithm
- 가장 참조 횟수가 적은 Page를 교체
- Page 참조 시 마다, 참조 횟수를 누적 시켜야 함
- Locality 활용
  - LRU 대비 적은 overhead
- 단점
  - 최근 적재된 참조될 가능성인 높은 page가 교체 될 가능성이 있음
  - 참조 횟수 누적 overhead
<img width="380" alt="스크린샷 2024-02-19 오후 8 45 43" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/df64b087-c729-46be-8b82-b1b443df1d7f">

## NUR (Not Used Recently) Algorithm
- LRU approximation scheme
  - LRU보다 적은 overhead로 비슷한 성능 달성 목적
- Bit vector 사용
- 교체 순서
  1.  (r,m) = (0,0)
  2.  (r,m) = (0,1)
  3.  (r,m) = (1,0)
  4.  (r,m) = (1,1)
<img width="492" alt="스크린샷 2024-02-19 오후 8 47 50" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/60d2e3b8-74f9-4914-88a6-58120b02f815">

## Clock Algorithm
- Reference bit 사용함
  - 주기적인 초기화 없음
- Page frame들을 순차적으로 가리키는 pointer(시계 바늘)를 사용하여 교체될 page 결정
<img width="399" alt="스크린샷 2024-02-19 오후 8 48 53" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/74ba927e-f492-4f25-9af2-324a4f71c0aa">

- Pointer를 돌리면서 교체 page 결정
  - 현재 가리키고 있는 page의 referenct bit(r) 확인
  - r = 0 인 경우, 교체 page로 결정
  - r = 1 인 경우, referenct bit 초기화 후 pointer 이동
- 먼저 적재된 page가 교체될 가능성이 높음
  - FIFO와 유사
- Referenct bit를 사용하여 교체 페이지 결정
  - LRU (or NUR)과 유사

<img width="509" alt="스크린샷 2024-02-19 오후 8 50 55" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/2ab2b9d9-397e-47d8-b66e-46346a3db50e">

## Second Chance Algorithm
- Clock algorithm과 유사
- Update bit (m)도 함께 고려 함
  - 현재 가리키고 있는 page의 (r, m) 확인
  - (0, 0) : 교체 page로 결정
  - (0, 1) : -> (0, 0), write-back (cleaning) list에 추가 후 이동
  - (1, 0) : -> (0, 0) 후 이동
  - (1, 1) : -> (0, 1) 후 이동
<img width="506" alt="스크린샷 2024-02-19 오후 8 53 44" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/54f50282-8f94-48f8-bd9a-b3ccd5fbaed8">

## Other Algorithms
- Additional-reference-bits algorithm
  - LRU approximation
  - 여러 개의 referenct bit를 가짐
    - 각 time-interval에 대한 참조 여부 기록
    - History register for each page
- MRU(Most Recently Used) algorithm
  - LRU와 정반대 기법
- MFU(Most Frequently Used) algorithm
  - LFU와 정반대 기법
