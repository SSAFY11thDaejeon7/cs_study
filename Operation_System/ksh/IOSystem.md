# I/O Mechanisms
## Processor controlled memory access
### Pooling
- 프로세서가 주기적으로 I/O 장치의 상태 확인
  - 모든 I/O 장치를 순환하면 확인
  - 전송 준비 및 전송 상태등
- 장점: 단순함, I/O 장치가 빠르고, 데이터 전송이 잦은 경우 효율적임.
- 단점: 프로세서의 부담이 큼. (I/O 장치가 느린 경우, pooling overhead가 발생함.

### Interrupt
- I/O 장치가 작업을 완료한 후, 자신의 상태를 프로세서에게 전달함.
- 장점
  - Pooling 대비 overhead가 적음
  - 불규칙적인 요청 처리에 적합함
단점: interrupt handling overhead가 발생함.
  
## Direct Memory Access
- I/O 장치와 메모리 사이의 데이터 전송을 프로세서 개입없이 수행함
- 프로세서는 데이터 전송의 시작/종료만 관여함

![16일차-1](https://github.com/SSAFY11thDaejeon7/cs_study/assets/80624927/79706ed7-9b6a-4040-8548-ad6e2a8ab5ff)

# I/O Service of OS
- I/O Scheduling
  - 입출력 요처에 대한 처리 순서 결정
    - 시스템의 전반적 성능 향상
    - 프로세스의 요구에 대한 공평한 처리
- Error handling
  - 입출력 중 발생하는 오류 처리
- I/O device information managements

- Buffering
  - 입출력 장치와 프로그램 사이에 전송되는 데이터를 버퍼에 임시 저장
  - 전송 속도(or 처리 단위) 차이 문제 해결
- Caching
  - 자주 사용하는 데이터를 미리 복사해둠
  - Cache hit시 I/O를 생략할 수 있음
- Spooling
  - 한 I/O 장치에 여러 프로그램이 요청을 보낼 시, 출력이 섞이지 않도록 하는 기법
  - 각 프로그램에 대응하는 디스크 파일에 기록함
  - Spooling이 완료되면, spool을 한번에 하나씩 I/O 장치로 전송함

# Disk Scheduling
- 디스크 접근 요청들의 처리 순서를 결정
- 디스크 시스템의 성능을 향상
- 평가 기준
  - Throughput: 단위 시간당 처리량
  - Mean response time: 평균 응답 시간
  - Predictability: 응답 시간의 예측성, 요청이 무기한 연기되지않도록 방지
- seek time과 rotational delay를 최적화해야 함

## First Come First Service (FCFS)
- 요청이 도착한 순서에 따라 처리
- 장점: 간단하고 scheduling overhead가 적음. 공평한 처리 기법덕분에 무한 대기가 방지됨
- 단점: 최적 성능 달성에 대한 고려가 없음
- 디스크 접근 부하가 적은 경우에 적합함

## Shortest Seek Time First
- 현재 head 위치에서 가장 가까운 요청 먼저 처리
- 장점: Throughput이 높고, 평균 응답 시간이 적음
- 단점: Predictability가 적고 기아 현상 발생 가능
- 일괄처리 시스템에 적합함

## Scan Scheduling
- 현재 head의 진행 방향에서 head와 가장 가까운 요청 먼저 처리함
- (진행방향 기준) 마지막 실린더 도착 후에 반대 방향으로 진행함
- 장점: SSTF의 기아현상 문제 해결, Throughput 및 평균 응답시간 우수
- 단점: 진행 방향 반대쪽 끝의 응답 시간이 늦어짐

## C-Scan Scheduling
- Scan 스케줄링와 유사함
- Head가 미리 정해진 방향으로만 이동
  - 마지막 실린더 도착 후, 시작 실린더로 이동 후 재시작
- 장점: Scan대비 균등한 기회 제공

## Look Scheduling
- Elevator algorithm
- Scan(C-Scan)에서 현재 진행 방향에 요청이 없으면 방향 전환
  - 마지막 실린더까지 이동하지 않음
  - Scan(C-Scan)의 실제 구현 방법
- 장점: Scan의 불필요한 이동 head 제거

## Shortest Latency Time First
- Fixed head disk 시스템에 사용
  - 각 track마다 head를 가진 disk, Head의 이동이 없음
- Sector queuing algorithm
  - 각 sector별 queue 유지
  - Head 아래 도착한 sector의 queue에 있는 요청을 먼저 처리함
- Moving head disk의 경우
- 같은 실린더 또는 track에 여러 개의 요청 처리를 위해 사용 가능
  - Head가 특정 실린더에 도착하면, 고정 후 해당 실린더의 요청을 모두 처리

## Shortest Positioning Time First
- Positioning time = Seek time + rotational delay
- Positioning time이 가장 작은 요청 먼저 처리
- 장점: Throughput이 높고, 평균 응답 시간이 적음
- 단점: 가장 안쪽과 바깥쪽 실린더의 요청에 대해 기아 현상이 발생 가능함
- Eschenbach scheduling
  - Positioning time 최적화 시도
  - Disk가 1회전하는동안 요청을 처리할 수 있도록 요청을 정렬

# RAID Architecture
- Redundant Array of Inexpensive Disks
- 여러개의 물리 디스크를 하나의 논리 디스크로 사용
- 디스크 시스템의 성능 향상을 위해 사용함

## RAID 0
- Disk striping: 논리적인 한 블록을 일정한 크기로 나누어서 각 디스크에 나누어 저장
- 모든 디스크에 입출력 부하 균등 부냅
  - 병렬 접근, 성능 향상
- 한 디스크에서 장애시, 데이터 손실 발생

## RAID 1
- Disk mirroring
  - 동일한 데이터를 mirroring disk에 중복 저장
- 최소 2개의 disk로 구성
- 한 disk에 장애가 생겨도 데이터 손실 X
- 가용 disk 용량 = 전체 disk 용량 / 2

## RAID 3
- RAID 0 + parity disk
  - 바이트 단위 분할 저장
  - 모든 디스크에 입출력 부하 균등 분배
- 한 디스크에 장애 발생 시, perity 정보를 이용하여 복구
- Write시 parity 계산 필요

## RAID 4
- RAID3과 유사, 단 Block 단위로 분산 저장
  - 독립된 접근 방법
  - 디스크간 균등 분배가 안될 수도 있음
  - 디스크간 균등 분배가 안될 수도 있음
  - 한 디스크에 장애 발생 시, parity 정보를 이용하여 복구
  - Write시 parity 계산 필요
- 병목 현상으로 성능 저하 가능

## RAID 5
- RAID4와 유사
- Parity 정보를 각 디스크들에 분산 저장
- 현재 가장 널리 사용되는 RAID level중 하나
- 
