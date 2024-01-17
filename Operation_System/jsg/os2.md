# 운영체제의 역할
- User Interface(편리성)
  - CUI (Character user interface)
  - GUI (Graphical User interface)
  - EUCI (End-User Comfortable Interface) * 해당 목적만을 위한 UI(mp3 플레이어 UI)
- Resource management(효율성)
  - HW resource (processor, memory, I/O devices, Etc.)
  - SW resource (file, application, message, signal, Etc.)
  - Process and Thread management
  - System management(시스템 보호) 

## 컴퓨터 시스템의 구성
<img width="590" alt="스크린샷 2024-01-17 오후 8 55 02" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/07d23c51-0f07-4a26-af30-fc6fa3ec7ecf">
<img width="672" alt="스크린샷 2024-01-17 오후 8 56 06" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/420208a4-b5c6-4542-a396-211b8a6db231">

* System Call : 터널이 제공하는 기능들 중에서 사용자가 사용할 수 있는 기능들을 모아놓은 인터페이스(사용자가 커널을 함부로 조작하면 문제가 생길 수 있기 때문에)

## 운영체제의 구분
### 동시 사용자 수
  - 단일 사용자(Single-user system)
    - 한 명의 사용자만 시스템 사용 가능
    - 한 명의 사용자가 모든 시스템 자원 독점
    - 자원 관리 및 시스템 보호 방식이 간단함
    - Windows, android, MS-DOS 등 
  - 다중 사용자(Multi-user system)
    - 동시에 여러 사용자들이 시스템 사용
    - 각종 시스템 자원들에 대한 소유 권한 관리 필요
    - 기본적으로 Multi-tasking 기능 필요
    - OS의 기능 및 구조가 복잡
    - 서버, 클러스터 장비 등에 사용
    - Unix, Linux, Windows server등
### 동시 실행 프로세스 수
  - 단일작업(Single-tasking system)
    - 시스템 내에 하나의 작업(프로세스)만 존재
    - 하나의 프로그램 실행을 마친 뒤에 다른 프로그램의 실행
    - 운영체제의 구조가 간단
    - MS-DOS
  - 다중작업(Multi-tasking system)
    - 동시에 여러 작업(프로세스)의 수행 가능
    - 작업들 사이의 동시 수행, 동기화 등을 관리해야 함
    - 운영체제의 기능 및 구조가 복잡
    - Unix/Linux, Windows 등
### 작업 수행 방식
#### Batch processing system

<img width="559" alt="스크린샷 2024-01-17 오후 10 46 49" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d85c5c9a-07cc-4504-b755-9e98308acccf">
  
  - 일괄처리 시스템
  - 모든 시스템을 중앙에서 관리 및 운영
  - 사용자의 요청 작업을 일정 시간 모아두었다가 한번에 처리
  - 시스템 지향적(System-oriented)
  - 장점
    - 많은 사용자가 시스템 자원 공유
    - 처리 효율(throughput) 향상
  - 단점
    - 생산성 저하(같은 유형의 작업들이 모이기를 기다려야 함)
    - 긴 응답시간(약 6시간이 걸린다.)

#### Time-sharing
    
  <img width="537" alt="스크린샷 2024-01-17 오후 10 01 47" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/6c6ad57d-d1c8-4ef3-aab3-53259b308d25">
    
  <img width="667" alt="스크린샷 2024-01-17 오후 10 04 39" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/db455f28-be24-47b0-a398-a7dfda2b4bb1">
  
  - 시분할 시스템
  - 여러 사용자가 자원을 동시에 사용
  - OS가 파일 시스템 및 가상 메모리 관리
  - 사용자 지향적(User-oriented)
    - 대화형 시스템
      - 단말기(CRT terminal) 사용
    - 장점
      - 응답시간 단축(약 5초)
      - 생산성 향상
        - 프로세서 유휴 시간 감소
    - 단점
      - 통신 비용 증가
        - 통신선 비용, 보안 문제 등
      - 개인 사용자 체감 속도 저하
        - 동시 사용자 수 증가 -> 시스템 부하 증가 -> 느려짐(개인관점)
#### Personal Computing
  - 개인이 시스템 전체 독점
  - CPU 활용률이 고려의 대상이 아님
  - OS가 상대적으로 단순함. 편리성 증대
  - 장점
    - 빠른 응답시간
  - 단점
    - 성능이 낮음
#### Parallel Processing System
  - 단일 시스템 내에서 둘 이상의 프로세서 사용
    - 동시에 둘 이상의 프로세스 지원
  - 메모리 등의 자원 공유
  - 사용 목적
    - 성능 향상
    - 신뢰성 향상(하나가 고장나도 정상 동작 가능)
  - 프로세서간 관계 및 역할 관리 필요
  <img width="402" alt="스크린샷 2024-01-17 오후 10 10 51" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/03e9b80e-3b24-48dc-9f32-e3916b6ba507">
    
  - 컴퓨터에 CPU를 넣는 것은 공간적 시스템적 한계가 있다.
#### Distributed processing system (분산처리 시스템)
  - 네트워크를 기반으로 구축된 병렬처리 시스템
  - 물리적인 분산, 통신망 이용한 상호 연결
  - 각각의 컴퓨터를 노드라고 한다.
  - 이들을 묶어서 관리해주는 분산 운영체제 존재
  - 각 구성 요소들간의 독립성유지, 공동 작업 가능
  - Cluster system, client-server system, P2P 등
  - 장점
    - 자원 공유를 통한 높은 성능
    - 고신뢰성, 높은 확정성
  - 단점
    - 구축 및 관리가 어려움
#### Real-time system
  - 실시간 시스템
  - 작업 처리에 제한 시간을 갖는 시스템
    - 제한 시간 내에 서비스를 제공하는 것이 자원 활용 효울보다 중요
  - 작업(task)의 종류
    - Hard real-time task
      - 시간 제약을 지키지 못하는 경우 시스템에 치명적 영향
      - 발전소 제어, 주기 제어 등
    - Soft real-time task
      - 동영상 재생 등
    - Non real-time task
     
# 운영체제의 구조
### 커널(Kernel)
  - OS의 핵심 부분(메모리 상주)
    - 가장 빈번하게 사용된느 기능등 담당
      - 시스템 관리(processor, memory, Etc) 등
  - 동의어
    - 핵(neucleus), 관리자프로그램(supervisor), 상주 프로그램(resident program), 제어 프로그램(control program) 등
### 유틸리티(Utility)
  - 비상주 프로그램
  - UI등 서비스 프로그램
<img width="592" alt="스크린샷 2024-01-17 오후 10 25 50" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/0e8af5ab-dbe5-4964-b614-f7825a0d6943">

## 단일 구조 
<img width="632" alt="스크린샷 2024-01-17 오후 10 30 01" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/0a2d5436-be3a-4d0d-b006-166b92feb74e">

- main에 모든 코드를 다 넣은 상태
- 장점
  - 커널 내 모듈간 직접 통신
  - 효율적 자원 관리 및 사용
- 단점
  - 커널의 거대화
  - 오류 및 버그, 추가 기능 구현 등 유지보수가 어려움
  - 동일 메모리에 모든 기능이 있어, 한 모듈의 문제가 전체 시스템에 영향(예, 악성코드)
     
## 계층 구조
<img width="285" alt="스크린샷 2024-01-17 오후 10 31 47" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/0642681e-7439-4315-a24f-b1dd0c2a1c4f">

- 기능별 분류
- 장점
  - 모듈화
  - 계층간 검증 및 수정 용의
  - 설계 및 구현의 단순화
- 단점
  - 단일구조 대비 성능 저하
    - 원하는 기능 수행을 위해 여러 계층을 거쳐야 함

## 마이크로 커널 구조
<img width="538" alt="스크린샷 2024-01-17 오후 10 33 07" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/79960e4e-6603-484e-b618-079e1f5c9738">
    
- 커널의 크기 최소화
- 필수 기능만 포함
- 기타 기능은 사용자 영역에서 수행

# 운영체제의 기능
### 프로세스(Process) 관리
#### 프로세스
- 커널에 등록된 실행 단위(실행중인 프로그램)
- 사용자 요청/프로그램의 수행 주체(entity)
#### OS의 프로세스 관리 기능
- 생성/삭제 상태관리
- 자원할당
- 프로세스 간 통신 및 동기화
- 교착상태 해결
#### 프로세스 정보 관리
- PCB(Process Control Bloc)
  
### 프로세서(Processor) 관리
#### 중앙 처리 장치(CPU)
- 프로그램을 실행하는 핵심 자원
#### 프로세스 스케줄링(Scheduling)
- 시스템 내의 프로세스 처리 순서 결정
#### 프로세서 할당 관리
- 프로세스들에 대한 프로세서 할당
  - 한 번에 하나의 프로세스만 사용 가능
### 메모리(Memory) 관리
#### 주기억장치
-  작업을 위한 프로그램 및 데이터를 올려놓는 공간
#### Multi-user, Multi-tasking 시스템
- 프로세스에 대한 메모리 할당 및 회수
- 메모리 여유 공간 관리
- 각 프로세스의 할당 메모리 영역 접근 보호
#### 메모리 할당 방법(scheme)
- 전체 적재
  - 장점 : 구현이 간단 / 단점 : 제한적 공간
- 일부 적재
  - 장점 : 메모리의 효율적 활용 / 단점 : 보조기억 장치 접근 필요
### 파일(File) 관리
#### 파일
- 논리적 데이터 저장 단위
#### 사용자 및 시스템의 파일관리
#### 디렉토리(directory)구조 지원
#### 파일 관리 기능
- 파일 및 디렉토리 생성/삭제
- 파일 접근 및 조작
- 파일을 물리적 저장 공간으로 사상(mapping)
- 백업 등
### 입출력(I/O) 관리
#### 입출력(I/O 과정)
- OS를 반드시 거쳐야함
<img width="506" alt="스크린샷 2024-01-17 오후 10 43 19" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/279cb607-267f-414e-b404-1328fcb6959e">

### 보조 기억 장치 및 기타 주변장치 관리 등
