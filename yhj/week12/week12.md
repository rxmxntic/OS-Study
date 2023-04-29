# 페이징 기법
페이징 기법은 아래 사진으로 깔끔하게 설명이 가능하다.
![](https://velog.velcdn.com/images/ofohj/post/bf9f803c-fa37-43bd-94eb-de56fd4dbcd0/image.png)

- 페이징 기법: logical memory부분처럼 동일한 크기로 나뉜 메모리에 물리적 주소 삽입
- 페이징 도구: page table(주소 변환시에 사용됨, 논리적 메모리 개수만큼 존재)
- 페이지 프레임: 물리적 주소가 들어갈 수 있는 공간(사진 상으로 0~7번 메모리에 해당)

[사진 해석]
📍 0번 페이지는 page table에 의해 1번 페이지 프레임에 할당된다.
📍 1번 페이지는 page table에 의해 3번 페이지 프레임에 할당된다.

나머지 두 페이지도 같은 방식으로 이루어진다.

---
page table에 대해 더 자세히 알아보자!

---

# page table
## 1. 개념
논리적 주소를 물리적 주소로 변환하기 위한 매핑 정보 저장

## 2. 위치

⚡ Page Table은 main memory에 상주한다.

🔻 cf.
- main memory: 프로그램이 실행되는 메모리
- logical memory: 가상 메모리
- physical memory: 실제로 존재하는 하드웨어 메모리

## 3. 구성 요소
*⭐= 수업시간에 언급

1) Page Table Entry (PTE): 페이지의 정보를 담고 있는 항목

2) Valid/Invalid Bit: PTE가 유효한지 여부를 나타내는 비트

3) Protection/Access Bits: 페이지의 보호 및 접근 권한에 대한 정보 포함 👉 해당 페이지에 대한 사용 권한 제어

4) ⭐Page Table Base Regis(PTBR): 페이지 테이블의 **시작 주소** 저장(페이지 테이블을 가리킴)

5) ⭐Page Table Length Register(PTLR): 페이지 테이블의 **크기** 저장

6) ⭐Translation Lookaside Buffer(TLB): 페이지 테이블을 빠르게 조회하기 위한 캐시

---
# 2단계 page table
![](https://velog.velcdn.com/images/ofohj/post/46067dd6-ecc2-4d29-b4ae-44e6312b3903/image.png)

- 설명: page table이 두 개인 페이지 테이블
- 필요성: 페이지 테이블의 크기를 줄이고 메모리 사용량 최적화
- 종류:
	- 첫 번째(바깥쪽) page table: 프로세스의 가상 주소공간 나누기
	- 두 번째(안쪽) page tabe:첫 번째 테이블에 대한 프레임 링크 제출
    
>💡 page table 1개 크기 = 그 안에 있는 총 엘리먼트 크기
if page table = 4KB, 각 엘리먼트 크기: 4B 👉 d엘리먼트는 총 1000개

---
# 다단계 page table
## Multilevel Page Table
- 대규모 가상 주소 공간을 관리하기 위한 페이지 테이블 구조
- 여러 수준의 페이지 테이블을 사용하여 가상 주소와 물리적 메모리 주소 간의 매핑 정보 저장

## Multilevel paging
- 주소 공간이 더 커지면 다단계 페이지 테이블이 필요
- 각 페이지 테이블 엔트리의 크기가 고정X 👉 물리 메모리의 낭비 최소화

---
# Memory Protection
메모리 보호를 위해 page table의 각 entry마다 아래의 bit을 둔다.

## 1. Protection bit
- page에 대한 접근 권한(read/write/read-only)

## 2. valid-invalid bit
### 1) valid bit
- 해당 주소의 프레임에 그 프로세스를 구성하는 유효한 내용이 **있**음(접근 허용)

### 2) invalid bit
- 해당 주소의 프레임에 그 프로세스를 구성하는 유효한 내용이 **없**음(접근 불허)

12:40

---

출처: http://kocw.net/home/m/search/kemView.do?kemId=1046323