## 프로세스와 스레드
  - 프로세스의 자원과 제어 중 자원을 공유하면서 제어만 분리한 부분을 스레드라고 한다.
  - 하나의 프로세스에 스레드는 여러 개 존재할 수 있다.
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/c1521574-64e6-4744-bbc8-3adc2e38c853" width="500px;" alt=""/>
<br><br>

## 스레드
- Light Weight Process : 프로세스에서 제어 부분만을 관여하기 때문
- 프로세서 활용의 기본 단위 : 스레드가 여러 개면 여러 개의 프로세서 사용
- 구성요소
  1) Thread ID
  2) Register set
  3) Stack
- 스레드 마다 제어 요소를 가지며, 코드, 데이터 및 자원들은 프로세스 내 다른 스레드들과 공유한다
- 전통적 프로세스 = 단일 스레드 프로세스

<h3>1. Single-thread Process vs Multi-threads Process</h3>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/45557847-bc55-40c8-929f-1f040056966f" width="400px;" alt=""/>  
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/97e70f4a-cbe0-49fd-bee3-8a21fff7ba16" width="400px;" alt=""/>
<br><br><br>

<h3>2. 스레드의 장점</h3>

   - 사용자 응답성 증가
     - 일부스레드의 처리가 지연되어도, 다른 스레드는 작업을 계속 처리할 수 있다.
   - 자원 공유
      - 자원을 공유해서 사용하기 때문에, context switching이 적으며 커널의 개입을 피할수 있음
   - 경제성
      - 프로세스 생성 및 context switch 감소
   - 멀티 프로세서 활용
      - 병렬처리를 통해 성능 향상
<br><br>

## 스레드의 구현
<h3>1. 사용자 영역의 스레드 ( USer Thread )</h3>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/17334ce3-9249-4f17-889c-16d16c8fcf12" width="600px;" alt=""/>
<br><br>

  - 사용자 영역의 스레드 라이브러리로 구현됨
    - 스레드의 생성, 스케줄링 등
  - 커널은 스레드이 존재를 모름
    - 커널의 개입을 받지않아 생성 및 관리의 부하가 적으며 유연함 관리가 가능
    - 이식성 높음
  - 커널은 프로세스 단위로 자원 할당
    - 하나의 스레드가 block 되면 모든 스레드가 대기
   
<h3>2. 커널 수준 스레드 ( Kernel Threads )</h3>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/aa84c413-d058-4e2e-869d-6f31c20b3a3b" width="600px;" alt=""/>
<br><br>

  - OS Kernel이 스레드를 직접 관리한다.
  - 커널 영역에서 스레드의 생성 및 관리를 수행하기 때문에 Context switching 등의 부하가 크다.
  - 커널이 각 스레드를 개별적으로 관리한다.
    - 프로세스 내 스레드들이 병행 수행이 가능하다
    - 하나의 스레드가 block 되어도, 다른 스레드는 계속 작업 수행이 가능하다
   
<h3>3. 혼합형(n:m) 스레드</h3>
<img src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/91451735/5f2e1ed3-95ac-4eb0-9e77-18c7d32c40b3" width="600px;" alt=""/>
<br><br>

  - n개 사용자 수준 스레드 : m개의 커널 스레드 ( n > m 이어야 한다)
      - 사용자는 원하는 수만큼 스레드 사용
      - 커널 스레드는 자신에게 할당된 하나의 사용자 스레드가 block 되어도 다른 스레드 수행이 가능하다.(병행 처리 가능)
      - 사용자 수준 스레드들이 커널 스레드 각각에 모두 1:1 매핑 되어있기 때문이다.
  - 효율적이면서도 유연하게 동작
