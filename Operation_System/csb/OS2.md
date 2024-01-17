# 1. 운영체제의 역할

- User Interface (편리성)
    - CUI(Character User Interface)
    - GUI(Graphical User Interface)
    - ECUI(End-Use Comfortable Interface)
- Resource management (효율성)
    - HW resource(processor, memory, I/O devices, Etc.)
    - SW resource(file, application, message, signal, Etc.)
- Process and Thread management
- System management(시스템 보호)

## 컴퓨터 시스템의 구성

<img width="921" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/5df807e4-19ce-40fe-b064-a58d02834599">

<img width="634" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/caf53fef-8c33-4fee-9a13-baf280b5ca71">


# 2. 운영체제의 구분

##  2.1 동시 사용자 수

### Single-user system

- 한 명의 사용자만 시스템 사용 가능
    - 한 명의 사용자가 모든 시스템 자원 독점
    - 자원관리 및 시스템 보호 방식이 간단함
- 개인용 장비 등에 사용
    - Windows 7/10, android, MS-DOS 등

### Multi-user system

- 동시에 여러 사용자들이 시스템 사용
    - 각종 시스템 자원들에 대한 소유 권한 관리 필요
    - 기본적으로 Multi-tasking 기능 필요
    - OS의 기능 및 구조가 복잡
- 서버, 클러스터 장비 등에 사용
    - Unix, Linux, Window server 등

## 2.2 동시 실행 프로세스 수

### 단일작업(Single-tasking system)

- 시스템 내에 하나의 작업(프로세스)만 존재
    - 하나의 프로그램 실행을 마친 뒤에 다른 프로그램의 실행
- 운영체제의 구조가 간단
- MS-DOS

### 다중 작업(Multi-tasking system)

- 동시에 여러 작업(프로세스)의 수행 가능
    - 작업들 사이의 동시 수행, 동기화 등을 관리해야 함
- 운영체제의 기능 및 구조가 복잡
- Unix/Linux, Windows 등

## 2.3 작업 수행 방식

### 순차 처리(NO OS ~ 1940s)

- 운영체제 개념 존재하지 않음
    - 사용자가 기계어로 직접 프로그램 작성
    - 컴퓨터에 필요한 모든 작업 프로그램에 포함
- 실행하는 작업 별로 순차 처리
    - 각각의 작업에 대한 준비 시간이 소요

### Batch Systems(1950s ~ 1960s)

<img width="628" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/75a49970-9524-4e4e-ab67-0bff95f47d83">

- 모든 시스템을 중앙(전자계산소 등)에서 관리 및 운영
- 사용자의 요청 작업(천공카드 등)을 일정 시간 모아 두었다가 한번에 처리

- 시스템 지향적(system-oriented)
- 장점
    - 많은 사용자가 시스템 자원 공유
    - 처리 효율(throughput) 향상
- 단점
    - 생산성(productivity) 저하 → 같은 유형의 작업들이 모이기를 기다려야 함
    - 긴 응답시간(turnaround time) → 약 6시간(작업 제출에서 결과 출력까지의 시간)

### Time Sharing Systems(= 시분할, 1960s ~ 1970s)

<img width="501" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/32a7af9e-3fd8-4cad-9b6c-ee953e3ad869">

- 여러 사용자가 자원을 동시에 사용
    - OS가 파일 시스템 및 가상 메모리 관리
- 사용자 지향적(User-oriented)
    - 대화형 시스템
    - 단말기 사용
- 장점
    - 응답시간 단축(약 5초)
    - 생산성 향상 → 프로세서 유휴 시간 감소
- 단점
    - 통신 비용 증가, 보안 문제 발생
    - 개인 사용자 체감 속도 저하 → 동시 사용자 수가 높아짐 → 시스템 부하 높아짐 → 느려짐(개인 관점)
    - 동시 사용자 수가 늘어남에 따라 시스템 부하가 높아져 개인 사용자 체감 속도 저하

### Personal Computing(개인 컴퓨팅)

- 개인이 시스템 전체 독점
- CPU 활용률이 고려의 대상이 아님
- OS가 상대적으로 단순함
    - 다양한 사용자 지원 기능 지원
- 장점
    - 빠른 응답시간
- 단점
    - 낮은 성능

### Parallel Processing System(병렬 처리 시스템)

- 단일 시스템 내에서 둘 이상의 프로세서 사용
    - 동시에 둘 이상의 프로세스 지원
- 메모리 등의 자원 공유(Tightly-coupled system)
- 사용 목적
    - 성능 향상
    - 신뢰성 향상(하나가 고장나더라도 정상 동작 가능)
- 프로세서간 관계 및 역할 관리 필요
    
<img width="593" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/656f8c43-a6cd-4ed1-977a-cc741b328c46">

### Distributed Processing Systems(분산 처리 시스템)

- 네트워크를 기반으로 구축된 병렬처리 시스템(Loosely-coupled system)
    - 물리적인 분산, 통신망을 이용한 상호 연결
    - 각각 운영체제를 탑재한 다수의 범용 시스템으로 구성
    - 사용자는 분산운영체제를 통해 하나의 프로그램, 자원처럼 사용 가능
    - 각 구성 요소들간의 독립성유지, 공독작업 가능
    - Cluster system, client-server system, P2P 등
- 장점
    - 자원 공유를 통한 높은 성능
    - 높은 신뢰성, 높은 확장성
- 단점
    - 구축 및 관리가 어려움

### Real-time Systems(실시간 시스템)

- 작업 처리에 제한 시간(deadline)을 갖는 시스템
    - 제한 시간 내에 서비스를 제공하는 것이 자원 활용 효율보다 중요
- 작업의 종류
    - Hard real-time task
        - 시간 제약을 지키지 못하는 경우 시스템에 치명적 영향
        - 발전소 제어, 무기 제어 등
    - Soft real-time task
        - 동영상 재생 등
    - Non real-time task

# 3. 운영체제의 구조

<img width="427" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/b94effc9-9939-4e09-b215-d756fe05a4f4">

## 커널(Kernel)

- OS의 핵심 부분(메모리 상주)
    - 가장 빈번하게 사용되는 기능들을 담당
        - 시스템 관리(processor, memory, Etc) 등
    - 동의어
        - 핵, 관리자 프로그램, 상주 프로그램, 제어 프로그램 등

## 유틸리티(Utility)

- 비상주 프로그램
- UI 등 서비스 프로그램

## 단일 구조

<img width="701" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/18eaa51f-e733-4376-a490-0a6ade26be3b">

- 장점
    - 커널 내 모듈간 직접 통신
        - 효율적 자원 관리 및 사용
- 단점
    - 커널의 거대화
        - 오류 및 버그, 추가 기능 구현 등 유지보수가 어려움
        - 동일 메모리에 모든 기능이 있어, 한 모듈의 문제가 전체 시스템에 영향

## 계층 구조

<img width="368" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/0f656df9-d0f7-4020-9e74-aae651d514fc">

- 장점
    - 모듈화
        - 계층간 검증 및 수정 용이
    - 설계 및 구현의 단순화
- 단점
    - 단일 구조 대비 성능 저하
        - 원하는 기능 수행을 위해 여러 계층을 거쳐야 함

## 마이크로 커널 구조

<img width="781" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/92368fe3-01a6-4be1-9709-a0289cfab158">

- 커널의 크기 최소화
    - 필수 기능만 포함
    - 기타 기능은 사용자 영역에서 수행

# 4. 운영체제의 기능

- 프로세스 관리
- 프로세서 관리
- 메모리 관리
- 파일 관리
- 입출력 관리
- 보조 기억 장치 및 기타 주변장치 관리 등

## 4.1 프로세스 관리

- 프로세스
    - 커널에 등록된 실행 단위(실행 중인 프로그램)
    - 사용자 요청 / 프로그램의 수행 주체(entity)
- OS의 프로세스 관리 기능
    - 생성/삭제, 상태관리
    - 자원 할당
    - 프로세스 간 통신 및 동기화(synchronization)
    - 교착상태(deadlock) 해결
- 프로세스 정보 관리
    - PCB(Process Control Block)

## 4.2 프로세서 관리

- 중앙 처리 장치
    - 프로그램을 실행하는 핵심 자원
- 프로세스 스케줄링
    - 시스템 내의 프로세스 처리 순서 결정
- 프로세서 할당 관리
    - 프로세스들에 대한 프로세서 할당(한 번에 하나의 프로세스만 사용 가능)

## 4.3 메모리 관리

- 주기억장치
    - 작업을 위한 프로그램 및 데이터를 올려 놓는 공간
- Multi-user, Multi-tasking 시스템
    - 프로세스에 대한 메모리 할당 및 회수
    - 메모리 여유 공간 관리
    - 각 프로세스의 할당 메모리 영역 접근 보호
- 메모리 할당 방법
    - 전체 적재
        - 장점 - 구현이 간단
        - 단점 - 제한적 공간
    - 일부 적재
        - 프로그램 및 데이터의 일부만 적재
        - 장점 - 메모리의 효율적 활용
        - 단점 - 보조기억 장치 접근 필요

## 4.4 파일 관리

- 파일
    - 논리적 데이터 저장 단위
- 사용자 및 시스템의 파일 관리
- 디렉토리 구조 지원
- 파일 관리 기능
    - 파일 및 디렉토리 생성/삭제
    - 파일 접근 및 조작
    - 파일을 물리적 저장 공간으로 사상
    - 백업 등

## 4.5 입출력 관리

- 입출력 과정 → OS를 반드시 거쳐야 함

<img width="545" alt="image" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/81237987/66be79eb-da8d-43b1-a759-c59331e27be6">

### 4.6 Others

- Disk
- Networking
- Security and Protection system
- Command interpreter system
- System call interface
    - 응용 프로그램과 OS 사이의 인터페이스
    - OS가 응용프로그램에 제공하는 서비스
