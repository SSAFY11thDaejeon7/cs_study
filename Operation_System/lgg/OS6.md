# 6강

# Process Synchronization (동기화)

**배경**

다중 프로그래밍 시스템에서 프로세스들끼리 공유하는 자원이 있을 때 문제가 발생.

**용어**

동기화 : 이러한 병행수행중인 비동기적 프로세스들 간에 정보를 공유하고, 서로 동작을 맞추는 작업

Shared data 공유 데이터 : 여러 프로세스들이 공유하는 데이터

Critical Section 임계 영역 : 프로세스간 공유 데이터에 접근하는 **코드** 영역

Mutual Exclusion(mutex) 상호 배제 : 둘 이상의 프로세스가 동시에 critical section에 진입하는 것을 막는 것

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/14b1c30e-5d3b-48f6-8b6e-6fee7cccb9d0/b56f2bce-b3fd-4c11-9020-a5b73ce57a56/Untitled.png)

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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/14b1c30e-5d3b-48f6-8b6e-6fee7cccb9d0/557c36d3-cf6f-4bf2-ae79-991b77d3f657/Untitled.png)

**방법**

turn이라는 변수를 둔다. turn이 내 turn일 때만 cs영역에 진입. cs영역을 벗어나게 되면 다른 프로세스로 turn을 넘김

**mutual exclusion 위배**

p0이 cs 진입하지 않으면 p1은 무한대기에 빠지게 됨.

하나의 프로세스가 두 번 연속 cs영역 진입 불가.

- Mutual Exclusion version 2

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/14b1c30e-5d3b-48f6-8b6e-6fee7cccb9d0/436dba0b-5bc8-48d9-9666-0c8998bee497/Untitled.png)

**방법**

flag 변수 사용. 상대 flag가 안 세워져있으면, 내 flag를 세우고 cs영역에 진입하자.

**mutual exclusion 위배**

p0의 endwhile;에서 context switching이 일어나는 경우.

p1 flag가 안 세워진 것을 확인하고, cs영역에 진입할려했으나 내 flag를 못 세우고 context switching 일어남.

p1이 cs영역 진입. 아직 p0의 flag가 세워져있지 않으므로 cs영역으로 진입.

그러다가 p0 context switching 종료되고 다시 수행. flag[0] = true로 p0 깃발 세우고 cs영역 진입 → cs영역에 p0,p1 모두 들어가게 되는 상황 발생.

- Mutual Exclusion version 3

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/14b1c30e-5d3b-48f6-8b6e-6fee7cccb9d0/32a97691-d95a-4125-85ce-b949451e65d7/Untitled.png)

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

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/14b1c30e-5d3b-48f6-8b6e-6fee7cccb9d0/bf1fc96d-1963-43a1-b7ac-7579f2a16a8d/Untitled.png)

**개념**

turn 변수와 flag 변수를 함께 사용하자. 상대 프로세스 flag가 세워져있어도 turn을 한 번 더 확인하는 작업으로 mutual exclusion 보장.

### Dijkstra’s algorithm

flag 변수 3개 사용

1. idle : 프로세스가 cs영역 진입 시도하지 않고 있을 때
2. want-in : 프로세스가 cs영역 진입 시도 1단계일때
3. in-CS : 프로세스가 cs영역 진입 시도 2단계일때 + cs영역 내에 있을 때

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/14b1c30e-5d3b-48f6-8b6e-6fee7cccb9d0/42b1cd93-0f06-4a58-a53c-c954986cf12b/Untitled.png)

**1단계**

want-in flag세우고, 현재 turn인 프로세스가 idle 선언하면 turn을 내껄로 뺏어온다.

**2단계**

우선 내 in-cs flag를 세우고,

모든 프로세스를 확인하며 현재 in-cs인 프로세스가 없다면 내가 cs영역으로 진입한다. (cs영역에 나 외에 아무도 없음을 보장)

이러한 SW solution들의 문제점

속도 느림. 구현 복잡. ME primitive 중에도 preemption될 수 있다. 즉, 하나의 sw instruction도 여러 hw 기계어 명령어들로 이루어지므로 언제든지 중간에 context switching 일어날 수 있다.

busy waiting : 계속 쉬지않고 프로세스들의 flag를 확인해야하므로 → overhead

그래서 HW solution 탄생
