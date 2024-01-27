# 프로세스 동기화 & 상호배제

### 다중 프로그래밍 시스템

- 여러 개의 프로세스들이 존재
- 프로세스들은 서로 독립적으로 동작
- 공유 자원 또는 데이터가 있을 때, 문제 발생 가능

## 동기화 (Synchronization)

- 프로세스들이 서로 동작을 맞추는 것
- 프로세스들이 서로 정보를 공유하는 것

## 비동기적 (Asynchronous)

- 프로세스들이 서로에 대해 모름

## 병행적 (Concurrent)

- 여러 개의 프로세스들이 동시에 시스템에 존재

- 병행 수행중인 비동기적 프로세스들이 공유자원에 동시 접근 할 때 문제가 발생할 수 있다.

## Shared data (공유 데이터)

- 여러 프로세스들이 공유하는 데이터

## Critical section (임계 영역)

- 공유 데이터를 접근하는 코드 영역 (code segment)

공유 데이터를 두개의 프로세스가 접근해 더하기 연산을 한다면?

공유 데이터가 0 이고 각 더하기 연산에 의해 2가 안될 수도 있다.

### 기계어 명령(machine instruction)의 특성

- Atomicity (원자성), Indivisible (분리불가능)
- 한 기계어 명령의 실행 도중에 인터럽트 받지 않음

간단한 더하기 연산이라도 프로세스가 실행하는 기계어 명령어는 3개의 연산으로 나눠진다. 

프로세스가 해당 명령어를 처리하다가 다른 프로세스가 선점을 하면 2번 명령어처럼  1을 더하고 저장하지 못한 상황에서 다른 프로세스가 공유 데이터 값을 변경할 수 있다.

![](https://github.com/ensk26/cs_study/assets/70767115/be3cba7c-5326-484a-9bdf-806b9670cad2)

### Race condition

- 실행 순서에 따라 결과가 달라진다.

## Mutual exclusion (상호배제)

- 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

### Mutual exclusion primitive

- enterCS() primitive
    - Critical section 진입 전 검사
    - 다른 프로세스가 critical section 안에 있는지 검사

- exitCS() primitive
    - Critical section을 벗어날 때의 후처리 과정
    - Critical section을 벗어남을 시스템이 알림

### 상호배제를 만들기 위해 만족해야 하는 조건

- Mutual exclusion (상호배제)
    - Critical section (CS)에 프로세스가 있으면, 다른 프로세스의 진입을 금지

- Progress (진행)
    - CS 안에 있는 프로세스 외에는, 다른 프로세스가 CS에 진입하는 것을 방해하면 안됨

- Bounded waiting (한정대기)
    - 프로세스의 CS 진입은 유한시간 내에 허용되어야 T한다.
    

## Dekker’s Algorithm

- Two process Me을 보장하는 최초의 알고리즘
- 두 프로세스가 동시에 임계 영역에 들어가지 않고 하나만 들어가도록 한다.
- 각 플래그를 이용해 임계 영역을 들어갈 의사나 우선순위를 나타낸다.

## Peterson’s Algorithm

- Dekker’s algorithm 보다 간단하게 구현
- 순서를 지정해서 들어갈 순서를 정한다.
- 순서를 양보를 하고 양보를 늦게하는 것이 먼저 들어간다.

## 다익스트라 Dijkstra

- 최초로 프로세스 n개의 상호배제 문제를 소프트웨어적으로 해결
- 실행 시간이 가장 짧은 프로세스에 프로세서 할당하는 세마포 방법, 가장 짧은 평균 대기시간 제공

### 다익스트라 알고리즘의 flag[] 변수

- idle : 프로세스가 임계 지역 진입을 시도하고 있지 않을 때
- want-in : 프로세스의 임계 지역 진입 시도 1단계일 때
- in-CS : 프로세스의 임계 지역 진입 시도 2단계 및 임계 지역 내에SW을 때

## SW solutioin들의 문제점

- 속도가 느림
- 구현이 복잡한
- ME primtive 실행 중 preemption 될 수 있음
    - 공유 데이터 수정 중은 interrupt를 억제 함으로서 해결 가능
        - Overhead 발생
- Busy waiting
    - Inefficient
