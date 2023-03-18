Process 2, 3

Thread

'A thread (or lightweight process) is a basic unit of CPU utilization'

​

Thread의 구성

program counter

register set

stack space

​

Thread가 동료 Thread와 공유하는 부분(=task)

code section

data section

OS resources

​

전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있다.

Thread 사용의 장점

1. Responsiveness (응답성) 

   : 파일을 읽어오는 동안 미리 할 수 있는 작업 먼저 실행, 일종의 비동기식 입출력

2. Resource Sharing (자원 공유)

   : 똑같은 일을 하는 경우에 별도의 프로세스 사용 대신 

     하나의 프로세스 안에 cpu 수행 단위만 여러개를 두어 코드, 데이터, 자원을 스레드들이 공유 

→ 자원을 효율적으로 사용 가능

3. Economy (경제성)

   : thread간 cpu switching

     같은 작업을 하는 경우 프로세스를 여러개 만드는 것보단 프로세스 안에 thread를 두는것이 보다 효율적

4. Utilization of MP Architectures

   : 프로세스는 하나지만 스레드가 여러개인 경우 각각의 스레드가 서로 다른 cpu에서 병렬적으로 작업 가능

​

다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked(waiting) 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행(running)되어 빠른 처리 가능

동일한 일을 수행하는 다중 스레드와 협력으로 높은 처리율(throughput)과 성능 향상

스레드를 사용하여 병렬성 향상

각 프로세스마다 하나의 PCB 생성

PCB 구조

OS 관리용 정보

pointer, process state, process number 

 CPU 관련 정보 (수 words, Thread, lightweight)

program counter, registers

자원 관련 정보

memory limits, list of open files ...

→ 전체 KB (process, heavyweight)

​

        


주소 공간에서 코드와 데이터는 공유하고 stack부분에서 Thread마다 별도로 공간 차지

Implementation of Threads

kernel threads : kernel

ex. Windows 95/98/NT, Solaris, Digital UNIX, Mach

 OS의 커널이 thread가 여러개인 사실을 알고 하나의 스레드에서 다른 스레드로 cpu가 넘어가는 걸 

커널이 스케줄링 하듯 넘겨줌 

2.user threads :  library 

ex. POSIX Pthreads, Mach C-threads, Solaris threads

user threads는 라이브러리를 통해 지원

프로세스 안에 thread가 여러개인 사실을 os는 모르고 사용자 프로그램이 스스로 여러개의 threads를 관리