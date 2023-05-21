Management 3-2, 4

Shared Page

Shared code

 = Re-entrant Code(= Pure code), 재진입가능 코드

read-only로 하여 프로세스 간에 하나의 code만 메모리에 올림 (코드 공유)

(eg. text editors, compilers, window systems)

Shared code는 모든 프로세스의 논리적 주소 공간에서 동일한 위치에 있어야 함

​

Private code and data

각 프로세스들은 독자적으로 메모리에 올림

Private data는 논리적 주소 공간의 아무곳에 와도 무방

​

Segmentation

프로그램은 의미 단위인 여러 개의 segment로 구성

작게는 프로그램을 구성하는 함수 하나하나를 세그먼트로 정의

크게는 프로그램 전체를 하나의 세그먼트로 정의

일반적으로는 ​code, data, stack 부분이 하나씩의 세그먼트로 정의

segmeatation 구조

Protection(보안)

각 세그먼트 별로 protection bit이 있음

각 entry :

 Valid bit = 0 →illegal segment

Read/Write/Execution 권한 bit

​

Sharing(공유)

shared segment

same segment number

segmentation 기법은 의미 단위이기 때문에 공유와 보안에 있어 페이징 기법보다 효과적!

​

Allocation

first fit / best fit

external fragmentation 발생

segment의 길이가 동일하지 않으므로 가변분할 방식에서와 동일한 문제점들이 발생

​

​

logical address의 구성

1. segment-number 

2. offset (세그먼트 안에서 얼마큼 떨어져 있는지 나타냄 )

​

서로 다른 물리적인 메모리 위치에 올라가야 하므로 segment별 주소변환을 위해 Segment table을 사용해줌

​

segment table

base register : 세그먼트 시작 위치를 표시함

limit register :  세그먼트 테이블의 길이를 표시함 (세그먼트의 수)

​

Segment-table base register (STBR)

: 물리적 메모리에서의 segment table의 위치

​

Segment-table length register (STLR)

: 프로그램이 사용하는 segment의 수

​

주소변환을 할때 확인해봐야 할 2가지

 CPU에 주어진 주소에서 

논리적인 주소의 세그먼트 번호가 세그먼트 테이블의 길이 레지스터(STLR)보다 작은 값인지

2. 세그먼트 테이블의 세그먼트 길이보다 세그먼트의 오프셋 값이 더 크진 않은지

​

segmenatation 기법의 단점

external fragmentation : segment의 길이가 동일하지 않으므로 가변분할 방식에서와 동일한 문제점들이 발생

 →  그러므로 first fit / best fit 사용!

​

segmenatation 기법의 장점

의미단위의 작업에선 페이징기법보단 효율적

segment table에는 세그먼트의 시작위치와 세그먼트의 길이가 얼마인지를 담고 있다.

만약 오프셋 값이 세그먼트의 길이보다 더 크다면 적잘한 메모 참조가 아니다.

​

이것을 방지하기 위해서 세그먼트 테이블에서 적절한 길이인지 체크해보고 

적절하다면 주소변환이 가능하게 해준다.

​

만약 5번의 세그먼트를 요청했는데 세그먼트를 3개만 사용하고 있다면?

5번은 잘못된 주소이다. 이것을 체크하기 위해  세그먼트의 번호와 STLR의 값과 비교

페이징 vs 세그먼테이션

세그먼테이션을 사용하는 경우

의미 단위의 일, 공유/보안의 일

페이지의 크기가 균일하지 않기 때문에 물리적인 메모리에 올려놀으면 중간중간에 hole들이 생겨 큰 세그먼트의 경우에 들어갈 수 없게 된다  → 이럴 경우 페이징 기법으로 대체

​

페이징을 사용하는 경우

물리적인 메모리에 조각이 발생할 경우가 없기 때문에  비어있는 프레임을 언제든지 활용 가능

그러나 테이블을 위한 메모리 낭비가 심함

pure segmentation과의 차이점

segment-table-entry가 segment의 base address를 가지고 있는 것이 아니라 segment를 구성하는 page table의 base address를 가지고 있다

의미단위로 해야하는 공유나 보안의 일은 segment table에서 처리하고 실제로 물리적인 메모리에 올라갈때는 페이지 단위로 나눠져 두가지 장점을 모두 포함하고 있다.

​

논리적 주소에서의 세그먼트 안에서 오프셋을 다시 나누어 앞부분은 페이지 번호를 사용하고 뒷부분을 오프셋으로 사용한다.

​

출처: http://kocw.net/home/search/kemView.do?kemId=1046323

강의: 반효경( 이화여자대학교, 운영체제)