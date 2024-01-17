## 운영체제 역할
<h3>1. 사용자 인터페이스 (편리성)</h3>

  - CUI (Character user interface) : 문자 입력 형태
  - GUI (Graphical user interface) : 그림 형태
  - EUCI ( End-User Confortable Interface ) : 특별한 목적을 위해 만들어진 UI
      - ex) MP3 UI
   
<h3>2. 자원 관리 ( 효율성 )</h3>

  - HW resource
    - processor, memory, I/O devices 등
  - SW resource
    - file, application, message, signal 등

<h3>3. 프로세스 및 스레드 관리</h3>
  
<h3>4. 시스템 보호</h3>

   - 사용자가 불법적인 형태로 시스템을 사용하려고 할 때, 이를 보호하는 역할 수행
<br>

## 컴퓨터 시스템 구성
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/bd9ce82f-a811-485a-af30-4a3c2d04dbb2" width="500px;" alt=""/>
<br>

- System Call Interface와 Kernel 2개의 층이 OS이다.
- 하드웨어를 관리하면서 사용자에게 서비스를 제공하는 역할을 수행한다.
- System Call Interface
  - 사용자가 직접 Kernel에 접근하여 마음대로 조작하면 OS에 문제가 발생할 수 있다.
    이를 방지하기 위해 사용자가 필요한 기능이 있을 때 OS에 요청하는 중간 터널이 System Call Interface이다.
  - 커널이 제공하는 기능들 중 사용자가 접근할 수 있는 기능들
<br>

 <img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/4e34d7a3-16e0-404e-b73e-90c5d0205c25" width="500px;" alt=""/>

 - 위 그림에서 시스템 라이브러리가 시스템 콜 인터페이스이다.
<br>

## 운영체제의 구분
<h3>1. 동시 사용자 수</h3>

  - 단일 사용자 ( Single-user system )
      - 한 명의 사용자만 시스템 사용 가능
          - 한 명의 사용자가 모든 시스템 자원 독점
          - 자원관리 및 시스템 보호 방식이 간단함
      - 개인용 장비에 많이 사용 ( Windows, Android, IOS 등 )
  <br>
  
  - 다중 사용자 ( Multi-user system )
      - 동시에 여러 사용자들이 시스템 사용
          - 각종 시스템 자원들에 대한 소유 권한 관리 필요
          - 기본적으로 Multi-tasking 기능 필요
          - os의 기능 및 구조가 복잡
      - 서버, 클러스터 장비등에 사용 ( Unix, Linux 등 )
    <br>

<h3>2. 동시 실행 프로세스 수</h3>

  - 단일 작업( Single-tasking system )
    - 시스템 내에 하나의 작업 프로레스만 존재
    - 운영체제 구조 간단 ( MS-DOS 등 )
   <br>
   
  - 다중 작업 ( Multi-tasking system )
    - 동시에 여러 작업(프로세스) 수행 가능
    - 운영체제 구조 복잡 ( Unix, Linux, Windows 등 )
<br>

## 작업 수행 방식
<h3>1. 일괄 처리 시스템 ( Batch System )</h3>

- 모든 시스템을 중앙에서 관리 및 운영
- 사용자의 요청 작업을 일정 시간 모아 두었다가 한번에 처리
<br>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/1a138dee-b988-458c-b4d4-71ac31932874" width="500px;" alt=""/>
<br>

<특징>
<br>

- 시스템 지향적
- 장점
  - 많은 사용자가 시스템 자원 공유
  - 처리 효율 향상
- 당점
  - 생산성 저하
    - 같은 유형의 작업들이 모이기를 기다려야함
  - 긴 응답시간 ( turnaround time )
<br>

<h3>2. 시분할 시스템 ( Time Sharing Systems ), 1960s ~ 1970s</h3>

- 여러 사용자가 자원을 동시에 사용
  - OS가 파일 시스템 및 가상 메모리 관리
- 사용자 지향적
  - 대화형 시스템
  - 단말기 사용

 <img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/9723c73d-fe34-40fa-961b-4ba1d3b14b59" width="500px;" alt=""/>
 <br>
 
<장점>
  - 응답시간(response time) 단축
  - 생산성 향상
    - 프로세서 유휴 시간 감소
<br>

<단점>
  - 통신 비용 증가
      - 통신성 비용, 보안 문제
  - 개인 사용자 체감 속도 저하
    - 동시 사용자수 증가 -> 시스템 부하 -> 느려짐 (개인 관점에서)

<h3>3. 개인 시스템 ( Personal Computing)</h3>

- 개인이 시스템 전체 도점
- CPU 활용률 고려 대상 x
- OS가 상대적으로 단순

- 장점 : 빠른 응답시간
- 단점 : 성능 낮음

<h3>4. 병렬처리 시스템 ( Parallel Processing System )</h3>

- 단일 시스템 내에서 둘 이상의 프로세서 사용
- 메모리 등의 자원을 서로 공유
- 프로세서 간 관계 및 역할 관리 필요

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5eb56f05-f8ab-4e51-a16b-abc60c60525b" width="500px;" alt=""/>
<br>

[사용목적]<br>

- 성능 향상 : 하나보다 많은 프로세서 사용으로 처리 속도 향상
- 신뢰성 향상 : 하나가 고장나도 정상 동작
<br>

<h3>5. 분산 처리 시스템 ( Distributed Processing System ) </h3>

네트워크를 기반으로 구축된 병렬처리 시스템
<br>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/6fad6dc9-45ec-49c0-98fb-31135861e940" width="500px;" alt=""/>
<br>

- 물리적인 분산, 통신망 이용한 상호 연결 ( 각 노드들을 연결시키기 쉽다 )
- 각각 운영체제 탑재한 다수의 범용 시스템 구성
- 사용자는 분산운영체제를 통해 하나의 프로그램, 자원처럼 사용 가능
- 각 구성 요소들 간의 독립성 유지 및 공동작업 가능

[장점]
<br>

- 자원 공유를 통한 높은 성능
- 고 신뢰성, 높은 확정성
<br>
[단점]
<br>
- 구축 및 관리가 어려움
<br>

<h3>6. 실시간 시스템 ( Real-time Systems )</h3>

작업 처리에 제한 시간(deadline)을 갖는 시스템
- 작업의 종류
  - Hard real-time task
      - 시간 제약을 지키지 못하는 경우 시스템에 치명적 영향
      - ex) 발전소, 무기 제어 등
  - Soft real-time task
      - 동영상 재생 등
  - Non real-time task
    
## 운영체제의 구조
- 커널 ( Kernel )
  - OS의 핵심 부분 ( 메모리 상주 프로그램 )
  - 가장 빈번하게 사용되는 기능들 담당
  - 동의어 : 핵, 관리자 프로그램, 상주 프로그램, 제어 프로그램 등
  - 자원(Resource) 관리

- 유틸리티 ( Utility )
  - 비상주 프로그램
  - 커널을 제외한 나머지 부분들 ex) UI 등

<h3>1. 단일 구조</h3>

커널이 하나로 이루어진 구조
<br>

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/80046765-c24c-4cbf-823d-95d451820ee0" width="500px;" alt=""/>
<br>

[장점]<br>
  - 커널 내 모듈간 직접 통신으로 효율적 자원 관리 및 사용 가능
<br>

[단점]<br>

  - 커널의 거대화
    - 오류 및 버그, 추가 기능 구현 등 유지보수가 어려움
    - 동일 메모리에 모든 기능이 있어, 한 모듈의 문제가 전체 시스템에 영향
   
<h3>2. 계층 구조</h3>

커널은 기능별로 계층적으로 묶은 운영체제 구조
<br>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/68e26759-10cb-40fb-809c-b390ded2bed7" width="500px;" alt=""/>
<br>

[장점]
<br>
  - 모듈화
      - 계층간 검증 및 수정 용이
  - 설계 및 구현의 단순화

<br>
[단점]
<br>

  - 단일구조 대비 성능 저하
      - 원하는 기능 수행을 위해 여러 계층을 거쳐야 함
<br>

<h3>3. 마이크로 커널 구조</h3>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5a8e972f-c0ef-4594-81e0-630020523176" width="500px;" alt=""/>

  - 커널의 크기 최소화
    - 필수 기능만 포함
    - 기타 기능은 사용자 영역에서 수행
<br>

## 운영체제의 기능
  1. 프로세스 관리
     - 프로세스
      - 커널에 등록된 실행 단위
      - 사용자 요청/프로그램의 수행 주체
  
  - OS의 프로세스 관리 기능
     - 생성/삭제, 상태 관리
     - 자원 할당
     - 프로세스 간 통신 및 동기화
     - 교착상태(deadlock) 해결 : 프로세스가 하나의 프로세서를 두고 서로 차지하려고 할 때 발생
       
  - 프로세스 정보 관리를 위해 사용
     - PCB ( Process Control Block )
  
  2. 프로세서 관리
    - 중앙 처리 장치(CPU)
       - 프로그램을 실행하는 핵심 자원

    - 프로세스 스케줄링
       - 시스템 내의 프로세스 처리 순서 결정

    - 프로세서 할당 관리
      - 프로세스들에 대한 프로세서 할당
        - 한 번에 하나의 프로세스만 사용 가능

  3. 메모리 관리
     - 주기억 장치
         - 작업을 위한 프로그램 및 데이터를 올려 놓는 공간
     - Multi-User, Multi-tasking 시스템관리
     - 메모리 할당 방법 관리
    
  4. 파일 관리
    - 파일
       - 논리적 데이터 저장 단위
    - 사용자 및 시스템의 파일 관리
    - 디렉토리 구조 지원
    - 파일 관리 기능
      - 파일 및 디렉토리 생성/삭제
      - 파일 접근 및 조작
      - 파일을 물리적 저장 공간으로 사상
      - 백업 등
    
  5. 입출력 관리
     - OS를 반드시
  <img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/382218c7-c56e-4f08-92f5-c1a1eed8174a" width="500px;" alt=""/>
