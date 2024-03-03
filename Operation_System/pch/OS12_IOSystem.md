# 입출력 시스템 & 디스크 관리

# I/O Mechanisms
1. Processor controlled memory access
  - Polling (Programmed I/O)
  - Interrupt
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/dd19153d-e5cc-4269-8808-25ae1eaf01f6" width="400px;" alt=""/>

2. Direct Memory Access(DMA)

## Processor Controlled Memory Access
1. Pooling (Programmed I/O)
- Processor가 주기적으로 I/O 장치의 상태 확인
  - 모든 I/O 장치를 순환하면 확인
  - 전송 준비 및 전송 상태 등
- 장점
  - 간단하며 I/O 장치가 빠르고, 데이터 전송이 잦은 경우 효율적
- 단점
  - Pooling overhead로 인해 Processor이 부담이 큼
 <br>

2. Interrupt
- I/O 장치가 작업을 완료한 후, 자신의 상태를 Processor에게 전달
  - Interrupt 발생 시, Processor는 데이터 전송 수행
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5a922ba7-26e2-43f0-a10c-e23688713713" width="400px;" alt=""/>

- 장점
  - Pooling 대비 낮은 overhead
  - 불규칙적인 요청 처리에 적합
- 단점
  - Interrupt handling overhead
<br>

## Direct Memory Access(DMA)
- Processor controlled memory access 방법은 Processor가 모든 데이터 전송을 처리해야 했다.
  - High overehead for the processor
- Direct Memory Access(DMA)
  - I/O 장치와 Memory 사이의 데이터 전송을 Processor 개입없이 DMA로 수행
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/eb75a564-a8cc-40ac-a7bd-e079168dd59f" width="400px;" alt=""/>
<br>

- Processor는 데이터 전송의 시작과 종료에만 관여
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/155521c9-06c2-40dc-b804-f7575208239e" width="700px;" alt=""/>

# I/O Services of OS
1. I/O Scheduling
   - 입출력 요청에 대한 처리 순서 결정
     - 시스템 전반적 성능 향상
     - Process의 요구에 대한 공평한 처리
2. Error handling
   - 입출력 중 발생하는 오류 처리
3. I/O device information managements
4. Buffering
   - I/O 장치와 Program 사이에 전송되는 데이터를 Buffer에 임시 저장
   - 전송 속도(처리 단위) 차이 문제 해결
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/16e67715-1d18-4984-8280-caa43571760c" width="700px;" alt=""/>

5. Caching
   - 자주 사용하는 데이터를 미리 복사해 둠
   - Cahche hit시 I/O를 생략 할 수 있음
6. Spooling
   - 한 I/O 장치에 여러 Program이 요청을 보낼 시, 출력이 섞이지 않도록 하는 기법
     - 각 Program에 대응하는 disk file에 기록(Spooling)
     - Spooling이 완료되면, spool을 한번에 하나씩 I/O 장치로 전송

# Disk Scheduling
- Disk access 요청들의 처리 순서를 결정
- Disk system의 성능을 향상
- 평가 기준
  - Throughput : 단위 시간당 처리량
  - Mean response time : 평균 응답 시간
  - Predictability : 응답 시간의 예측성, 요청이 무기한 연기(Startvation)되지 않도록 방지

- Data access time = Seek time + Rotational delay + Data transmission time
  - Seek time : 디스크 head를 필요한 cylinder로 이동하는 시간
  - Rotational delay : Seek time 이후 필요한 sector가 head 위치로 도착하는 시간
  - Data transmission time : Rotational delay 이후 해당 sector를 읽어서 전송 또는 기록하는 시간
<br>

<h2>Seek Time에 적용되는 Disk scheduling 기법</h2>

1. First Come First Service(FCFS)
   - 요청이 도착한 순서에 따라 처리
   - 장점
     - 간단하여 overhead 낮음
     - 공평한 처리 기법으로 무한대기(starvation) 방지
   - 단점
     - 최적 성능 달성에 대한 고려가 없음
   - Disk access 부하가 적은 경우 적합
<br>

2. Shortest Seek Time First(SSTF)
  - 현재 head 위치에서 가장 가까운 요청 먼저 처리
  - 장점
    - Throughput 상승
    - 평균 응답 시간 감소
  - 단점
    - Predictability 감소
    - Starvation(무한 대기) 현상 발생 가능
  - 일괄처리 시스템에 적합
<br>

3. Scan Scheduling
  - 현재 head의 진행 방향에서, head와 가장 가까운 요청 먼저 처리
  - (진행방향 기준) 마지막 cylinder 도착 후, 반대 방향으로 진행
  - 장점
    - SSTF의 starvation 문제 해결
    - Throughput 및 평균 응답시간 우수
  - 단점
    - 진행 방향 반대쪽 끝의 요청들의 응답시간 상승
<br>

4. C-Scan Scheduling
   - SCAN과 유사
   - Head가 미리 정해진 방향으로만 이동
     - 마지막 cylinder 도착 후, 시작 cylinder로 이동 후 재시작
   - 장점
     - Scan대비 균등한 기회 제공
<br>

5. Look Scheduling
   - Elevator algorithm
   - Scan(C-Scan)에서 현재 진행 방향에 요청이 없으면 방향 전환
     - 마지막 cylinder까지 이동하지 않음
     - Scan(C-Scan)의 실제 구현 방법
   - 장점
     - Scan의 불필요한 head 이동 제거
<br>

<h2>Rotational Delay에 적용되는 Disk scheduling 기법</h2>

1. Shortest Latency Time First(SLTF)
- Fixed head disk 시스템에 사용
  - 각 track마다 head를 가진 disk
  - head의 이동이 없음
- Sector queuing algorithm
  - 각 sector별 queue 유지
  - Head 아래 도착한 sector의 queue에 있는 요청을 먼저 처리함
- 만약 track마다 head가 없어 이동해야하는 Moving head disk인 경우
  - cylinder에 도착하면, 고정 후 해당 cylinder의 모든 요청을 처리
<br>

2. Shortest Positioning Time First(SPTF)
- Positioning time = Seek time + rotational delay
- Positioning time이 가장 작은 요청 먼저 처리
- 원하는 위치에 head를 가져다 놓는 작업
- 장점
  - Throughput 증가, 평균 응답 시간 감소
- 단점
  - 가장 안쪽과 바깥쪽 cylinder의 요청에 대해 starvation 현상 발생 가능
<br>

# RAID Architecture
- Redundant Array of Inexpensive Disks (RAID)
- 여러 개의 물리 disk를 하나의 논리 disk로 사용
- Disk system의 성능 향상을 위해 사용
  - Performance(access speed) 와 Reliability(신뢰성) 향상
<br>

1. RAID 0
- Disk striping : 논리적인 한 block을 일정한 크기로 나누어 각 disk에 나누어 저장
- 모든 disk에 입출력 부하 균등 분배
  - 병렬 접근으로 성능 향상
- 한 Disk에서 장애 발생 시, 데이터 손실 발생
    - 낮은 신뢰성
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/a7cd5dee-6d1a-4502-8c77-75ebe731aadc" width="400px;" alt=""/>
<br>

2. RAID 1
- Disk mirroring : 동일한 데이터를 mirroring disk에 중복 저장
- 최소 2개의 disk로 구성
  - 입출려은 둘 중 어느 disk에서도 가능
- 한 disk에 장애가 생겨도 데이터 손실 발생 x, 높은 신뢰성
- 가용 disk 용량 = 전체 disk 용량 / 2
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/de7bf1fe-dfa9-4f93-8617-f413298ec035" width="300px;" alt=""/>
<br>

3. RAID 2
- RAID 0 + parity disk
  - Byte 단위 분할 저장
  - 모든 disk에 입출력 부하 균등 분배
    - 병렬 처리로 성능 향상
- 한 disk에 장애 발생 시, parity 정보를 이용하여 복구
- Write시 parity 계산 필요
  - overhead
  - Write가 몰릴 시, 하나의 disk에 접근량이 많아지면서 병목 현상 발생 가능
  - parity를 저장한 disk 장애 발생 시, 복구 불가
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/d39c256f-b9b0-4187-9d9d-3e6c4505a148" width="700px;" alt=""/>
<br>

4. RAID 4
- RAID 3과 유사, 단 Block 단위로 분산 저장
- 독립된 access 방법
- Disk 간 균등 분배가 안될 수도 있음
- 한 Disk에 장애 발생 시, parity 정보를 이용하여 복구
- Write 시 parity 계산 필요
  - Overhead / Write가 몰릴 시 해당 disk로의 병목현상 발생 가능
- 한 disk에 입출력이 몰릴 때, 병목 현상으로 성능 저하 가능
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/39846c51-323d-463c-8aa9-573cfb7ecd30" width="700px;" alt=""/>
<br>

5. RAID 5
- RAID 4와 유사
  - 독립된 access 방법
- Parity 정보를 각 disk들에 분산 저장
  - Parity disk의 병목현상 문제 해소
- 현재 가장 널리 사용되는 RAID level 중 하나
  - High performance and reliability
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/9888b7e6-ce40-4f35-9b26-cb190abd182c" width="700px;" alt=""/>
<br>
