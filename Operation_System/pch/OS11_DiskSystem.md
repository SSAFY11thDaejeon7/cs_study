# 파일 시스템

## Disk System
- Disk pack
  - 데이터 영구 저장 장치(비휘발성)
  - 구성
    - Sector : 데이터 저장/판독의 물리적 단위
    - Track : Platter 한 면에서 중심으로 같은 거리에 있는 sector들의 집합
    - Cylinder : 같은 반지름을 갖는 track의 집합
    - Platter : 양면에 자성 물질을 입힌 원형 금속판, 데이터의 기록 판독이 가능한 기록 매체
    - Surface : Platter의 윗면과 아랫면
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/c7431a27-3f4d-47f3-8271-35dd67f2a664" width="300px;" alt=""/>

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/ac273619-4440-4b08-a60a-b7e2704b23c6" width="300px;" alt=""/>

## Disk drive
- Disk pack에 데이터를 기록하거나 판독할 수 있도록 구성된 장치
- 구성
  - Head : 디스크 표면에 데이터를 기록/판독
  - Arm : Head를 고정/지탱
  - Positioner(booom) : Arm을 지탱, Head를 원하는 track으로 이동
  - Spindle : Disk pack을 고정하는 회전축, 분당 회전수가 성능 영향

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/6c9b68a8-c583-4e23-8b17-c670c7fac45a" width="300px;" alt=""/>

## Disk Address
- Physical disk address : sector (물리적 데이터 전송 단위)를 지정
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/38216211-1d70-4cf0-a7df-d74c0f138e1a" width="500px;" alt=""/>

<br>

- Logical disk address (relative address) : Disk system의 데이터 전체를 block들이 나열로 취급
  - Block에 번호 부여
  - 임의의 block에 접근 가능
 
## Data Access in Disk System
- Sekk time : 디스크 head를 필요한 cylinder로 이동하는 시간
- Rotational delay : Seek time 이후부터 필요한 sector가 head 위치로 도착하는 시간
- Data transmission time : Rotational delay 이후부터, 해당 sector를 읽어서 전송 또는 기록하는 시간


  - Driver : Block 번호를 실제 물리 디스크 주소로 매핑해주는 역할 수행, 하드웨어 제조사들이 제공
- Block 번호 : physical address 모듈 필요
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/2908fc8b-349d-4018-9173-4bd63092603c" width="500px;" alt=""/>

- Disk Address Mapiing 과정
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/885fb1c5-94a4-4b7b-bceb-187c7d3d79c2" width="500px;" alt=""/>

## File System
- 사용자들이 사용하는 파일들을 관리하는 운영체제의 한 부분
- 구성
  - Files : 연관된 정보의 집합
  - Directory structure : 시스템 내 파일들의 정보를 구성 및 제공, File들을 분류 및 보관하기 우핸 개념
  - Partitions : Directory들의 집합을 논리적/물리적으로 구분(Virtual disk)

## File Concept
- 보조 기억 장치에 저장된 연관된 정보들의 집합
  - 보조 기억 장치 할당의 최소 단위
  - Sequence of bytes(물리적 정의)
 
- 내용에 따른 분류
  - Program file : source program, object program, executable files 등
  - Data file

- 형태에 따른 분류
  - Text(ascii) file
  - Binary file

## File Access Methods
- Sequential access(순차 접근) : File을 record 또는 bytes 단위로 순서대ㅗㄹ 접근
- Directed access(직접 접근) : 원하는 Block을 직접 접근
- Indexed access : Index를 참조하여, 원하는 block을 찾은 후 데이터에 접근
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/e81686e6-9b2b-4da9-b764-0289d0b6c957" width="300px;" alt=""/>

<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/2af805f3-44e6-4b04-9721-d8c926269686" width="300px;" alt=""/>

- Mounting : 현재 File System에 다른 File System을 붙이는 것
- Mount point : Mounting 지점
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/83ea4a6f-809e-4ed3-ba25-ea83ea0c88f8" width="500px;" alt=""/>

## Directory Structure
<h2>Flat Directory Structure : FS 내에 하나의 directory만 존재</h2>

  - Single-level directory structure
  - Issues
    - File naming
    - File protection
    - File management
    - 다중 사용자 환경에서 문제가 더욱 커짐
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/4d486fe1-2cc6-4a2a-8156-494d2ebbe0c5" width="500px;" alt=""/>
<br>

<h2>2-Level Directory Structure : 사용자 마다 하나의 directory 배정</h2>
  - 문제
    - Sub-directory 생성 불가
    - 사용자간 파일 공유 불가
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/761d4148-858a-46b2-8fd9-23966b0bd1dd5" width="500px;" alt=""/>

<h2>Hierarchical Directory Structure</h2>
- Tree 형태의 계층적 directory 사용 가능
- 사용자가 하부 directory 생성 및 관리 가능
  - os가 system call을 제공하여야 함
- 대부분의 OS가 사용
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/a6f4fca1-425e-4bee-b3f9-762086441415" width="500px;" alt=""/>
<br>

- Acyclic Graph Directory Structure
  - Hierarchical Directory Structure 확장판
  - Link 개념 사용하였으며, 사이클이 허용되지 않는 파일 구조

- General Graph Directory Structure
  - Hierarchical Directory Structure 확장판
  - Link 개념 사용하였으며, 사이클이 허용되는 파일 구조
  - FIle 탐색 시, 무한 루프를 고려하여야 함


# File Protection
- File에 대한 부적절한 접근 방지
  - 다중 사용자 시스템에서 더욱 필요
- 접근 제어가 필요한 연산들 : Read, Write, Execute, Append

## File Protection Mechanism
- 파일 보호 기법은 system size 및 응용 분야에 따라 다를 수 있음
1. Password 기법
   - 각 file들에 PW 부여
   - 비현실적
     - 사용자가 파일 각각에 대한 PW를 기억해야 함
     - 접근 권한 별로 서로 다른 PW를 부여해야 함

2. Access Matrix 기법

## Access Matrix
- 사용자(domain)와 파일(object)사이의 접근권한을 명시
- 용어
  - Object : 접근 대상
  - domain : 접근 권한의 집합, 같은 권한을 가지는 그룹(사용자, 프로세스)
  - Access right : 권한
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/d18a4ef9-1fec-45df-b283-7f79c5d0903b" width="500px;" alt=""/>

- 시스템 전체 file들에 대한 권한을 table로 유지
- 단점 : 권한 설정이 없는 부분까지 불필요하게 메모리를 차지한다.

## Access List
- Access matrix의 열을 list로 표현
  - 각 object(파일)에 대한 접근 권한을 나열
- Object 생성 시, 각 domain에 대한 권한 부여
- Object접근 시 권한을 검사
- 실제 OS에서 많이 사용됨

## Capability List
- 각 domain(사용자 및 프로세스)가 가진 접근 권한을 list로 표현
- Capability를 가짐이 권한을 가짐을 의미
- Kernel안에 저장되기 때문에 시스템이 capability list 자체를 보호해야 함

## 기법 별 정리
- Global table
  - 간편, 그러나 큰 메모리 차지
- Access list
  - Object 별 권한 관리가 용이
  - 모든 접근마다 권한을 검사해야 함, Object에 많이 접근하는 경우 느림
- Capability list
  - List 내 object들에 대한 접근에 유리
  - Object별 권한 관리(권한 취소 등)가 어려움
 <br>

 - 많은 OS가 Access list와 capability list개념을 함께 사용
   - Object에 대한 첫 접근은 access list로 탐색
   - 접근 허용 시, Capability 생성 후 해당 사용자(프로세스)에게 전달
     - 이후 Capability를 가지고 있는 사용자(프로세스)는 권한 검사 불필요
   - 사용자(프로세스)가 Object에 마지막으로 접근 후, 가지고 있는 Capability 삭제
  
# File System Implementation
- Allocation methods : File 저장을 위한 디스크 공간 할당 방법
- Free space management : 디스크의 빈 공간 관리

## Allocation methods
1. Continuous Allocation
  - 한 File을 디스크의 연속된 block에 저장
  - 장점
    - 효율적인 file 접근(순차, 직접 접근)
  - 단점
    - 새로운 file을 위한 공간 확보가 어려움
    - 외부 단편화
<br>

2. Linked Allocation
- File이 저장된 block들을 linked list로 연결
  - 비연속 할당 가능
- Directory는 각 file에 대한 첫 번째 block에 대한 포인터를 가짐
- 장점
  - 구조가 간단하고 외부 단편화 발생 x
- 단점
  - 직접 접근에 비효율적
  - 포인터 저장을 위한 추가 공간 필요
  - 사용자가 포인터를 실수소 건드리는 등 신뢰성 문제

## FAT
- File Allocation Table(FAT) : 각 block의 시작 부분에 다음 블록의 번호를 기록하는 방법
- Discontinuous Allocation
- MS-DOS, Windows 등에 사용

## Indexed Allocation
- File이 저장된 block의 정보(pointer)를 Index block에 모아 둠
- Discontinuous Allocation
- 직접 접근에 효율적, 순차 접근엔 비효율적
- File 당 Index block을 유지
  - space overhead
  - Index block 크기에 따라 파일의 최대 크기가 제한 됨

## Free Space Management
1. Bit Vector
- 시스템 내 모든 block들에 대한 사용 여부를 1 bit flag로 표시
- 간편하고 효율적
- Bit vector 전체를 메모리에 보관해야 하기 떄문에 대형 시스템에 부적합
<br>

2. Linked List
- 빈 block을 하나의 linked list로 연결
- 공간적으로도, 탐색적으로도 비효율적
<br>

3. Gruoping
- n개의 빈 block을 그룹으로 묶고, 그룹 단위로 linked list로 연결
- 연속된 빈 block을 쉽게 찾을 수 있음

4. Counting
- 연속된 빈 block들 중 첫 번째 block의 주소와 연속된 block의 수를 table로 유지
- Continuous allocation 시스템에 유리한 기법
