# HW solution
### TestAndSet(TAS) instruction
-  Test와 Set을 한번에 수행하는 기계어
-  Machine instruction
  - 실행 중 인터럽트를 받지 않는다(선점되지 않음)
- Busy waiting
- 장점 : 구현이 간단
- 단점 : Busy waiting
  - Busy waiting 문제를 해소한 상호배제 기법 -> Semaphore
# OS solution
### Spinlock
<img width="482" alt="스크린샷 2024-01-29 오후 11 53 16" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/7eade7db-a9df-4a08-8cbe-2b448acc8686">

<img width="463" alt="스크린샷 2024-01-29 오후 11 36 25" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/a7be8b70-5cf2-4c5a-a54b-637b15367cf3">

- 정수 변수
- 초기화, P(), V() 연산으로만 접근 가능
- 멀티 프로세서 시스템에서만 사용 가능
- Busy waiting가 아직 존재

## Semaphore
- 음이 아닌 정수형 변수
  - 초기화 연산, P(), V()로만 접근 가능
    - P : Probern(검사)
    - V : Verhogen(증가)
- **임의의 S변수 하나에 ready queue 하나가 할당 됨**
### 종류
- Binary semaphore : 상호배제나 프로세스 동기화 목적으로 사용
- Countion semaphore : Producer-Consumer 문제 등을 해결하기 위해 사용
##### 초기화 연산
- S 변수에 초기값을 부여하는 연산
#### P() 연산, V() 연산
<img width="558" alt="스크린샷 2024-01-29 오후 11 41 56" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/6a3e1796-8e05-4153-a9b1-5fbdb510e253">

#### 모두 indivisible 연산
- OS support
- 전체가 한 instruction cycle에 수행 됨

### Semaphore를 사용하여 해결하는 문제
#### Process synchronization
- Process들의 실행 순서 맞추기
  - 프로세스들은 병행적이며, 비동기적으로 수행
<img width="974" alt="스크린샷 2024-01-29 오후 11 55 57" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d3ddb561-b1f7-4707-847a-3cd09deb7766">

#### Producer-Consumer problem
- 생산자(Producer) 프로세스
  - 메시지를 생성하는 프로세스 그룹
- 소비자(Consumer) 프로세스
  - 메세지 전달받는 프로세스 그룹
<img width="835" alt="스크린샷 2024-01-29 오후 11 54 39" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/7421700e-a817-4963-945f-f5c6a36b42bc">

<img width="940" alt="스크린샷 2024-01-29 오후 11 47 11" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/66414c08-39cb-4313-a5d3-8dc8575ba285">

#### Reader-Writer problem
- Reader
  - 데이터에 대해 읽기 연산만 수행
- Writer
  - 데이터에 대해 갱신 연산을 수행
- 데이터 무결성 보장 필요
  - Reader들은 동시에 데이터 접근 가능
  - Writer들이(또는 reader와 write) 동시에 데이터 접근 시, 상호배제(동기화)필요
- 해결법
  - reader / writer 에 대한 우선권 부여
<img width="1039" alt="스크린샷 2024-01-29 오후 11 49 55" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/a15555da-070f-4812-ad02-92e60c5d2f10">

### Semaphore 대표적 특징
- No busy waiting
  - 기다려야 하는 프로세스는 block(asleep) 상태가 된다.
- Semaphore queue에 대한 wake-up 순서는 비결정적이다.
