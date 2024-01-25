# 5강

https://rebro.kr/175

**배경**

여러 프로세스가 시스템 내에 존재하게 되었다.

자원을 할당할 프로세스들을 선택해줘야 한다. 어떤 프로세스 우선순위?

**자원 관리 2가지 측면**

1. 시간 분할 관리(CPU)
2. 공간 분할 관리(메모리)

이 중, 시간 분할 관리 측면을 CPU 스케줄링이라고 하고, 보통 넓은 의미로 **프로세스 스케줄링**이라고 한다. 공간 분할 관리 메모리적인 측면은 향후에 다룰 것.(가상 메모리)

# 프로세스(CPU) 스케줄링

---

## **스케줄링 왜함? 스케줄링의 목적**

시스템의 성능 향상을 목적에 둠.

**시스템 성능?**

성능이라는 표현이 굉장히 애매할 수 있다. 그래서 여러가지 척도로 성능을 측정

1. **시스템 입장**에서의 성능 척도

CPU 이용률 : CPU가 쉬지 않고 일한 시간 / 전체 시간

처리량 Throughput : 단위 시간당 수행 완료한 프로세스 수

자원 활용도 : 단위 시간당 자원 활용률

1. **프로그램 입장**에서의 성능 척도

소요시간 Turnaround time : 프로세스가 ready 상태에 대기한 시간부터 작업을 완료하는데 걸린 시간

대기시간 Waiting time : 프로세스가 ready 상태에서 대기한 시간

응답시간 Response time : 프로세스가 처음으로 출력(응답)을 받기까지 걸린 시간

실행시간 Burst time : 실행 시작 → 실행 끝

## **스케줄링 기준**

1. 프로세스 특성
    
    기본적으로 프로세스는 CPU만 사용하는 단계(CPU burst)와 I/O 작업만 하는 단계(I/O burst)의 반복으로 구성된 사이클 형태이다. 따라서 이 각 주기들을 잘 파악하여 CPU를 효과적으로 할당해주는 것이 중요
    
2. 시스템 특성 (한번에 다 처리하는 Batch vs 사용자와 Interactive)
3. 프로세스의 긴급성
4. 프로세스의 우선순위
5. 프로세스의 총 실행 시간

## 스케줄링 단계(process state diagram과 연관 지어서 생각)

**발생하는 빈도** 및 **할당 자원**에 따른 구분

- Long-term scheduling (Job scheduling)
- Mid-term scheduling (memory allocation)
- Short-term scheduling (process schdeuling)

### Long-term Scheduling

**개념**

Kernel에 등록할 작업(프로세스의 정의임) 결정 ⇒ 시스템 내에 등록할 프로세스의 수를 조절

프로세스의 단위가 크므로 이런 long-term scheduling은 빈번하게 일어나지 않는다.

I/O bounded 프로세스와 compute-bounded 프로세스들을 잘 섞어서 분배해서 선택해줘야한다.

cf. 시분할 시스템에선 모든 프로세스들을 시스템에 등록하기 때문에 long-term scheduling 상대적으로 덜 중요

### Mid-term scheduling

**개념**

메모리를 할당해줄지 말지 결정 (memory allocation)

프로세스 state diagram에서 Swapping을 나타냄.

swap-in : suspended ready → ready

swap-out : ready → suspended ready

### Short-term scheduling

**개념**

프로세서를 할당할(dispatch할) 프로세스를 결정

즉, diagram 에서 ready → running 이거를 결정

Interrupt, I/O block, time-out 등 아주 다양한 이유들이 존재하고 가장 빈번하게 발생함.

## 스케줄링 정책(Policy)

1. 선점 preemptive vs 비선점 Non-preemptive
2. 우선순위

**선점 preemptive**

할당 받은 자원을 누군가가 빼앗을 수 있다.

장점 : time sharing, real time system에 적합한 스케줄링

단점 : context switch가 자주 일어나므로 overhead 큼.

**비선점 Non preemptive**

할당 받은 자원을 다른 이가 빼앗을 수 없다. 즉, 할당 받은 자원을 스스로 반납할 때까지 사용함.

장점 : context switch 자주 발생 x

단점 : 우선순위 모호, 평균 응답시간 증가

**우선순위**

프로세스 우선순위 1. 정적 2. 동적

1. 정적 : 프로세스 생성시 결정된 우선순위가 쭉 유지
2. 동적 : 프로세스 상태 변화에 따라 우선순위 변경



## 스케줄링 방식

**FCFS(First Come First Service) 방식**

도착 시간(프로세스가 ready queue에 도착하는 순서) 기준. Non-preemptive(비선점)

선착순으로 프로세서 CPU를 할당 받는다.

- 장점

자원을 효율적으로 사용 가능. context switching 적다.

일괄적으로 처리하는 Batch System에 적합. 사용자 interactive system에 부적합.

- 단점

Convoy effect : 수행시간이 긴 하나의 프로세스 때문에 다른 프로세스들의 대기시간 증가.

긴 평균 응답시간(response time)

**RR(Round-Robin) 방식**

FCFS처럼 도착시간 기준이지만, 프로세스마다 자원 사용에 **제한시간**이 있다.

- 장점

특정 프로세스의 자원 독점을 방지할 수 있다. 사용자 대화형, 시분할 시스템에 적합함.

- 단점

context switch overhead가 크다.

Time quantum (제한시간)이 System 성능을 결정 짓는 중요한 요소임.

Time quantum 무한대 : FCFS   Time quantum 0에 수렴 ⇒ 사용자는 모든 프로세스가 각각의 프로세서에서 실행되는 것처럼 느낌 (하지만 overhead 증가)

**SPN(Shortest Process Next)**

실행시간 기준 : shortest job first. 실행시간이 작은 프로세스 먼저 처리한다.

- 장점

평균 대기시간(WT) 최소화

시스템 내 프로세스 수 최소화 → 시스템 입장에서는 부하가 감소, 메모리 절약함으로써 시스템 성능 향상

대다수 프로세스들에게 빠른 응답시간을 제공함.

- 단점

Starvation 현상 발생 : 무한대기에 빠지는 프로세스가 생길 수 있다. (실행시간이 긴 프로세스) → HRRN의 aging 기법으로 해결

정확한 실행 시간을 알 수 없다. 측정할 수 있는 척도가 필요함.

**SRTN(Shortest Remaining Time Next)**

SPN의 변형. 잔여 실행시간이 더 적은 프로세스를 먼저 처리한다.

잔여 실행시간이 적은 프로세스가 등장하면, 선점된다. CPU 자원 빼앗긴다.

- 장점

SPN의 장점 최대화 : 얼마 남지 않은 프로세스들을 빠르게 처리할 수 있다.

- 단점

프로세스의 잔여 실행시간을 계속 tracking해야한다. → 구현 및 사용이 비현실적

**HRRN(High-Response-Ratio-Next)**

SPN의 변형. (SPN + Aging) + Nonpreemptive 비선점

SPN의 문제 해결 : starvation 현상. 수행시간 긴 프로세스들이 무한대기에 빠지는 상황

Aging 기법 : 단순히 프로세스 실행시간, 잔여시간만을 고려하는 것이 아니라 프로세스의 age나이를 고려하겠다.

프로세스의 대기한 시간을 고려해서 진행.

기준 : Response Ratio → aging 기법 적용 가능.

Response Ratio 응답률 : (WT+BT)/BT     WT가 증가하면 Response Ratio가 증가하므로 SPN의 장점 + Aging 기법 적용 가능하다.

- 단점

SPN 처럼 실행시간을 계속 예측하고 tracking해야한다는 overhead가 발생한다.

**SPN,SRTN,HRRN 의 문제점 : 실행시간 예측 및 tracking이 힘들다.**

1. MLQ(Multi Level Queue)
2. MFQ(Multi Level Feedback Queue)

위의 2가지 기법으로 해결함.

**MLQ (Multi Level Queue)**

작업별(우선순위별) 별도의 프로세스 대기 큐를 여러개를 사용한다. 그래서 우선순위 높은 큐의 작업들을 먼저 수행

최초 배정된 queue를 벗어나지 못함

fixed-priority preemptive scheduling

- 장점

우선순위 큐를 여러개 둠으로써, 프로세스마다 실행시간, 잔여 실행시간 등을 별도로 관리하지 않고 SPN 기법들을 구현 가능하다.

- 단점

여러 개의 queue 관리 등 overhead 발생.

우선순위 낮은 queue는 starvation 발생.

MLQ의 한계점 : 최초 배정된 우선순위 큐를 벗어날 수 없다. 시스템 변화에 프로세스 실행순서가 유동적으로 변화하기 어렵다. 낮은 우선순위 큐에 있는 프로세스들이 중요한 작업을 처리해야할 수도 있음. → MFQ 등장

**MFQ (Multi Level Feedback Queue)**

프로세스의 queue간 이동이 허용된 MLQ

Feedback을 통해 우선 순위 조정 → Dynamic Priority

- 장점
1. MLQ의 장점 : 기존 SPN,SRTN,HRRN 방식의 실행시간(BT) 예측 및 관리의 번거로움 해결
2. Feedback을 통해서 프로세스들이 우선 순위를 유연하게 변동하며 ready queue 이동 ⇒ 시스템 변화에 프로세스들이 동적으로 대응

- 단점

설계,구현이 복잡함. overhead 커짐

starvation 현상 여전히 발생

- 해결방안

각 준비 큐마다 시간 할당량을 다르게 배정 → 우선순위에 따라 time quantum을 차등하게 제공

I/O bounded 프로세스들을 상위 우선순위 큐로 옮김. I/O 관련 프로세스들은 cpu를 잠깐만 쓰고 빠지기 때문에, 이런 설계가 평균 응답시간을 높여줄 수 있다.

에이징Aging 기법 함께 적용 → 대기시간이 일정 시간을 넘어간 프로세스들은 상위 큐로 넘겨줄 수 있게
