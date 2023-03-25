목적 :

동기식 - 비동기식 입출력의 구분 / Thread란 무엇인가

​

​

[동기식 입출력]

I/O 요청 후 입출력 작업이 완료된 후에 제어가 사용자 프로그램에 넘어감

구현 방법 1

I/O가 끝날 때까지 CPU를 낭비시킴

매시점 하나의 I/O만 일어날 수 있음

구현 방법 2

I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음

I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움

다른 프로그램에게 CPU를 줌

[비동기식 입출력]

I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

​

[Thread 란 lightweight preocess]

프로세스(하나) 내부에 cpu 수행 단위가 여러개가 있는 경우

(프로세스를 별도로 두는 것보다 프로세스 안에 쓰레드를 여러개 두는게

cpu 사용률을 상승시킨다고 합니다..!)

📌 Thread의 구성

program counter

register set

stack space

​

📌 Thread가 동료 thread와 공유하는 부분(=task)

code section

data section

OS resources

​

CPU관련 정보인 program counter와 registers는

쓰레드마다 별도로 가지고 있습니다.


프로세스의 주소공간 // 하나의 프로세스 마다 만들어지는 PCB

✨ Thread 사용의 장점

다중 Thread로 구성된 태스크 구조에서는 하나의 서버 Thread가 blocked(waiting)

상태인 동안에도 동일한 task 내의 다른 Thread가 실행(running)되어 빠른 처리를 할 수 있다.

동일한 일을 수행하는 다중 Thread가 협력하여 높은 처리율과 성능 향상을 얻을 수 있다.

Thread를 사용하면 병렬성을 높일 수 있다.

Responsiveness - Resource Sharing - Economy - Utilization of MP Architectures

​

​

출처: http://kocw.net/home/search/kemView.do?kemId=1046323

강의: 이화여자대학교, 반효경(운영체제)