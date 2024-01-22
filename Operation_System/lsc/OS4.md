# [OS] Lecture 4. Thread management

## 프로세스(Process)와 스레드(Thread)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/0b6976ae-aebe-4c02-9908-b77dafaa70b5)

- 프로세스는 자원(Resource)을 **제어**하고, 자원은 프로세스에 할당된다.
- 제어 = 스레드

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/ca3d8a0b-c644-4514-a452-34717996cdbb)

- 프로세스
  - 제어 정보(SP, PC, 상태 등), 지역 데이터, 스택
- 리소스
  - 코드, PC, 전역 데이터, 힙
 
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/29b9b523-f0bd-4a2c-8743-df2656781c36)

- 하나의 프로세스 안에 여러 개의 스레드가 존재할 수 있고 각자 제어할 수 있다.
- 같은 프로세스의 스레드들은 동일한 주소 공간 공유

## 스레드(Thread)

- Light Weight Process (LWP)
- 프로세서(e.g, CPU) 활용의 기본 단위
- 구성요소
  - Thread ID
  - Register set (PC, SP 등)
  - Stack (i.e. local data)
- 제어 요소 외 코드, 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유
- 전통적 프로세스 = 단일 스레드 프로세스

## Single-thread vs Multi-threads
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/5f2af204-b866-48c4-bd66-f7af6d55bc79)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/a74cec53-b9a3-4cdc-9185-68f0fa7b7116)

## 스레드의 장점
- 사용자 응답성 (Responsiveness)
  - 일부 스레드의 처리가 지연되어도, 다른 스레드는 작업을 계속 처리 가능
- 자원 공유 (Resource sharing)
  - 자원을 공유해서 효율성 증가 (커널의 개입을 피할 수 있음)
    - 예) 동일 address space에서 스레드 여러 개
    - context switch가 발생하지 않음
- 경제성 (Economy)
  - 프로세스의 생성, context switch에 비해 효율적
- 멀티 프로세서(multi-processor) 활용
  - 병렬처리를 통해 성능 향상

## 스레드 사용의 예
- 스레드를 분리시킴으로써 여러 작업을 동시에 처리할 수 있다.

## 사용자 수준 스레드 (User Thread)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/22bae578-7b31-4fa4-891e-0935cabd2511)

- 사용자 영역의 스레드 라이브러리로 구현됨
  - 스레드의 생성, 스케줄링 등
  - POSIX threads, Win32 threads, Java threads API 등
- 커널은 스레드의 존재를 모름
  - 커널의 관리(개입)를 받지 않음
    - 생성 및 관리의 부하가 적음, 유연한 관리 기능
    - 이식성(portability)이 높음
  - 커널은 프로세스 단위로 자원 할당
    - 하나의 스레드가 block 상태가 되면, 모든 스레드가 대기 (Single-threaded kernel의 경우)

## 커널 수준 스레드 (Kernel Threads)
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/81a20fba-d100-4240-8dba-cff875c39913)

- OS(Kernel)가 직접 관리
- 커널 영역에서 스레드의 생성, 관리 수행
  - Context switching 등 부하(Overhead)가 큼
- 커널이 각 스레드를 개별적으로 관리
  - 프로세스 내 스레드들이 병행 수행 가능
    - 하나의 스레드가 block 상태가 되어도, 다른 스레드는 계속 작업 수행 가능

## Multi-Threading Model
- **다대일(n:1)** 모델
  - 사용자 수준 스레드
- **일대일(1:1)** 모델
  - 커널 수준 스레드
- **다대다(n:m)** 모델
  - n > m
  - 혼합형 스레드

## 혼합형 (n:m) 스레드
![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/68500724/7d18ba99-72bb-47da-a68b-8087b4b29014)

- n개 사용자 수준 스레드 - m개의 커널 스레드 (n > m)
  - 사용자는 원하는 수 만큼 스레드 사용
  - 커널 스레드는 자신에게 할당된 하나의 사용자 스레드가 block 상태가 되어도 다른 스레드 수행 가능
    - 병행 처리 가능
- 효율적이면서도 유연함
