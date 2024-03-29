# 운영체제(OS)란?
- 컴퓨터 시스템 자원(HW)들을 효율적으로 관리
- 사용자, 응용 프로그램에게 서비스를 제공
- 프로세서에게 처리할 작업 할당 및 관리
- 프로그램의 프로세서 사용 제어
- 메모리 할당 및 관리
<br>

# 컴퓨터 하드웨어
- 프로세서
  - 연산을 위해 사용된다.
  - CPU, 그래픽카드, 응용 전용 처리장치등
- 메모리
  - 무언가를 저장하는 데에 사용된다.
  - 주 기억장치, 보조 기억장치등
- 주변장치
  - 키보드, 마우스, 모니터, 프린터, 네트워크 모뎀 등

## 프로세서 (Processor)
> 컴퓨터의 두뇌 (중앙처리장치)
- 연산 수행
- 컴퓨터의 모든 장치의 동작 제어

## 레지스터 (Register)
> 프로세서 내부에 있는 메모리
- 프로세서가 사용할 데이터 저장
- 컴퓨터에서 가장 빠른 메모리

- 레지스터의 종류
  - 용도에 따른 분류
    - 전용 레지스터, 범용 레지스터
  - 사용자가 정보 변경 가능 여부에 따른 분류
    - 사용자 가시 레지스터, 사용자 불가시 레지스터
  - 저장하는 정보의 종류에 따른 분류
    - 데이터 레지스터, 주소 레지스터, 상태 레지스터

- 사용자 가시 레지스터
  - 데이터 레지스터: 함수 연산에 필요한 데이터를 저장한다.
  - 주소 레지스터: 주소나 유효 주소를 계산하는 데 필요한 주소의 일부분을 저장한다.
- 사용자 불가시 레지스터
  - 프로그램 카운터: 다음에 실행할 명령어의 주소를 보관하는 레지스터이다.
  - 명령어 레지스터: 현재 실행하는 명령어를 보관하는 레지스터이다.
  - 누산기: 데이터를 일시적으로 저장하는 레지스터이다.

- 프로세서는 다양한 PC, MAR, IR, MBR등 레지스터들을 통해서 연산을 하며 동작함.
<br>

## 메모리 (Memory)
> 데이터를 저장하는 장치 (기억장치)

![1일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/478fc79b-33ba-4643-88b4-eab6aba5a2ab)


### 주기억장치 (Main memory)
- 프로세서가 수행할 프로그램과 데이터 저장
- DRAM을 주로 사용
- 디스크 입출력 병목현상 (I/O bottleneck) 해소
<br>
> Processor - Main memory - Disk 간의 데이터 교환을 하는 이유

프로세서는 빠르게 발전한 반면, 디스크는 그렇지 않았음.

-> 디스크까지 데이터가 전달되는 것이 느리므로 이를 해소하기 위해 메인 메모리를 사이에 넣음

### 캐시 (Cache)
- 프로세서 내부에 있는 메모리 (L1, L2 캐시 등)
- 메인 메모리의 입출력 병목현상 해소 (CPU와 메인 메모리 사이에 넣음)

- 캐시의 동작
  - 일반적으로 HW적으로 관리됨
  - 캐시 히트 (Cache hit)
    - 필요한 데이터 블록이 캐시 존재
  - 캐시 미스 (Cache miss)
    - 필요한 데이터 블록이 없는 경우
    - 메모리에서 cache line(block)만큼 캐시로 가져와 채움

### 지역성 (Locality)
- 공간적 지역성 (Spatial locality)
  - 참조한 주소와 인접한 주소를 참조하는 특성
- 시간적 지역성 (Temporal locality)
  - 한 번 참조한 주소를 곧 다시 참조하는 특성
- 지역성은 캐시 적중률(cache hit ratio)과 밀접
  - 알고리즘 성능을 향상시키기 위한 중요한 요소 중 하나

### 보조기억 장치
- 프로그램과 데이터를 저장
- 프로세서가 직접 접근할 수 없음
- 용량이 크고, 가격이 저렴함

### 시스템 버스
> 하드웨어(프로세서, 메인 메모리, 주변장치)들이 데이터 및 신호를 주고 받는 물리적인 통로

![1일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/75370648-f941-4c8c-bb56-12498a641865)

- 종류: 데이터 버스, 주소 버스, 제어 버스
<br>

![1일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/f0ef904c-0647-45e9-98ea-4ed3a9412468)

1 - PC -> MAR: PC(메모리)에 저장된 주소를 프로세서 내부 버스를 이용해서 MAR에 전달

2-1 - MAR -> MBR:  MAR에 저장된 주소에 해당하는 메모리 위치에서 명령어를 인출한 후에, 이 명령어를 MBR에 저장. 이 때, 제어장치는 메모리에 저장된 내용을 읽도록 제어 신호를 발생시킴.

2-2 - PC + 1 -> PC: 다음 명령어를 인출하려고 PC를 증가시킨다.

3 - MBR -> IR: MBR에 저장된 내용을 IR에 전달한다.
- PC: 프로그램 카운터
- MAR: 메모리 주소 레지스터
- MBR: 메모리 버퍼 레지스터
- IR: 명령어 레지스터 

