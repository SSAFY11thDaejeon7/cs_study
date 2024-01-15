# 1강

## 운영체제 (OS) 란?

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/d0a7eddf-4b20-4469-87bd-58fb3110cc57)

- 컴퓨터 시스템 자원들(HW)을 효율적으로 관리하여 사용자 또는 응용 프로그램들에게 서비스를 제공하는 소프트웨어

## 컴퓨터 HW

- 프로세서 (CPU,GPU, ..)
- 메모리 (주기억장치 RAM, 보조 기억장치)
- 주변 장치

### 프로세서 CPU

컴퓨터의 두뇌, 연산 수행, 중앙처리장치

운영체제는 프로세스를 통해 프로세서에게 처리할 작업 할당

- 레지스터

프로세서 내부에 있는 메모리

속도 가장 빠르지만 비쌈

1. 사용자 가시(값 변경가능) 레지스터 : 데이터 레지스터, 주소 레지스터
2. 사용자 불가시(값 변경 불가능) 레지스터 : 프로그램 카운터(PC), 명령어 레지스터, 누산기

### 메모리 Memory

레지스터 → 캐시 → 메인 메모리(RAM) → 보조 기억장치(HDD,SSD)

CPU는 메인 메모리까지 직접 접근할 수 있음.

→ 순으로 가격 싸지고 용량 커짐

- 주기억장치 (메인 메모리)

디스크 입출력 병목현상(I/O Bottleneck) 방지 → CPU, 보조기억장치 속도 차이 해결

주로 DRAM 사용

- 캐시(Cache)

프로세서CPU “내부”에 있는 메모리

메인 메모리 입출력 병목현상 해결 : 메인 메모리 써도 CPU와 속도차이 존재하기 때문

캐시 동작 : HW적으로 관리

캐시 히트 : 메인 메모리까지 가지 않고 캐시에서 필요한 데이터를 바로 가져다 씀

캐시 미쓰 : 캐시 미쓰가 일어나면 메인 메모리까지 갔다가 캐시로 가져와야한다.

캐시 사용이 의미 있는 이유 (************ 중요 ***********)

- 공간적 지역성 Spatial Locality : 참조한 주소와 인접한 주소를 다시 참조하는 특성

배열, 구조체 등의 개념

- 시간적 지역 Temporal Locality : 한 번 참조한 주소를 특정 시간 이후에 다시 참조하는 특성

반복문 등 개념

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/eef5d574-7a1f-4e2d-b569-930d3f637914)

- 보조기억장치

프로세서가 직접 접근할 순 없음

용량이 가장 크고, 가격이 가장 저렴

** 가상 메모리 : 보조기억장치의 프로그램,데이터 > 주기억장치 인 경우에 사용

주기억장치 Main memory를 가상으로 관리하는 기법

### 시스템 버스

하드웨어들 간에 신호와 데이터를 주고 받는 통로

### 주변 장치

프로세서와 메모리를 제외한 HW들

입력장치(마우스,키보드), 출력장치(모니터,프린터), 저장장치 등