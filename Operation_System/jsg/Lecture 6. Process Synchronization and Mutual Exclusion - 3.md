# Eventcount/Sequencer
- 은행의 번호표와 비슷한 개념
- **No busy waiting**
- **No starvation**
- **Semaphore 보다 더 low-level control이 가능**
#### Sequencer
- 정수형 변수
- 생성시 0으로 초기화, 감소하지 않음
- 발생 사건들의 순서 유지
- ticket() 연산으로만 접근 가능
#### ticket(S)
- 현재까지 ticket() 연산이 호출 된 횟수를 반환
- Indivisible operation
#### Eventcount
- 정수형 변수
- 생성시 0으로 초기화, 감소하지 않음
- 특정 사건의 발생 횟수를 기록
- read(E), advance(E), await(E,v) 연산으로만 접근 가능
#### read(E)
- 현재 Eventcount 값 반환
#### advance(E)
- E <- E+1
- E를 기다리고 있는 프로세스를 깨움(wake-up)
#### await(E,v)
- V는 정수형 변수
- if(E<v) 이면 E에 연결된 Q에 프로세스 전달(push) 및 CPU scheduler 호출

#### Producer-Consumer problem
<img width="940" alt="스크린샷 2024-01-31 오후 11 04 34" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/2b9e9168-6030-4901-a7cb-3dca7ad463ed">

# Language-Level solution
## High-level Mechanism
- Monitor
- 사용이 쉬움
## Monitor
<img width="411" alt="스크린샷 2024-01-31 오후 11 09 27" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/976b33a3-e2f3-4134-9ea4-fcd99459caf4">

- 공유 데이터와 Critical section의 집합
- Conditional variable
  - wait(), signal() operations
### Monitor의 구조
- Entry queue(진입 큐)
  - 모니터 내의 procedure 수만큼 존재
- Mutual exclusion
  - 모니터 내에는 항상 하나의 프로세스만 진입 가능
- Information hiding(정보은폐)
  - 공유 데이터는 모니터 내의 프로세스만 접근 가능
- Condition queue(조건 큐)
  - 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
- Signaler queue(신호제공자 큐)
  - 모니터에 항상 하나의 신호제공자 큐가 존재
  - signal() 명령을 실행한 프로세스가 임시 대기
### Monitor의 자원할당 시나리오
<hr/>

- 자원 R 사용 가능
- Monitor 안에 프로세스 없음
<img width="893" alt="스크린샷 2024-01-31 오후 11 23 11" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/7ec54a4e-9a7f-4b62-9183-f927a136bc8b">
<hr/>

- 자원 R이 Pj에게 할당 됨
- 프로세스 Pk가 R 요청, Pm 또한 R요청
<img width="905" alt="스크린샷 2024-01-31 오후 11 24 30" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/17c4a22e-8157-413c-b8eb-fe1ce2ed4815">
<hr/>

- 자원 R이 Pj에게 할당 됨
- 프로세스 Pk가 R 요청, Pm 또한 R 요청
<img width="878" alt="스크린샷 2024-01-31 오후 11 26 00" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/fad625e4-c216-4c01-9abe-1d0ab75e5909">
<hr/>

- 자원 R이 Pk에게 할당 됨
- Pj가 모니터 안으로 돌아와서, 남은 작업 수행
<img width="870" alt="스크린샷 2024-01-31 오후 11 25 01" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/9db144e0-1ef7-4d9d-8c82-021f622eb57b">
<hr/>


### Problem
- Producer-Consumer Problem
- Reader-Writer Problem
  - reader/writer 프로세스들간의 데이터 무결성 보장 기법
- Dining philosopher problem
#### Dining philosopher problem
<img width="347" alt="스크린샷 2024-01-31 오후 11 37 18" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/fa44da59-fa74-44df-81e1-1f3db95c16b3">

- 5명의 철학자
- 철학자들은 생각하는 일, 스파게티 먹는 일만 반복합
- 공유 자원 : 스파게티, 포크
- 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야 함
<img width="906" alt="스크린샷 2024-01-31 오후 11 38 35" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/bf6dd9ac-5f40-45fc-95dd-5bb42b34a5a7">


## Monitor의 장단점
- 장점
  - 사용이 쉽다.
  - Deadlock 등 error 발생 가능성이 낮음
- 단점
  - 지원하는 언어에서만 사용 가능
  - 컴파일러가 OS를 이해하고 있어야 함
    - Critical section 접근을 위한 코드 생성

  
