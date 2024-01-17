# 운영체제의 역할
- User Interface (편리성)
  - CUI (과거에 문자를 기반으로 작동한 유저 인터페이스)
  - GUI (현재 많이 쓰는 그림 형태의 유저 인터페이스)
  - EUCI (특별한 목적을 위해서 사용자에 특화된 인터페이스)
- Resource management (효율성)
  - HW resource (processor, memory, I/O devices ...)
  - SW resource (file, application ...)
- 프로세스(실행 주체)와 쓰레드 관리
- 시스템 보호
<br>

# 컴퓨터 시스템의 구성
![2일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/5dfa8021-be47-4b25-8c61-463079d7ff52)

Kernel ~ System Call Interface: OS

- System Call Interface: 사용자가 사용할 수 있는 기능을 모아놓은 것들
- 사용자가 커널을 마음대로 바로 조작하면 위험하므로 OS에 요청하는 통로로서 System call Interface를 사용.
<br>

# 운영체제의 구분
- 동시 사용자 수
  - Single-user system
  - Multi-user system
- 동시 실행 프로세스 수
  - Single-tasking system
  - Multi-tasking system
- 작업 수행 방식
  - Batch processing system
  - Time-sharing system
  - Distributed processing system
  - Real-time system

## 동시 사용자 수에 따른 구분
- **단일 사용자**
  - 한 명의 사용자만 시스템 사용 가능
    - 한명의 사용자가 모든 시스템 자원 독점
    - 자원관리 및 시스템 보호 방식이 간단함
  - 개인용 장비 등에 사용
- **다중 사용자**
  - 동시에 여러 사용자들이 시스템 사용
    - 각종 시스템 자원(파일 등)들에 대한 소유 권한 관리 필요
    - 기본적으로 Multi-tasking 기능 필요
    - OS의 기능 및 구조가 복잡함 
  - 서버, 클러스터 장비 등에 사용
 
## 동시 실행 프로세스 수에 따른 구분
- **단일 작업 (Single-tasking system)**
  - 시스템 내에 하나의 작업(프로세스)만 존재
    - 하나의 프로그램 실행을 마친 뒤에 다른 프로그램 실행
  - 운영체제의 구조가 간단함
  - 예) MS-DOS
- **다중 작업 (Milti-tasking system)**
  - 동시에 여러 작업(프로세스)의 수행 가능
    - 작업들 사이의 동시 수행, 동기화 등을 관리해야 함
  - 운영체제의 기능 및 구조가 복잡함
  - 예) Unix/Linux, Windows 등

## 순차 처리 (No OS, ~1940s)
- **운영체제라는 개념이 존재하지 않음**
  - 사용자가 기계어로 직접 프로그램 작성
  - 컴퓨터에 필요한 모든 작업 프로그램에 포함
- 실행하는 작업 별 순차 처리
  - **각각의 작업에 대한 준비 시간이 소요**
 
## Batch Systems (1950s ~ 1960s)
![2일차-2](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/3a32c4ee-55c4-4861-a9ec-bc8fdb587e70)

- 모든 시스템을 중앙에서 관리 및 운영
- 사용자의 요청 작업을 일정 시간 모아 두었다가 한번에 처리
- **시스템 지향적 (System-oriented)**
- **장점**
  - 많은 사용자가 시스템 자원 공유
  - 처리 효율 향상
- **단점**
  - 생산성 저하 -> 같은 유형의 작업들이 모이기를 기다려야 함
  - 긴 응답시간

## Time Sharing Systems (시분할 시스템, 1960s ~ 1970s)
![2일차-3](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/af5cd3b4-91fb-4c4c-aa8e-3afb9ed9335f)

- 여러 사용자가 자원을 동시에 사용
  - OS가 파일 시스템 및 가상 메모리 관리
- 사용자 지향적 (User-oriented)
  - 대화형 시스템
  - 단말기 사용
- **장점**
  - 응답시간 단축
  - 생산성 향상 -> 프로세서 유휴 시간 감소
- **단점**
  - 통신 비용 증가 -> 통신선 비용, 보안 문제 등
  - 개인 사용자 체감 속도 저하
    - 동시 사용자 수가 많아질수록 시스템에 부하가 걸려서 느려짐 (개인 관점)

## Personal Computing
- 개인이 시스템 전체 독점 -> 시스템의 활용률을 높임
- CPU 활용률 (utilization)이 고려의 대상이 아님
- OS가 상대적으로 단순함. 하지만, 다양한 사용자의 편리성을 높여주는 기능을 지원함
- **장점**
  - 빠른 응답시간
- **단점**
  - 성능이 낮음
 
## Perallel Processing System (병렬 처리 시스템)
- 단일 시스템 내에서 둘 이상의 프로세서 사용
- 메모리 등의 자원 공유 (tightly-coupled system)
- 사용 목적
  - 성능 향상
  - 신뢰성 향상 (하나가 고장나도 정상 동작이 가능함)
- 프로세서간의 관계 및 역할 관리 필요

## Distributed Processing Systems (분산 처리 시스템)
> 네트워크를 기반으로 구축된 병렬처리 시스템 (loosely-coupled system)

- 물리적인 분산, 통신망 이용한 상호 연결
- 각각 운영체제 탑재한 다수의 범용 시스템으로 구성
- 사용자는 **분산운영체제**를 통해 하나의 프로그램, 자원처럼 사용 가능
- 각 구성 요소들간의 독립성 유지, 공동작업 가능
- **장점**
  - 자원 공유를 통한 높은 성능
  - 고신뢰성, 높은 확정성
- **단점**
  - 구축 및 관리가 어려움
 
## Real-time Systems (실시간 시스템)
- 작업 처리에 **제한 시간(deadline)**을 갖는 시스템
  - 제한 시간 내에 서비스를 제공하는 것을 자원 활용 효율보다 중요시함

- 작업(task)의 종류
  - Hard real-time task
    - 시간 제약을 지키지 못하는 경우 시스템에 치명적인 영향
    - 예) 발전소 제어, 무기 제어 등
  - Soft real-time task
    - 동영상 재생 등
  - Non real-time task

# 운영체제의 구조
- 커널(Kernel)
  -  OS의 핵심 부분 (메모리 상주)
  -  가장 빈번하게 사용되는 기능(프로세서, 메모리 등)들을 담당
- 유틸리티 (Utility)
  - 비상주 프로그램
  - UI 등 서비스 프로그램
 
## 단일 구조
![2일차-4](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/e3b2ef7b-515d-4894-9ce2-6638063d1b06)

- **장점**
  - 커널 내 모듈간 직접 통신
    - 효율적 자원 관리 및 사용
- **단점**
  - 커널의 거대화
    - 오류 및 버그, 추가 기능 구현 등 유지보수가 어려움
    - 동일 메모리에 모든 기능이 있어서 한 모듈의 문제가 전체 시스템에 영향을 줌 (악성코드 등)

## 계층 구조
![2일차-5](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/eaa9a8c6-c80b-4c9c-ab2e-7a70906230d0)

- 사용자 영역: 계층 5, 커널 영역: 계층 0 ~ 4
- **장점**
  - 모듈화: 계층간 검증 및 수정 용의함
  - 설계 및 구현의 단순화
- **단점**
  - 단일 구조 대비 성능 저하
    - 원하는 기능을 수행하기 위해 여러 계층을 거쳐야 함

## 마이크로 커널 구조
![2일차-6](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/3024a35d-0ae6-44aa-98ba-61543f2d0136)

- 커널의 크기 최소화
  - 필수 기능만 포함
  - 기타 기능은 사용자 영역에서 수행
<br>

# 운영체제의 기능
- 프로세스, 프로세서, 메모리, 파일, 입출력, 보조 기억 장치등의 주변 장치 관리

## Process Management
- 프로세스 (Process)
  - 커널에 등록된 실행 단위 (실행 중인 프로그램)
  - 사용자 요청/프로그램의 수행 주체
- OS의 프로세스 관리 기능
  - 생성/삭제, 상태 관리
  - 자원 할당
  - 프로세스 간 통신 및 동기화
  - 교착상태(deadlock) 해결
- 프로세스 정보 관리
- 중앙 처리 장치 (CPU)
  - 프로그램을 실행하는 핵심 자원
- 프로세스 스케줄링
  - 시스템 내의 프로세스 처리 순서 결정
- 프로세서 할당 관리
  -프로세스들에 대한 프로세서 할당

## Memory Management
- 주기억장치
  - 작업을 위한 프로그램 및 데이터를 올려 놓는 공간
- Multi-user, Multi-tasking 시스템
  - 프로세스에 대한 메모리 할당 및 회수
  - 메모리ㅣ 여유 공간 관리
  - 각 프로세스의 할당 메모리 영역 접근 보호
- 메모리 할당 방법
  - 전체 적재
  - 일부 적재

