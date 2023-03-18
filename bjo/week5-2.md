프로그램이 실행이 되면 어떤 프로그램이든 간에

CPU burst와 I/O burst가 번갈아가며 실행이 됩니다..!

즉 프로그램의 실행은 CPU burst와 I/O burst의 연속입니다 

​


CPU burst, I/O burst가 뭐에요?

CPU burst = > CPU명령을 실행하는 것

I/O burst = > I/O를 요청한다음 기다리는 

​

[CPU-burst Time의 분포]


I/O bound job : CPU를 짧게 쓰고 I/O작업을 하는 것 (우선적으로 cpu줘보자 효율성!)

CPU bound job : CPU만 오래 쓰는 작업

​

결론 : 여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케듈링이 필요

​

CPU Schduler & Dispatcher

CPU Scheduler(운영체제 안에서 스케듈링하는 코드가 있음,CPU를 누구에게 줄지 결정)

Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.

Dispatcher(CPU를 넘겨주는 역할)

CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.

이 과정을 context switch(문맥 교환)라고 한다.

CPU 스케듈링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다.

1. Running -> Blocked (예 : I/O 요청하는 시스템 콜)

2. Running -> Ready (예 : 할당시간만료로 timer interrupt)

3. Blocked -> Ready (예 : I/O 완료 후 인터럽트)

4. Terminate

1 ~ 4에서의 스케쥴링은 nonpreemptive (=강제로 빼앗지 않고 자진 반납)

All other scheduling is preemptive​ (=강제로 빼앗음)

​