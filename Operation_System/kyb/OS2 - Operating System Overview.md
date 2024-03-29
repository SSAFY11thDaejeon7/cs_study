# 운영체제의 역할

- **사용자 인터페이스(편리성)**
    - CUI(Character) : 문자 기반 인터페이스(Shell, Terminal …)
    - GUI(Graphic) : 그래픽 기반 인터페이스
    - EUCI(End-User Comfortable)
- **자원 관리(효율성)**
    - HW 자원 : 프로세서, 메모리, 입출력장치 등
    - SW 자원 : 파일, 어플리케이션, 메시지, 신호 등
- **프로세스 & 쓰레드 관리**
- **시스템 관리(보호)**

## 컴퓨터 시스템의 구성

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/644e295e-cf0a-4d2f-b938-6e0415c941c9)




OS는 하드웨어를 관리하며 사용자에게 서비스 제공

- **System Call Interface**
    - 사용자가 필요한 기능을 운영체제에 요청하기 위한 통로, 수단
    - 사용자가 커널에 직접 접근하는 것을 방지

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/e0be4a3c-5ab9-4a3a-a648-2584cb76e92a)




# 운영체제의 구분

## 동시 사용자 수

- 단일 사용자 (single-user)
    - 한 명의 사용자만 시스템 사용 가능
        - 한 사용자가 모든 시스템 자원 독점
        - 자원 관리, 시스템 보호 방식이 간단
    - 개인용 장비(PC, Mobile) 등에 사용
    
- 다중 사용자(multi-user)
    - 동시에 여러 사용자들이 시스템 사용
        - 각종 시스템 자원들에 대한 소유 권한 관리 필요
        - 멀티태스킹 기능 필요
        - OS의 기능, 구조가 복잡
    - 서버, 클러스터 장비 등에 사용
    

## 동시 실행 프로세스 수

- 단일 작업(Single-tasking)
    - 시스템 내에 하나의 작업(프로세스)만 존재
        - 기존의 작업이 종료되어야 다른 작업 수행 가능
    - 운영체제 구조가 간단 (MS-DOS)

- 다중 작업(Multi-tasking)
    - 동시에 여러 작업(프로세스) 수행 가능
        - 작업들간 동시 수행, 동기화 등의 관리 필요
    - 운영체제의 기능 및 구조가 복잡(Unix/Linux, Windows)

## 작업 수행 방식 (사용자가 느끼는 사용 환경)

### 일괄 처리 시스템(Batch) [1950s ~ 1960s]

- 모든 시스템을 중앙에서 관리, 운영
- 일정 시간동안 들어오는 작업을 모아 한번에 처리
- 시스템 지향적
- 장점
    - 많은 사용자가 시스템 자원 공유
    - 처리 효율 향상
- 단점
    - 생산성 저하 : 같은 유형의 작업들이 모여야 처리 → 대기 시간 발생
    - 긴 응답시간
    

### 시분할 시스템(Time-Sharing) [1960s ~ 1970s]

- 여러 사용자가 자원을 동시에 사용
    - 운영체제가 파일시스템 및 가상 메모리 관리
- 사용자 지향적
- 장점
    - 응답 시간 단축
    - 생산성 향상
- 단점
    - 통신 비용 증가
    - 개인 사용자 체감 속도 저하
        - 동시 사용자 수 많을수록 시스템 부하 ▲ → 느려짐

### 개인 컴퓨팅 시스템 (Personal Computing)

- 개인이 시스템 전체 독점
- CPU 활용률은 고려 x
- 운영체제가 상대적으로 단순함
- 장점
    - 빠른 응답 시간
- 단점
    - 낮은 성능

### 병렬 처리 시스템 ( Parallel Processing)

- 단일 시스템 내 둘 이상의 프로세서 사용
- 메모리 등의 자원 공유
- 목적
    - 성능 향상
    - 신뢰성 향상 ( A가 고장나도 B를 통해 정상 동작 수행)
    - 프로세서간 관계 및 역할 관리 필요

### 분산 처리 시스템(Distributed Processing)

- 네트워크 기반으로 구축된 병렬처리 시스템
- 물리적인 분산, 통신망을 이용한 상호 연결
- 각각의 운영체제를 탑재한 다수의 범용 시스템으로 구성
- 사용자는 분산운영체제를 통해 하나의 프로그램, 자원처럼 사용 가능
- 각 구성요소간 독립성 유지, 공동작업 가능
- 슈퍼컴퓨터, 클라이언트-서버시스템, P2P 등에 사용
- 장점
    - 자원 공유를 통한 높은 성능
    - 고 신뢰성, 높은 확정성
- 단점
    - 구축 및 관리가 어려움

### 실시간 시스템(Real-Time)

- 작업 처리에 제한 시간을 갖는 시스템
    - 제한 시간 내에 서비스를 제공하는 것이, 자원 활용 효율보다 중요
    - 작업의 종류
        - Hard realtime task
            - 시간 제약을 지키지 못하는 경우, 시스템에 치명적 영향
            - 발전소, 무기 제어 등
        - Soft real-time task
            - 동영상 재생 등
        - Non real-time task

# 운영체제의 구조

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/289316b2-7e5c-4df7-89df-ea33cd595af4)



### 커널

- 운영체제의 핵심 부분(메모리 상주)
    - 가장 빈번하게 사용되는 기능들 담당(시스템 관리 - 프로세서, 메모리, … 등)

### 유틸리티

- 비 상주 프로그램
- UI등 서비스 프로그램

## 단일 구조

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/ad095278-ef58-422a-a5e0-a59b682a5752)



운영체제의 기능을 **하나의 커널**에 모아놓은 구조

- 장점
    - 커널 내 모듈간 직접 통신 → 효율적 자원 관리 및 사용
- 단점
    - 커널의 거대화
        - 오류, 버그, 추가기능 구현 등 유지보수가 어려움
        - 동일 메모리에 모든 기능 존재 → 한 모듈에 문제가 모든 모듈에 영향 가능성

## 계층 구조

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/f39528cc-040a-4e27-aa6e-07ffe1063bfe)



각 **기능 별로 분리**(모듈화) 하여 **계층**으로 묶는 구조

- 장점
    - 모듈화를 통한 계층간 검증, 수정 용이
    - 설계 및 구현의 단순화
- 단점
    - 단일구조 대비 성능 저하
        - 원하는 기능을 수행하기 위해 여러 계층을 거쳐야 함

## 마이크로 커널 구조

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/d9b6f3f6-cb00-44e4-8552-f6dd3359fe01)



커널에는 필수 기능만을 담고, 나머지는 사용자 영역에서 수행

→ **커널의 크기 최소화**

# 운영체제의 기능

## 프로세스 관리

- 프로세스
    - 커널에 등록된 실행 단위(실행 중인 프로그램)
    - 사용자 요청/프로그램의 수행 주체
- 운영체제의 프로세스 관리 기능
    - 생성/삭제, 상태 관리
    - 자원 할당
    - 프로세스 간 통신 및 동기화
    - 교착상태 해결
- 프로세스 정보 관리(PCB)

## 프로세서 관리

- 중앙 처리 장치(CPU)
    - 프로그램을 실행하는 핵심 자원
- 프로세스 스케줄링
    - 시스탬 내의 프로세스 처리 순서 결정
- 프로세서 할당 관리
    - 프로세스들에 대한 프로세서 할당

## 메모리 관리

- 주 기억 장치
    - 작업을 위한 프로그램 및 데이터를 올려 놓는 공간
- 다중 사용자, 멀티 태스킹 시스템
    - 프로세스에 대한 메모리 할당/회수
    - 메모리 여유 공간 관리
    - 각 프로세스의 할당 메모리 영역 접근 보호
- 메모리 할당 방법
    - 전체 적재: 구현 간단, but 제한적 공간
    - 일부 적재(가상메모리) : 메모리의 효율적 활용, but 보조 기억 장치 접근 필요

## 파일 관리

- 사용자 및 시스템의 파일 관리
- 디렉토리 구조 지원
- 파일 관리 기능
    - 파일, 디렉토리의 생성/삭제
    - 파일 접근, 조작
    - 파일을 물리적 저장 공간으로 사상
    - 백업

## 입출력 관리

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/45e57f56-a7b1-4b0e-8153-05aab835e415)



## 기타

- 디스크
- 네트워킹
- 보안, 보호 시스템
- 명령 인터프리터 시스템
- System call interface
