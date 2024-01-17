# 2강

# 운영체제의 역할

---

## 1. User Interface (편리성)

- CUI (Character User Interface) : 문자 interface, 과거 cmd등
- GUI (Graphical User Interface) : Graphic interface
- EUCI (End-User Comfortable Interface) : 특정 사용자 interface

## 2. Resource Management (효율성)

- HW resource 관리 (프로세서, 메모리, IO 장치)
- SW resource (파일, 애플리케이션, 메시지, 신호)

## 3. Process and Thread management

- 프로세스, 스레드 관리

## 4. System Management

- 시스템 관리

# 컴퓨터 시스템 구성

---

![Untitled](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/86fd8b24-c5e5-4928-bfff-10555302b6d7)
## Kernel 커널

**정의** : 운영체제 중 항상 메모리에 올라가 있는 운영체제의 핵심 부분으로써 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하는 역할을 하며 컴퓨터 자원(HW 리소스, SW리소스, 프로세스, 스레드 등)들을 관리하는 역할을 한다.

**커널**은 항상 컴퓨터 자원들만 바라보고 있기에 사용자와 상호작용은 지원하지 않는다.  따라서 사용자와 직접적인 상호작용 위해 대표적으로 쉘(Shell) 명령어 해석기 등의 프로그램을 지원

**커널의 역할**

![Untitled (7)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/e2bef978-32d1-4851-98b4-ea4eb7854248)
**인터페이스로써의 커널**

![Untitled (1)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/cc2d2185-41ec-4bb3-aa88-ef4fed426d00)
사용자들이 OS 커널에 직접적으로 접근하게 되면 문제 발생.

따라서 사용자가 커널에 필요한 기능들을 요청한다

그 통로가 SCI 즉 **System Call Interface**이다.

**정리**

**커널**은 사용자가 **System Call**을 통해 컴퓨터 자원을 사용할 수 있게 해주는 **자원 관리자**라고 할 수 있다.

# 운영체제의 구분

---

## 1. 동시 사용자 수

- **단일 사용자 (Single-user system)**

한 명의 사용자만 시스템 사용 가능, 모든 시스템 자원 독점

개인용 장비 (PC, mobile) 등에 사용. Windows, android, MS-DOS 등

- **다중 사용자 (Multi-user system)**

동시에 여러 사용자들이 시스템 사용

각종 시스템 자원들에 대한 소유 권한 관리 필요, Multi-tasking 기능 필요

서버, 클러스터 장비 등에 사용. Unix, Linux, Windows Server 등

## 2. 동시 실행 프로세스 수

- **단일 작업 (Single-tasking system)**

시스템 내에 한 번에 하나의 작업(프로세스)만 존재 가능

하나의 프로그램 실행을 마친 후에 다른 프로그램 실행 가능

예) MS-DOS

- **다중 작업 (Multi-tasking system)**

동시에 여러 작업(프로세스) 수행이 가능 → 작업들 간에 동시 수행, 동기화 고려 필요

예) Unix, Linux, Windows

# 3. 작업 수행 방식

운영체제 발전 역사와 관련 있음

- Batch processing system : 일괄처리 시스템
- Time-sharing system : 시분할 시스템
- Distributed processing system : 분산처리 시스템
- Real-time system : 실시간 시스템

### 0. 순차처리

운영체제 개념 자체가 없었음

사용자가 기계어로 직접 모든 프로그램 작성

실행하는 작업 별 순차 처리. 기다리다가 자기 차례가 되면 작업 수행 

### 1. Batch processing system 일괄처리

모든 작업들을 모아놓은 것 : Batch

모든 시스템을 중앙에서 일괄적으로 관리 및 운영. 사용자들의 요청 작업을 모두 모아놨다가 일정 시간마다 한번에 처리

**시스템 지향적 (System-oriented)**

**장점**

많은 사용자가 시스템 자원 공유. 처리 효율(throughput) 향상

**단점**

생산성 저하(같은 유형 작업들이 모일때까지 기다려야함)

긴 응답시간

### 2. Time Sharing Systems 시분할 시스템

![Untitled (2)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/856427b6-8199-47a8-be68-c4c48f535d5e)
프로세스들을 시간 단위로 쪼개서 번갈아가며 처리

여러 사용자가 자원을 동시에 사용.

OS는 파일 시스템 및 가상 메모리를 관리

**사용자 지향적 (User-Oriented)**

**장점**

응답시간 단축

생산성 향상. 프로세서 유휴 시간 감소

**단점**

통신 비용 증가. 보안 문제 발생

동시 사용자 수가 많으면 부하가 커져서 느려짐. 개인 사용자 체감 속도 저하

### 3. Personal Computing

개인이 시스템 전체를 독점

CPU 활용률이 목적이 아님. 내가 얼마나 편리하게 자원들을 사용할 수 있는지가 중요

**장점**

빠른 응답시간

**단점**

비용 때문에 좋은 성능 발휘 불가능

### 4. Parellel Processing System 병렬 처리 시스템

그럼 하나의 컴퓨터 안에 **CPU 여러개**를 넣자. 단일 시스템 내에서 둘 이상의 프로세서를 사용

메모리 등의 기타 자원들은 프로세서들이 공유를 함. → **Tightly Coupled System** 

목적 : 성능 향상, 신뢰성 향상 (CPU 1개가 고장나도 ok)

프로세서 간에 관계 및 역할 정리 필요

### 5. Distributed Processing System 분산 처리 시스템

CPU를 무한정으로 추가할 수 없기에, 컴퓨터들을 붙여보자

컴퓨터들을 통신 즉, 네트워크를 통해서 연결. 연결된 컴퓨터 1개를 node라고 한다.

자원들을 여러 컴퓨터들끼리 공유하므로 느슨하게 연결되었다 → **Loosely coupled system**

서버 컴퓨터, cluster system 등

**장점**

자원 공유를 통한 높은 성능

고신뢰성, 높은 확장성

**단점**

구축 및 관리가 어려움

### 6. Real-time Systems 실시간 시스템

작업 처리에 제한 시간을 갖는 시스템.

**제한 시간내에 서비스를 제공**하는 것이 주 목표. 자원 활용 효율보다도

# 운영체제의 구조

---

![Untitled (3)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/7b134bc7-76e2-4ad6-8e3a-b381cc679138)
- **커널 (Kernel)**

os의 핵심 부분(메모리에 상주하는 프로그램).

- **유틸리티 (Utility)**

커널을 제외한 프로그램들 (메모리에 비상주. 필요할 때만 올림)

HW → 커널 → System call → Utilities → 사용자 applications

## 1. 단일 구조

![Untitled (4)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/42bbfc6f-eadb-4e78-8042-97a25f87367e)
커널의 핵심 기능들을 하나의 큰 커널로 구현

**장점**

커널 내 모듈간 직접 통신 가능

효율적 자원 관리 및 사용

**단점**

커널의 거대화로 인한 오류, 버그 발생. 유지보수 어려움

한 모듈의 문제가 전체 시스템에 크게 영

## 2. 계층 구조

![Untitled (5)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/03a2fd79-7e4a-466a-a4bc-1e9cd7723de6)
커널 기능들을 모듈화하여 계층 구조로 구현

**장점**

모듈화 → 계층간 검증 및 수정 용의

설계 및 구현 단순화

**단점**

단일구조 대비 성능 저하 (원하는 기능을 위해 계층을 거쳐야하므로)

## 3. 마이크로 커널 구조

![Untitled (6)](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/c301db1d-0ead-4ca2-af67-69665ee04c7f)
커널이 계속 커지니까 문제 발생. 커널엔 진짜 필수 기능만 담자.

나머지는 사용자 영역에서 처리하자.

# 운영체제의 기능

- 프로세스 관리
- 프로세서 관리
- 메모리 관리
- 파일 관리
- 입출력 I/O 관리
- 보조 기억 장치 및 기타 주변장치 관리

**프로세스 관리**

프로세스는 커널에 등록된 실행 단위 (실행 중인 프로그램)

교착상태 (deadlock) 해결

프로세스 정보 PCB(Process Control Block) 관리

**프로세서 관리**

프로세스 스케줄링 : 프로세스 처리 순서 결정

프로세스를 어떤 CPU 프로세서에게 할당할 것인가를 결정

**메모리 관리**

multi-user, multi-tasking시에 각 프로세스들에 메모리 할당 및 회수, 메모리 여유 공간 관리, 프로세스들의 메모리 접근 영역 보호

메모리 할당 방법

1. 전체 적재 : 메모리에 프로그램 다 올리는거
2. 일부 적재 (virtual memory) : 프로그램 및 데이터 일부만 적재하여 메모리를 효율적으로 활용 가능. 원하는 프로그램 및 데이터가 없을 때 보조기억 장치 추가적으로 접근해야함

**파일 관리**

**입출력 관리**

프로세스가 I/O 장치에 접근하려면 반드시 OS를 거쳐야함.
