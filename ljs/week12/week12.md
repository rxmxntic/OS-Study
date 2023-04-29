Management 2, 3-1

Noncontiguous allocation (불연속 할당)

​

1. Paging 

Paging 

Process의 virtual memory를 동일한 사이즈의 page 단위로 나눔

Virtual memory의 내용이 page단위로 불연속하게 저장됨

일부는 backing storage에, 일부는 physical memory에 저장

​


Basic Method

physical memory를 동일한 크기의 frame으로 나눔

logical memory를 동일 크기의 page로 나눔​ (frame과 같은 크기)

모든 가용 frame들을 관리

page table을 사용하여 logical address를 physical address로 변환 (주소변환을 위해 사용)

page table : 인덱스를 이용하여 physical memory로 접근 가능

External fragmentation 발생 안함

Internal fragmentation 발생 가능

​

page table

main memory에 상주

Page-table base register(PTBR)가 page table을 가리킴

Page-table base register(PTBR) : 메모리 상에 페이지 테이블이 어디에 있는지, 시작 위치 포함

Page-table length register(PTLR)가 테이블 크기를 보관

모든 메모리 접근 연산에는 2번의 메모리 접근 필요

page table 접근 1번, 실제 data/instruction 접근 1번

(메모리 2번 접근으로 인해 시간이 오래걸리기 때문에 ) 속도 향상을 위해 별도의 하드웨어 사용

associative register 혹은 translation look-aside buffer (TLB, 메인메모리와  cpu 사이에 존재)라 불리는 고속의 lookup 하드웨어 캐시 사용

​

Translation Look-aside Buffer (TLB)

​


 

TLB(Translation Look-aside Buffer): 메모리 주소 변환을 위한 별도의 캐시 메모리

메인메모리와 cpu 사이에 존재하여 주소 변환을 해주는 계층

page table에서 빈번히 참조되는 일부 엔트리를 캐싱

TLB는 key-value 쌍으로 데이터를 관리하고 key에는 page number, value에는 frame number가 대응

​

CPU는 page table보다 TLB를 우선적으로 참조하여,

만약 원하는 page가 TLB에 있는 경우 곧바로 frame number를 얻을 수 있고, 

그렇지 않은 경우 메인 메모리에 있는 page table로부터 frame number를 얻을 수 있다.

 

원하는 엔트리가 TLB에 존재하는지 알기 위해선 TLB 전체를 다 찾아봐야 한다.

하지만, parallel search가 가능 →  탐색하는 시간은 적다. 

​

TLB의 성능을 높이고 싶다면 page의 크기를 키우는 방법이 있다. 

​

 page의 크기를 키우는 방법

TLB에서 찾아지는 비율 = a

TLB를 탐색하는데 걸리는 시간 =  b

​

메모리 접근 횟수의 기댓값 =  TLB에서 찾은 경우 + 못 찾은 경우 = (b+1)*a + (b+2)*(1-a) = 2+b-a

​

(b는 일반적으로 매우 작은 값이고, a는 값이 큼)

따라서 기존의 메모리 접근 횟수인 2보다 훨씬 작은 값이 된다. 

Structure of the Page Table

현대 컴퓨터는 주소 공간이 매우 큰 프로그램을 지원

 32 bit 주소를 사용하는 경우 232  = 4GB의 주소 공간을 사용

 이때 page의 크기가 4KB이면 약 100만 개의 page table entry가 필요

 그러나 대부분의 프로그램은 4GB 중 매우 일부분만 사용하므로 page table 공간이 심하게 낭비

​

이를 해결하기 위해 page table을 효율적으로 구성하는 몇 가지 방법이 있다. 

Multi-level paging,  Hashed Page Table, Inverted Page Table

1. Multi-level paging

 Two-Level Page Table

논리적 주소 공간을 여러 단계의 page table로 분할하여 사용되는 page의 page table만 할당하는 기법

​

page table과 메모리 사이에 page table을 하나 더 두어 두 단계를 거치는 방법

이를 통해 모든 page를 로드해야 하는 수고를 줄일 수 있다.


기존의 경우

 32 bit 논리적 주소 공간이라면, 20 bit는 page number, 12 bit는 page offset을 나타냄

​

Two-level인 경우

page table 자체가 page로 구성

page table에 관한 값인 20bits로 구성된 page number를 10bit, 10bit로 나눠서 구성

즉, page number는 10 bit의 page number와 12 bit의 page offset으로 또 나뉘게 된다. 


최종적 logical address

p1 : outer page table의 index

p2 : outer page table의 page에서의 배치

즉, p1과 p2가 결합하여 outer page table이 가진 값을 가져옴

​


만약 주소 공간이 더 커지면 Multi-level page table이 필요하다. 

이 경우 각 단계의 page table이 모두 메모리에 존재하기 때문에 더 많은 메모리 접근이 필요

이는 TLB로 해결가능

​

→ 사용하는 이유 : 주소공간이 커지면서 다단계 페이지 테이블을 필요로 하는데 

각 단계의 페이지 테이블의 메모리 변환에 더 많은 메모리 접근을 필요로 하기 때문에 캐시메모리를 통해 메모리 접근 시간을 줄일 수 있음!

MeMory Protection

Protection bit : page에 대한 접근 권한(read/write/read-only)


​

Valid-invalid bit

bit가 valid인 경우, 해당 주소의 frame에 그 프로세스를 구성하는 유효한 내용이 있다.(접근 허용),

    관련 page가 프로세스의 논리적 주소 공간에 있다.

bit가 invalid인 경우, 해당 주소의 frame에 유효한 내용이 없음을 뜻한다.(접근 불허),

    page가 프로세스의 논리적 주소 공간에 없다.

​

invalid가 발생할 경우, OS가 인터럽트의 한 종류인  trap을 발생시킨다.

그래서 trap에 의해 invalid한 페이지를 논리적 주소 공간에 들여오는 작업 수행

2. Inverted Page Table

지금까지의 page table은 각 page마다 하나의 항목을 가졌다. 

반대로 Inverted page table은

메모리의 frame마다 한 항목씩 할당하는데(논리적인 주소로), 그러면 physical frame에 대응하는 항목만 저장하면 되므로 메모리를 훨씬 적게 사용한다. 

​

각 page table entry는 각각의 메모리의 frame이 담고 있는 내용(PID, logical address)을 표시한다.

​

다만 테이블 전체를 탐색해야 하므로 시간이 오래 걸리는 단점이 있어 대부분의 메모리는 Hashed page table과 Inverted page table의 결합으로 이루어져 있다.  

​


​

출처: http://kocw.net/home/search/kemView.do?kemId=1046323

강의: 반효경( 이화여자대학교, 운영체제)

​