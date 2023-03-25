프로세스 문맥이란?

프로세스의 현재 상태를

정확하게 규명하기 위해서 필요한 모든 요소들

​

1. CPU 수행 상태를 나타내는 HW 문맥 (Program Counter, 각종 register)

"프로세스는 CPU를 잡고 매 순간 instruction을 실행합니다.

그래서 현재 시점의 프로세스가 instruction을 어디까지 실행을 했는지를

알기 위해선 register에 어떤 값을 넣고 있었고,Program Counter가

어딜 가리키고 있었는지에 대한 정보가 필요합니다."

​

2. Memory와 관련된 프로세스의 주소 공간(code,data,stack)

code,data,stack에 어떤 내용이 들어있는가?

​

3. 프로세스 관련 커널 자료 구조 (운영체제)

​

- PCB(Process Control Block)

"운영체제는 프로세스들이 들어올 때매 그 프로세스 들을 관리하기 위해서

PCB라는 자료구조를 두게 됩니다. 이 자료구조를 통해서 프로세스들에 CPU와

메모리를 얼마나 줘야되는지 평가하게 됩니다."

- Kernel stack

"프로세스가 본인이 할 수 없는일을 OS에게 대신 요청(System call)

프로세스의 주소공간을 가리키던 Program Counter가 커널 주소 공간을 가리키게 되고

커널의 코드를 실행하게 됩니다."(커널 또한 함수들로 구성되어 있기 때문에 함수 호출이

이루어지게 되면 stack에 관련정보를 쌓아 두게 됩니다..)

​

​

저희가 이런 문맥을 왜 알아야 될까요?

​

"현대의 프로세스는 하나의 프로세스만 실행되는게 아니라

여러 프로세스들이 번갈아서 실행이 되기 때문에 프로세스의 문맥을

파악하는 것이 필수라고 합니다..!"

​

​

프로세스의 상태(Process State)

​

프로세스의 상태도(16:51)

- Running :

프로세스가 CPU를 잡고 instruction을 수행중인 상태

- Ready :

CPU를 기다리는 상태 (CPU는 하나밖에 없기 때문에)

CPU를 받기만 하면 instruction이 실행되는 상태 (최소한의 메모리는 가지고 있어야 함)

- Blocked(wait,sleep) :

CPU를 줘도 당장 instruction을 수행할 수 없는 상태

Process 자신이 요청한 event(I/O)가 즉시 만족되지 않아

이를 기다리는 상태

ex) 디스크에서 file을 읽어와야 하는 경우

​

문맥 교환(Context Switch) :

사용자 프로세스 하나로부터 다른 사용자 프로세스로 넘어가는 과정

어떤 작업이 필요한가?

System call 이나 Interrupt 같은 발생은 context switch가 아니다

​

​

프로세스를 스케줄링하기 위한 큐​

* Job queue

현재 시스템 내에 있는 모든 프로세스의 집합

​

* Ready queue

현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는

프로세스의 집합

​

* Device queues

I/O device의 처리를 기다리는 프로세스의 집합

​

* 프로세스들은 각 큐들을 오가며 수행 된다.

​

스케줄러

* Long-term scheduler

시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정

프로세스에 memory를 주는 문제

degree of Multiprogramming을 제어

time sharing system에는 보통 장기 스케줄러가 없음

​

* Short term scheduler

어떤 프로세스를 다음번에 running시킬지 결정

프로세스에 CPU를 주는 문제

충분히 빨라야 함

​

* Medium-Term Scheduler

여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄

프로세스에게서 memory를 뺐는 문제

degree of Multiprogramming을 제어

​

​

지금의 시스템들은 장기 스켈듈러가 없고 중기 스케쥴러로 degree of Multiprogramming을 제어한다고 합니다.!

​

중개 스케듈러 도입으로 인해추가된 프로세스의 상태

* suspended

외부적인 이유로 프로세스의 수행이 정지된 상태

프로세스는 통째로 디스크에 swap out 된다

ex) 사용자가 프로그램을 일시 정지시킨 경우(break key)

시스템이 여러 이유로 프로세스를 잠시 중단 시킴

(메모리에 너무 많은 프로세스가 올라와 있을 때)