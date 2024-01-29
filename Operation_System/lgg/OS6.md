# 6강

# Process Synchronization (동기화)

**배경**

다중 프로그래밍 시스템에서 프로세스들끼리 공유하는 자원이 있을 때 문제가 발생.

**용어**

동기화 : 이러한 병행수행중인 비동기적 프로세스들 간에 정보를 공유하고, 서로 동작을 맞추는 작업

Shared data 공유 데이터 : 여러 프로세스들이 공유하는 데이터

Critical Section 임계 영역 : 프로세스간 공유 데이터에 접근하는 **코드** 영역

Mutual Exclusion(mutex) 상호 배제 : 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/60e00397-73e7-4af2-a1f5-c3147ea36089)

**원자성**

하나의 machine instruction(기계어 연산)은 원자성을 가진다. 원자성이란 indivisible, 즉 여러 명령으로 쪼개질 수 없음을 의미하고 하나의 instruction 도중에 다른 프로세스의 interrupt를 받지 않는 것을 보장한다. 

그러나 기계어 연산들 사이에서는 언제든지 context switching이 일어날 수 있다. 위 그림에서 i 프로세스 수행완료 후 j 프로세스 수행을 원했다. sdata += 2를 원했다. 

하지만 프로세스 i가 값을 쓰기전에 context switching이 일어나면서 sdata에는 1이 쓰이게 된다.

**Race Condition**

이렇게 프로세스 명령어 수행 순서에 따라서 결과가 달라지는 문제점을 race condition이라고 한다.

**Critical Section**

따라서 3개의 연산을 하나의 원자성을 보장해주기 위해 critical section으로 보호해야 한다. critical section이란 프로세스간 공유 데이터에 접근하는 **코드** 영역을 의미함. 즉 프로세스간 공유 데이터에 접근하는 코드 영역.

## **Mutual Exclusion**

race condition을 방지하기 위해, 하나의 프로세스 코드가 critical section에 진입했을 때 다른 프로세스의 해당 데이터에 접근하지 못하도록 막는 것

Mutual Exclusion의 3가지 Requirement

1. Mutual Exclusion : 상호배제. critical section에 프로세스가 있으면, 다른 프로세스의 진입을 금지함.
2. Progress : 진행. cs안에 있는 프로세스 외에는, 다른 프로세스들의 cs 영역 접근을 막으면 안됨.
3. Bounded waiting : 한정대기. 프로세스의 cs 진입은 유한시간내에 허용되어야 한다. 무한대기에 빠지면 안됨.

- Mutual Exclusion version 1

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/01e2ad17-d858-4884-aebb-b202ce40d20f)

**방법**

turn이라는 변수를 둔다. turn이 내 turn일 때만 cs영역에 진입. cs영역을 벗어나게 되면 다른 프로세스로 turn을 넘김

**mutual exclusion 위배**

p0이 cs 진입하지 않으면 p1은 무한대기에 빠지게 됨.

하나의 프로세스가 두 번 연속 cs영역 진입 불가.

- Mutual Exclusion version 2

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/9ccc4a87-e693-4413-9a70-102f9f088799)

**방법**

flag 변수 사용. 상대 flag가 안 세워져있으면, 내 flag를 세우고 cs영역에 진입하자.

**mutual exclusion 위배**

p0의 endwhile;에서 context switching이 일어나는 경우.

p1 flag가 안 세워진 것을 확인하고, cs영역에 진입할려했으나 내 flag를 못 세우고 context switching 일어남.

p1이 cs영역 진입. 아직 p0의 flag가 세워져있지 않으므로 cs영역으로 진입.

그러다가 p0 context switching 종료되고 다시 수행. flag[0] = true로 p0 깃발 세우고 cs영역 진입 → cs영역에 p0,p1 모두 들어가게 되는 상황 발생.

- Mutual Exclusion version 3

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/81b23a2f-edbf-44b2-ac62-a2a978711d31)

**방법**

상대 flag 확인하기 전에, 내 flag부터 세우자.

**mutual exclusion 위배**

p0이 flag세우고 context switching 일어나는 경우에, p1이 cs영역 진입

p1도 flag세우고 진입하려 하는데, p0의 flag가 세워져있으므로 cs영역 진입 불가.

version1,2,3 모두 mutual exclusion requirements를 만족시키지 못함.

그래서 SW solution + HW solution 등장

## <ME SW solution>

### Dekker’s algorithm

ME를 보장하는 최초의 SW 알고리즘

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/c54388b2-a901-4ca3-9e2a-fa46a61b088c)

**개념**

turn 변수와 flag 변수를 함께 사용하자. 상대 프로세스 flag가 세워져있어도 turn을 한 번 더 확인하는 작업으로 mutual exclusion 보장.

### Dijkstra’s algorithm

flag 변수 3개 사용

1. idle : 프로세스가 cs영역 진입 시도하지 않고 있을 때
2. want-in : 프로세스가 cs영역 진입 시도 1단계일때
3. in-CS : 프로세스가 cs영역 진입 시도 2단계일때 + cs영역 내에 있을 때

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/3a90e87e-1177-4c21-8c96-4ce5ba1b7d43)

**1단계**

want-in flag세우고, 현재 turn인 프로세스가 idle 선언하면 turn을 내껄로 뺏어온다.

**2단계**

우선 내 in-cs flag를 세우고,

모든 프로세스를 확인하며 현재 in-cs인 프로세스가 없다면 내가 cs영역으로 진입한다. (cs영역에 나 외에 아무도 없음을 보장)

이러한 SW solution들의 문제점

속도 느림. 구현 복잡. ME primitive 중에도 preemption될 수 있다. 즉, 하나의 sw instruction도 여러 hw 기계어 명령어들로 이루어지므로 언제든지 중간에 context switching 일어날 수 있다.

busy waiting : 계속 쉬지않고 프로세스들의 flag를 확인해야하므로 → overhead

그래서 HW solution 탄생




## <ME HW solution>

### TAS instruction

**개념**

Test와 Set을 한 번에 수행하는 기계어(Machine instruction)

한번에 수행한다 : Atomicity(원자성)를 가진다. 실행 도중에 interrupt를 받아서 preemption을 당하지 않는다. 도중에 context switching이 일어나지 않음.

이전 race conditon이 발생했던 1. 값을 읽어오고 2. 값을 변경하고 3. 값을 쓰는 3개의 instruction을 TAS instruction 으로 기계어 한 번에 처리한다. ⇒ HW solution

하지만 기존처럼 TAS 명령어와 lock만으로 구현하면, 여전히 lock이 풀렸을 때 어떤 프로세스가 우선순위를 가지고 CS영역에 진입할지? 에 대한 문제점이 생긴다. 그래서 프로세스마다의 waiting[] 배열도 함께 구현

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/40e7f880-f6b2-485d-a1b7-3e95e3aeebb3)

내 프로세스의 작업이 종료되면, j= (j+1)%n 즉 오른쪽으로 쭉 살피면서 waiting 중인 프로세스가 있다면 걔의 waiting을 풀어줌 (걔가 우선권을 가지고 cs영역 들어올 수 있게) 그리고 나서 내 lock을 풀면 우선순위 문제점을 일정한 방식으로 해결할 수 있다.

TAS 명령어

**장점**

구현이 쉽다

**단점**

여전히 busy waiting이 존재한다. 임계영역 진입하기 전에 계속해서 while문으로 lock이 풀렸는지, waiting상태가 어떤지 파악하고 있어야한다. 그래서 OS가 지원하는 SW solution을 함께 사용하게 됨. (Spinlock,Semaphore,Eventcount/sequencer

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/935c2778-2def-4d6e-98a4-1438eb6c57ca)

### Spinlock

**개념**

특별한 정수형 변수. 직접 값을 쓰지 못하고 P(), V() 연산으로만 spinlock 변수에 접근이 가능함.

P(), V() 연산은 atomic 연산임을 OS가 보장해준다. 실행도중에 preemption이 일어나지 않는다.

P() 연산 : 물건(spinlock 변수)을 사용한다. 물건이 생길때까지 while문 반복 (busy waiting)

V() 연산 : 물건을 반납한다. 사용하고 spinlock(active) 변수 ++;

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/b1932617-ffec-4ab1-82de-7ab010293f97)

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/32d3d8d5-740b-42e0-9e18-411ecccb4086)

**장점**

CS영역 진입할 때, 나올 때 연산을 OS가 spinlock 변수로 하나의 연산으로 보장해줌으로써 mutual exclusion 문제를 해결했음.

**단점**

CPU가 여러 개인 상황에서만 spinlock 변수를 사용가능하다.단일 CPU 상황에서 CS영역 도중에 context switching 일어나면, active spinlock 변수가 0인 상태로 Pi,Pj 둘다 일을 못하게 됨.

Busy waiting 여전히 미해결 .P() 연산 while문

### Semaphore

**개념**

음이 아닌 정수. spinlock 변수와 유사함. P(),V()연산 둘 다 가지고 있음.

** spinlock 과의 차이점 : semaphore 변수 1개당 **ready queue**가 1개 할당된다.

**종류**

- binary semaphore : 0,1값만 가진다.
- counting semaphore : 0 이상의 모든 정수를 가질 수 있다.

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/fadfff02-c78d-4dcb-9b21-6c8fe70d38d2)

**spinlock 변수와의 차이점**

spinlock은 busy waiting하면서 spinlock변수가 생겼을 때 P()연산을 진행했지만,

semaphore는 프로세스가 cs영역 들어갈 수 없는 상황이라면 **ready queue**에 가서 그냥 대기하면 된다. → busy waiting 해결

v() 연산은 ready queue에 프로세스가 존재하는지 확인하고 깨워주는 방식

**semaphore로 해결 가능한 동기화 문제들**

- mutual exclusion 상호 배제
- process synchronization 프로세스 동기화
- producer-consumer 생산자-소비자
- reader-writer
- Dining Philosophers

1. mutual exclusion
    
    spinlock과 같은 방법으로 mutual exclusion 해결
    
2. process synchronization
    
    병렬, 비동기적으로 수행되는 프로세스들의 실행 순서를 해결할 수 있다.
    
3. producer-consumer
    
    생산자가 생산하는 도중에 소비자가 가져가면 안됨.
    
    생산자가 생산하는 도중에 다른 생산자가 생산하면 안됨.
    
    **single buffer producer-consumer 문제**

     ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/193a0199-d2b9-4288-9a68-d3ee3a8adad1)
    

- 생산자 : P(consumed)로 소비자에 의해 소비되었는지 확인. buffer에 생산물 입력. V(produced)로 생산됨을 알려줌
- 소비자 : P(produced)로 생성되었는지 확인. buffer값 사용. V(consumed)로 소비되었음을 알려줌

N-**buffers producer-consumer 문제**

원형 buffer와 pointer로 구현 → 몇 개 생성되고, 몇 개 소비되었는지 tracking

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/217aae1f-f154-4855-a0c8-bb954a5ac8fd)

- 생산자 : P(nrempty)로 buffer 비워진거 있니? 확인하고 생산완료 후에 V(nrfull) nrfull 변수 1증가.
- 소비자 : P(nrfull)로 생산된거 있니? 확인하고 소비완료후에 V(nrempty) empty 변수 1 증가.

**Reader-Writer 문제**

reader들은 동시에 접근이 가능. writer는 1개씩만 접근가능(mutual exclusion 필요)

reader / writer 에 대한 우선권을 부여

- reader preference problem : 여러 reader와 writer가 있을 때 reader들이 먼저 읽게 해주는것
    
    ![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/138864974/b6c7bf1a-d53e-4920-a8b4-1b761726309a)
    

1단계 (읽기 전 작업)

if (nreaders = 0) P(wmutex) : 나 읽을거야 writer 너 그만 써.

else nreaders += 1 : 이미 reader가 우선권 가지고 읽고 있네? 나도 낄게

2단계 (읽고나서 작업)

nreaders -= 1;

if (nreaders = 0) : V(wmutex) : 내가 마지막 reader네? writer야 이제 너 써.

else : 아직 읽고 있는 reader님이 있네. writer 풀어주면 안되겠다

- writer preference problem : 여러 reader와 writer가 있을 때 writer들이 먼저 값을 쓰게 해주는것
    
    reader preference problem 반대로 구현
    

**Semaphore 장점 결론**

busy waiting을 없앴다! CS영역 못 들어가는 프로세스들은 ready queue에서 대기

**하지만** **단점**

wake up 순서를 어떻게 해야할지? starvation 현상 여전히 발생할 수 있다.
