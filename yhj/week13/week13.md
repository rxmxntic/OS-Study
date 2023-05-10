⚡ 이전 내용: 물리적 메모리 할당 기법 中 Noncontiguous allocation
- paging 기법
- segmentation 기법
- paged segmentation 기법

그 중 paging 기법에 대해 배웠고, 오늘은 남은 paging 기법과 다른 두 가지에 대해 배울 예정이다.

---
앞서 배웠던 page table의 문제는 **많은 용량이 필요하다**는 것이었다.

그리고 이를 해결하기 위해 나온 또다른 형태의 page table이 바로 오늘 배울

<span style='color:blue'>Inverted Page Table</span>이다!

---
# Inverted Page Table
## 1. 형태
🔻 기존의 page table을 역발상으로 바꾸어 놓은 형태
![](https://velog.velcdn.com/images/ofohj/post/2c18e939-4231-448d-92f1-d5e5b6920dc0/image.png)

## 2. 비교
- 기존: 논리적 메모리의 프로세스마다 페이지 테이블 존재
- inverted: 페이지 테이블이 딱 한개만 존재하며, 물리적 메모리 프레임 수만큼의 entry가 존재

## 3. 정보
각 entry 안에 저장되는 정보는 다음과 같다.

1. n번째 논리적 페이지 번호 👉 n번째 page frame에 저장
2. process id (어떤 프로세스의 n번째 페이지인지 알기 위함)

## 4. 작동 방법
- 페이지 테이블 entry를 전체 검색하여 주소를 검색한다.
👉 inverted page table은 공간 overhead은 줄일 수 있으나 전체 검색으로 인한 시간적 overhead가 커진다.
👉 따라서, entry를 병렬적으로 검색할 수 있게 도와주는 associative register를 사용한다.

---
# Shared Page
## 1. 개념
- 사용되는 경우: 서로 다른 프로세스에 같은 코드가 사용될 때
- 사용 방법: 같은 코드를 여러번 올리는 것이 아니라 shared code라는 하나의 물리적 메모리에 올림
👉 즉, 같은 프레임으로 mapping해서 메모리에 한 copy만 올림

## 2. 조건
1) Read Only로 작성되어야 함
2) 동일한 logical address에 위치해야 함 👉 컴파일된 코드 안에 있는 logical address를 바꿀 수 없기 때문

---
페이징 기법 끝!

---
# Segmentation 기법
## 1. 비교
- Paging 기법: 주소 공간을 같은 크기의 페이지로 쪼갬
- Segmentation 기법: 의미있는 단위(또는 code, data, stack 단위)로 자름

## 2. 구성 요소
1) logical address
이는 아래 두 가지로 구성
- segment number
- offset: segment table 안에서 얼마나 떨어져 있는지를 나타냄

2) segment table
종류
- base: segment table에서 물리적 주소의 시작 위치를 나타냄
- limit: segment의 길이를 나타냄
📍 limit > offset 을 만족해야함!

3) segment table base register(STBR)
: 물리적 메모리에서 segment table의 위치

4) segment table length register(STLR)
: 프로그램이 사용하는 segment의 수
📍 segment number < STLR 을 만족해야함!

## 3. 장단점
- 장점: 의미 단위의 작업 시(ex. 공유, 보안), 페이징 기법보다 효율적
- 단점: external fragmentation
👉 segment 길이가 동일하지 않아 문제 발생
👉 first fit, best fit으로 해결 가능!

---

# Paged Segmentation
Segmentatin with Paging 이라고도 하며, segmentation과 paging을 합친 기법이다.

## 1. 구성 요소
segmentation 기법과 동일
- segment number
- offset

## 2. 비교
기존 segmentation과는 분할 방식에서 차이가 있다.
- 기존: 의미 단위로 분할
- paged segmentation: 페이지 단위로 분할

## 3. 장점
segment 하나가 여러 개의 **페이지**로 구성
🔻
메모리에 올라갈 때 페이지 단위로 올라감
🔻
hole이 발생하지 않음
🔻
기존 방법의 분할에서 발생하는 external fragmentation 문제 해결 가능

## 4. 동작원리
위 장점에 따라 동작 원리는 아래와 같이 이루어진다.

1. 의미 단위로 나눠야하는 업무의 경우 segment table에서 처리
2. 페이지 단위로 물리적인 메모리에 올리기

위와 같은 작업이 가능하기 때문에 두 기법의 장점을 모두 포함하고 있다!!

---
[강의]
http://kocw.net/home/m/search/kemView.do?kemId=1046323

[참고]
https://blog.naver.com/rannnneey/222975091775