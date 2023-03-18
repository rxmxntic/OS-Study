Process Management

프로세스 생성 (Process Creation)

copy-on-write(COW)

 write발생 시 copy

부모 프로세스(Parent process)가 자식 프로세스(children process) 생성 → 복제 생성

효율적인 운영체제의 경우

             일단 복제하지 않고 자식 프로세스가 부모 프로세스의 주소공간을 공유 후

             부모와 자식의 내용이 달라진 순간 공유하던 일부를 자식 프로세스가 카피하여 생성

프로세스의 트리(계층 구조)형성

       부모 프로세스 아래에 여러개의 자식 프로세스

프로세스는 자원을 필요로 함

       운영체제로부터 받는다, 부모와 공유한다

자원의 공유

부모와 자식이 모든 자원을 공유하는 모델

일부를 공유하는 모델

전혀 공유하지 않는 모델

수행(Execution)

    1. 부모와 자식은 공존하며 수행되는 모델

    2. 자식이 종료(terminate)될 때까지 부모가 기다리는(wait)모델

주소공간(Address space)

자식은 부모의 공간을 복사함 (binary and OS data)

자식은 그 공간에 새로운 프로그램을 올림

유닉스의 예

fork()시스템 콜이 새로운 프로세스를 생성

            부모를 그대로 복사 (OS fata except PID + binary)

            주소 공간 할당

fork 다음에 이어지는 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올림

​

프로세스 종료(Process Termination)

프로세스가 마지막 명려을 수행한 후 운영체제에게 이를 알려줌(exit)

자식이 부모에게 output data를 보냄 (via wait)

프로세스의 각종 자원들이 운영체제에게 반납됨

부모 프로세스가 자식의 수향을 종료시킴 (abort)

자식이 할당 자원의 한계치를 넘어섬

자식에게 할당된 태스크가 더 이상 필요하지 않음

부모가 종료(exit)하는 경우

                운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않는다

                단계적인 종료

프로세스와 관련한 시스템 콜

fork()  -  create a child (copy)

exec()  -  overly new image

wait()  -  sleep until child is done

exit()  -  frees all the resources, notify parent

fork() 시스템 콜

프로세스는 fork()에 의해 시스템 생성


​

자식 프로세스의 경우 0이기 대문에 if 문 실행 

부모 프로세스의 경우 결과값이 양수이기 때문에 else if부터 실행

​

자식 프로세스의 경우

fork 이후부터 시작하기 때문에 Hello~! 실행 못함 


exec() 시스템 콜

프로세스는 다른 프로그램을 exec()를 통해 실행가능


​

execlp를 만나면 


이와 같이 새로운 곳에서 execlp의 프로그램을 덮어 씌우기 // execlp사용 후 다시 되돌아오기는 불가능

​

자식뿐만 아니라 부모에서도 execlp사용 가능!!


그러므로 execlp이후에 나오는 

Hello, I am parent!는 실행하지 못함

​

​

execlp("프로그램 이름", "프로그램 이름", "전달할 코드", "포인터")

wait () 시스템 콜

프로세스 A가 wait() 시스템 콜을 호출하면

커널은 child가 종료될 때까지 프로세스 A를 sleep시킨다. (block 상태)

Child process가 종료되면 커널은 프로세스 A를 깨운다. (​ready 상태)

​


exit() 시스템 콜

프로세스의 종료

자발적 종료

마지막 statement 수행 후 exit() 시스템 콜을 통해 프로그램에 명시적으로 적어주지 않아도

main 함수가 리턴되는 위치에 컴파일러가 넣어줌

​

비자발적 종료

 - 부모 프로세스가 자식 프로세스를 강제 종료시킴

    자식 프로세스가 한계치를 넘어서는 자원 요청

    자식에게 할당된 태스크가 더 이상 필요하지 않음

 - 키보드로 kill, break등을 사용한 경우

 - 부모가 종료는 경우

   부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨

​


Hi 출력 후 종료

프로세스 간 협력

독립적 프로세스 (Independent process)

프로세스는 각자의 주소 공간을 가지고 수행되므로 

원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함

​

협력 프로세스 (Cooperating process)

프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음

​

프로세스 간 협력 메커니즘(IPC: Interprocess Communication)

  메시지를 전달하는 방법

    message passing  :  커널을 통해 메시지 전달

주소 공간을 공유하는 방법

   shared memory

    : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음

​

thread :  thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 

동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능

​

​

Message passing 

Message system

플세스 사이에 공유 변수를 일체 사용하지 않고 통신하는 시스템

​

방법 2가지

1. Direct Communication

통신하려는 프로세스의 이름을 명시적으로 표시

2. Indirect Communication

mailbox (또는 port)를 통해 메시지를 간접 전달

CPU Scheduling

​

CPU 스케줄링 필요성

 여러 종류의 job(=process)이 섞여 있기 때문

Interactive job에게 적절한 response 제공 요망

CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용

​

CPU-burst Time의 분포

I/O bound job : CPU를 짧게 쓰고 I/O를 오래 쓰는 경우

CPU bound job : CPU를 오래 쓰는 경우

프로세스의 특성 분류

프로세스는 그 특성에 따라 다음 두 가지로 나눔

​

I/O bound process

CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job

(many short CPU bursts)

​

CPU bound process

계산 위주의 job

(few very long CPU bursts)

CPU Scheduler 

Ready상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 선택

​

 Dispatcher

CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘김

→ 이 과정을 context switch(문맥 교환) 이라고 부름

​

CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우

Running -> Blocked (ex. I/O 요청하는 시스템 콜)

Running -> Ready (ex. 할당시간 만료로 timer Interrupt)

Blocked -> Ready (ex. I/O 완료 후 인터럽트)

Terminate

​

nonpreemptive (강제로 빼앗지 않고 자진 반납)

→ →1, 4 스케줄링

​

All other scheduling 

preemptive (강제로 빼앗음)

→ →2, 3 스케줄링