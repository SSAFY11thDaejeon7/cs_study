## Working Set algorithm
- 1968년 Denning이 제안
- Working set
  - Process가 특정 시점에 자주 참조하는 page들의 집합
  - 최근 일정시간 동안 참조된 page들의 집합
  - 시간에 따라 변함
- Working Set memory management
  - Locality에 기반을 둠
  - Working set을 메모리에 항상 유지
    - Page fault rate 감소
    - 시스템 성능 향상
  - Window size는 고정
    - Memory allocation은 가변
    - Window size 값이 성능을 결정 짓는 중요한 요소
<img width="576" alt="스크린샷 2024-02-21 오후 9 20 29" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/a3c85a86-ed65-4a0d-8e10-2b49f5ebe742">

- 성능 평가
  - Page fault 수 외 다른 지표도 함께 봐야 함
- 특성
  - 적재 되는 page가 없더라도, 메모리를 반납하는 page가 있을 수 있음
  - 새로 적재되는 page가 있더라도, 교체되는 page가 없을 수 있음
- 단점
  - Working set management overhead
  - Residence set을 Page fault가 없더라도, 지속적으로 관리해야 함
<img width="683" alt="스크린샷 2024-02-21 오후 9 23 07" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/d48caa46-7f1a-4e6b-92fa-690861cb8658">

## Page Fault Frequency algorithm
- Residence set size를 page fault rate에 따라 결정
  - Low page fault rate (long inter-fault time)
    - Process에게 할당된 PF 수를 감소
  - High page fault rate (short inter-fault time)
    - Process에게 할당된 PF 수를 증가
- Resident set 갱신 및 메모리 할당
  - Page fault가 발생시에만 수행
  - Low overhead
- 성능 평가
  - Page fault 수 외 다른 지표도 함께 봐야 함
  - Example
    - Time interval [1,10]
      - of page fault = 5
      - 평균 할당 page frame 수 = 3.7
- 특징
  - 메모리 상태 변화가 page fault 발생 시에만 변함
    - Low overhead
## Variable MIN algorithm
- Variable allocation 기반 교체 기법 중 optimal algorithm
  - 평균 메모리 할당량과 page fault 발생 횟수 모두 고려 했을 때의 Optimal
- 실현 불가능한 기법
  - Page reference string을 미리 알고 있어야 함
  <img width="657" alt="스크린샷 2024-02-21 오후 9 28 24" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/73da7e57-7f9c-4ef7-ba11-177f8af7c47b">

## Page Size
- 시스템 특성에 따라 다름
  - No best answer
  - 점점 커지는 경향
- 일반적인 page size
  - 2^7(128)bytes ~ 2^22(4M)bytes
- Small page size
  - 내부 단편화 감소
  - I/O 시간 증가
  - Locality 향상
  - Page fault 증가
- Large page size
  - 내부 단편화 증가
  - I/O 시간 감소
  - Locality 저하
  - Page fault 감소
## Program Restructuring
- 가상 메모리 시스템의 특성에 맞도록 프로그램을 재구성
- 사용자가 가상 메모리 관리 기법(예, paging system)에 대해 이해하고 있다면, 프로그램의 구조를 변경하여 성능을 높일 수 있음
## TLB Reach
- TLB를 통해 접근 할 수 있는 메모리의 양
  - The number of entries * the page size
- TLB의 hit ratio를 높이려면
  - TLB의 크기 증가
    - Expensive
  - Page 크기 증가 or 다양한 page size 지원
    - OS의 지원이 필요
      - 최근 OS의 발전 경향
  
