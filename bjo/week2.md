목적 :

컴퓨터 시스템의 동작원리를 이해하기 위해서

​


​

​

[컴퓨터 시스템 구조]

컴퓨터 내부장치 : Cpu + Memory(cpu의 작업공간)

컴퓨터 외부장치 : I/O device 입출력 장치(디스크,키보드,모니터,네트워크 장치)

​

Deivce controller :

각각의 I/O 장치들은 각 장치를 전담하는 작은 CPU가 붙에 있게 되는데

그건 device controller라고 부릅니다.

CPU는 device controller에게 데이터를 읽어오라고 일을 시키고,

그럼으로써 CPU는 메모리 작업을 하고 I/O device의 작업은 device controller에게 시켜

일을 분담하게 됩니다.

(disk의 내부를 통제하는 것은 cpu의 역할이 아니고 device controller의 역할입니다.)

​

​

Local buffer :

여기서 메인 cpu의 작업공간인 Main Memory가 있듯이

이러한 device controller들도 그들의 작업 공간이 필요합니다.

그걸 Local buffer라고 부릅니다.

​

​

Registers :

CPU안에 Memory보다 빠르면서 정보를 저장할 수있는 공간들

​

​

Mode bit :

CPU안에서 실행되는 것이 OS인지 사용자 프로그램인지를 구분해주는 역할을 합니다.

​

mode bit 0일 때 ==> OS가 CPU를 갖고 있기 때문에

메모리접근, I/O device접근권한을 다 갖게 됨 

​

mode bit 1일 때 ==> 사용자 프로그램이 CPU를 갖고 있기 때문에 제한된

Instruction만 CPU에서 실행이 가능합니다.(보안측면)

​

​

Interrupt line :

CPU에 붙어 있으면서 I/O장치에서 Local buffer로 내용을 다 읽어들어왔다면,

이것은 CPU에게 알려야 하는데, 이 알리는 행위를 하기위해 Interrup line이 존재합니다.

Interrupt line이 있기 때문에 CPU는 자신의 일을 하다가, Interrup line에 신호가 들어오면,

하던 일을 멈추고, Interrupt와 관련된 일을 수행합니다.

​

​

Timer 의 동작 :

특정 프로그램이 CPU를 독점하지 못하게 막는 하드웨어 입니다.

예를들어 for문이나 while문을 통해 무한루프가 들어오게 될 시,

CPU는 그 프로그램에서 빠져나오지 못하고 분할작업을 하지 못하게 됩니다.

​

여기서 OS가 CPU를 가지고 있다가 여러 프로그램이 실행될 때 CPU를 넘겨주게 되는데

그래서 CPU가 Instruction을 실행하다가 정해진 시간이 되면 timer는 CPU에게 Interrupt를

걸어주게 됩니다. 그럼으로써 CPU제어권은 OS에게 넘어갑니다.

​

이 과정에서 또한 timer에게 값을 셋팅한 다음 넘겨주게 되고

promgram이 CPU를 쓰다가 시간만료가 되면 timer interrup가 들어오고 CPU제어권이 OS에게

넘어가고 그 OS는 다음 프로그램에게 CPU를 넘겨주고 이런 과정들을 OS가 관리해주게 됩니다.

​

​

DMA controller :

직접 Memory를 접근할 수 있는 controller

I/O 장치가 너무 자주 Interrupt를 걸게 되면 CPU가 방해를 많이 받기 때문에

DMA controller를 통해 중간에 local buffer에 들어오는 작업이 끝났으면

DMA가 직접 메모리에 복사합니다. 그렇게 작업이 끝난 후 CPU한테 

Interrupt를 한번만 걸어주기 때문에 CPU를 효율적으로 관리할 수 있습니다.

​

추가로 알아야 될 개념 System call

​

​

출처: http://kocw.net/home/search/kemView.do?kemId=1046323

강의: 이화여자대학교, 반효경(운영체제)