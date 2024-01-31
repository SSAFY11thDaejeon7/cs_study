# 프로세스 동기화 & 상호배제

### Eventcount/Sequencer

- 은행의 번호표와 비슷한 개념
- 세마포의 기아 현상을 해결한다.

- Sequencer
    - 정수형 변수
    - 생성시 0으로 초기화, 감소하지 않음
    - 발생 사건들의 순서 유지
    - ticket() 연산으로만 접근 가능
- ticket(S)
    - 현재까지 ticket() 연산이 호출 된 횟수를 반환
    - Indivisible operation

- Eventcount
    - 정수형 변수
    - 생성시 0으로 초기화, 감소하지 않음
    - 특정 사건의 발생 횟수를 기록
    - read(E), advance(E), await(E, v) 연산으로만 접근 가능

- read(E)
    - 현재 Eventcount 값 반환

- advance(E)
    - E ← E+1
    - E를 기다리고 있는 프로세스를 깨움 (wake-up)

- await(E, v)
    - v는 정수형 변수
    - if (E < v)이면 E에 연결된 Q에 프로세스 전달(push) 및 CPU scheduler 호

## Monitor

- 공유 데이터와 Critical section의 집합
- Conditional variable
    - wait(), signal() operations

### Monitor의 구조

- Entry queue (진입 큐)
    - 모니터 내의 procedure 수만큼 존재
- Mutual exclusion
    - 모니터 내에는 항상 하나의 프로세스만 진입 가능
- Information hiding (정보 은폐)
    - 공유 데이터는 모니터 내의 프로세스만 접근 가능
- Condition queue (조건 큐)
    - 모니터 내의 특정 이벤트를 기다리는 프로세스가 대기
- Signaler queue (신호제공자 큐)
    - 모니터에 항상 하나의 신호제공자 큐가 존재
    - signal() 명령을 실행한 프로세스가 임시 대기

### 자원 할당 시나리오

- 자원 R 사용 가능
- Monitor 안에 프로세스 없음
- 프로세스 pj가 모니터 안에서 자원 R을 요청
- 자원 R이 pj에게 할당 됨
- 프로세스 Pk가 요청 R 요청, Pm 또한 R 요청
- Pj가 R 반환
- R_Free.signal() 호출에 의해 pk가 wakeup
- 자원 R이 Pk에게 할당 됨
- Pj가 모니터 안으로 돌아와서, 남은 작업 수행

### Reader-Writer Problem

- reader/writer 프로세스들간의 데이터 무결성 보장 기법
- writer 프로세스에 의한 데이터 접근 시에만 상호 배제 및 동기화 필요함

### 모니터 구성

- 변수 2개
    - 현재 읽기 작업을 진행하고 있는 reader 프로세스의 수
    - 현재 writer 프로세스가 쓰기 작업을 진행 중인지 표시
- 조건 큐 2개
    - reader/writer 프로세스가 대기해야 할 경우에 사용
- 프로시져 4개
    - reader(writer) 프로세스가 읽기(쓰기) 작업을 원할 경우에 호출, 읽기(쓰기) 작업을 마쳤을 때 호출

### Dining philosopher problem

- 5명의 철학자
- 철학자들은 생각하는 일, 스파게티 먹는 일만 반복함
- 공유 자원: 스파게티, 포크
- 스파게티를 먹기 위해서는 좌우 포크 2개 모두 들어야

### 장점

- 사용이 쉽다.
- Deadlock 등 error 발생 가능성이 낮음

### 단점

- 지원하는 언어에서만 사용 가능
- 컴파일러가 OS를 이해하고 있어야 함
    - Critical section 접근을 위한 코드 생
