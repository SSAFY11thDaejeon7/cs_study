# 스레드(Thread)

프로세스가 자원을 제어하는 과정

- Light Weight Process(LWP)
- 프로세서 활용의 기본 단위
- 하나의 프로세스 안에 여러 쓰레드 존재

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/6cfc7b28-b11f-4542-8d0d-cdd2fa23b2ff)


- 구성 요소
    - Thread ID
    - Register Set
    - Stack
- 제어 요소 외 코드, 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유
- 전통적 프로세스 = 단일 스레드 프로세스

## 장점

- 사용자 응답성
    - 일부 스레드의 처리가 지연되어도, 다른 스레드는 작업을 계속 처리 가능
- 자원 공유
    - 자원을 공유해서 효율성 증가
- 경제성
    - 프로세스의 생성, Context switch에 비해 효율적
- 멀티 프로세서 활용
    - 병렬 처리를 통한 성능 향상

## 스레드 사용의 예

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/fdc2861d-e561-4973-96a4-ac37157a7ab9)


실시간 게임을 하는 위 상황에서
싱글 스레드를 사용하는 경우 각 작업을 하는 동안 다른 작업이 잠시 중단됨

→ 멀티 스레드를 사용하여 3가지 작업을 동시에 처리할 수 있어야 함.

## 사용자 수준 스레드 (n : 1 model)

- 사용자 영역의 **스레드 라이브러리**로 구현 됨
    - 스레드 생성, 스케줄링 등

- 커널은 스레드의 존재를 모름
    - 장점
        - 커널의 관리(개입)을 받지 않음
            - 생성 및 관리의 부하가 적음 → 유연한 관리 가능
            - 이식성이 높음
            
    - 단점
        - 커널은 프로세스 단위로 자원 할당
            - 하나의 스레드가 block 상태가 되면, 모든 스레드가 대기

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/15d8eb63-ed86-4cc2-a4a6-7def04b1d525)


## 커널 수준 스레드 (1 : 1 model)

- OS가 직접 관리
- 장점
    - 커널이 각 스레드를 개별적으로 관리
        - 프로세스 내 스레드들이 병행 수행 가능
        
- 단점
    - 커널 영역에서 스레드의 생성, 관리 수행
        - Context switching 등 부하(Overhead)가 큼

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/04c3b81c-ad51-48da-9400-ebc1bf6baf56)


## 혼합형 스레드(n : m model)

- 사용자 수준 스레드와, 커널 수준 스레드를 여러 개 혼합하여 사용

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/6849efa7-0c04-4471-97eb-08cc297ed1af)


- n개의 사용자 수준 스레드 - m개의 커널 스레드(n ≥ m)
    - 사용자는 원하는 수 만큼 스레드 사용
    - 커널 스레드는 자신에게 할당된 하나의 사용자 스레드가 block 상태가 되어도, 다른 스레드 수행 가능
        - 병행 처리 가능
- 효율적이며 유연

![image](https://github.com/SSAFY11thDaejeon7/cs_study/assets/90568693/82a423b5-fc36-4dc7-bcc8-231c55692b8c)
