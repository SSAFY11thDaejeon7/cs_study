# Segmentation System
- 프로그램을 논리적 block으로 분할(segment)
  - block의 크기가 서로 다를 수 있음
  - 예)stack, heap, main procedure, shared lib, Etc.
- 특징
  - 메모리를 미리 분할 하지 않음
    - VPM과 유사
  - Segment sharing/protection이 용이함
  - Address mapping 및 메모리 관리의 overhead가 큼
  - No internal fragmentation
    - External fragmentation 발생 가능
<img width="598" alt="스크린샷 2024-02-14 오후 11 21 03" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/3ac6e381-7966-4487-9fda-813efd954131">

## Address mapping
1. 프로세스의 SMT가 저장되어 있는 주소 b에 접근
2. SMT에서 segment s의 entry 찾음
3. 찾아진 Entry에 대해 다음 단계들을 순차적으로 실행
  1. 존재 비트가 0인 경우
      - swap deive로 부터 해당 segment를 메모리로 적재, SMT를 갱신
  2. 변위(d)가 segment 길이보다 큰 경우
      - segment overflow exception 처리 모듈을 호출
  3. 허가되지 않은 연산일 경우
      - segment protection exception 처리 모듈을 호출
4. 실제 주소 r계산 (r = a + d)
5. r로 메모리에 접근
<img width="687" alt="스크린샷 2024-02-14 오후 11 28 30" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/46fe9507-97af-4323-a207-f16c29119b69">

### Segment sharing/protection
- 논리적으로 분할되어 있어, 공유 및 보호가 용이함
<img width="568" alt="스크린샷 2024-02-14 오후 11 30 11" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/2fce6471-9ca7-4379-811d-f9f5fcbd6676">

## Segmentation System - Summary
- 프로그램을 논리 단위로 분할(segment) / 메모리를 동적으로 분할
  - 내부 단편화 문제 없음
  - Segment sharing/protection이 용이함
  - Paging system 대비 과리 overhead가 큼
- 필요한 segment만 메모리에 적재하여 사용용
  - 메모리의 효율적 활용
- Segment mapping overhead
  - 메모리 공간 및 추가적인 메모리 접근이 필요
  - 전용 HW 활용으로 해결 가능
 

# Hybrid Paging/Segmentation
- Paging과 Segmentation의 장점 결합
- 프로그램 분할
  - 논리 단위의 segment로 분할
  - 각 segment를 고정된 크기의 page들로 분할
- Page 단위로 메모리에 적재
<img width="505" alt="스크린샷 2024-02-14 오후 11 36 30" src="https://github.com/SSAFY11thDaejeon7/cs_study/assets/129651243/8ba2f5c5-c950-42ac-a39a-7aad67e9c2f3">

- Summary
  - 논리적 분할(segment)와 고정 크기 분할(page)을 결합
    - Page sharing/protection이 쉬움
    - 메모리 할당/관리 overhead가 적음
    - No external fragmentation
  - 전체 테이블의 수 증가
    - 메모리 소모가 큼
    - Address mapping 과정이 복잡
  - Direct mapping의 경우, 메모리 접근이 3배
    - 성능이 저하될 수 있음


