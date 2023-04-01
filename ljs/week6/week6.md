CPU Scheduling 1

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

Scheduling Criteria

Performance Index(=성능 척도)

​

시스템에서의 성능 척도

CPU utilization(이용률) : 가능한한 많이 사용

Throughput(처리량) : 주어진 시간동안 몇개의 작업을 완료했는지

​

프로세스에서의 성능 척도

Turnaround time(소요시간, 반환시간) : CPU의 대기시간부터 사용 시작과 끝날때까지 걸린 시간

Waiting time(대기 시간) : CPU사용 시작 전까지 기다리는 시간

Response time(응답 시간) : CPU를 최초 얻기까지 걸리는 시간 (time sharing을 위해)

​

waiting time과 response time의 차이점

선점형 스케줄링의 경우

waiting time : CPU할당 받은 후 뺏겼다 다시 할당까지의 시간을 모두 합친 시간

response time : CPU를 최초 얻기까지 걸리는 시간

FCFS (First-Come First-Served)

비선점형 스케줄링

프로세스 도착순서 :  P1, P2, P3

스케줄링 순서 :  P1, P2, P3

​

0 ---------------->  24(P1)  ----->  27(P2)  ----> 30(P3)

​

waiting time

 P1 = 0, P2 = 24, P3 = 27

average waiting time

(0 + 24 + 27)/3 = 17

​

긴시간의 프로세스가 먼저 도착하여 짧은 시간의 프로세스가 오래 기다려야 하는 경우

: Convoy effect

​

---> 좋은 스케줄링 방법은 아니다.

​

if) CPU를 짧게 쓰는 프로세스들이 먼저 도착했다면

프로세스 도착순서 :  P2, P3, P1

스케줄링 순서 :   P2, P3, P1

​

0 ---->  3(P2)  ---->  27(P3)  ---------------------> 30(P1)

​

waiting time

 P1 = 6, P2 = 0, P3 = 3

average waiting time

(6 + 0 + 3)/3 = 3

SJF (Shortest-Job-First)

각 프로세스의 다음번 CPU burst time을 가지고 스케줄링에 활용

CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄

​

2가지 경우

 - Nonpreemptive

일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점당하지 않음

 - Preemptive

현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하여 CPU를 빼앗김

이 방법을 Shortest-Remaining-Time-First(SRTF)이라고도 부름

​

---> 주어진 프로세스들에 대해 최소 평균 대기 시간 보장

​

​

 Nonpreemptive의 SJF

Process

Arrival Time

Burst Time

P1

0.0

7

P2

2.0

4

P3

4.0

1

P4

5.0

4

먼저 도착한 P1 → CPU를 짧게 쓰는 P3에 할당

0 --------> 7(P1) → 8(P3) ----> 12(P2) ----> 16(P4)

​

평균 대기 시간

(0 + 6 + 3 + 7)/4 = 4

​

​

 Preemptive의 SJF

Process   Arrival Time   Burst Time

P1          0.0              7

P2          2.0              4

P3          4.0              1

P4          5.0              4

먼저 도착한 P1 → 2의 시점에서 CPU를 가장 짧게 쓰는 P2에 할당

0 --> 2(P1) --> 4(P2) → 5(P2) --> 16(P2) ----> 11(P4) -----> 16(P1)

​

평균 대기 시간

(9 + 1 + 0 + 2)/4 = 3

​

---> 모든 스케줄링 알고리즘 중 가장 짧은 평균 대기 시간 

​

SJF의 문제점 

기아 현상(Starvation) : 우선순위가 낮은 프로세스가 지나치게 오래 기다리게 됨

→ 영원히 cpu할당을 못받을 가능도 있음

CPU 사용시간을 미리 알 수 없음

​

​

다음 CPU Burst Time의 예측

다음번 CPU burst time을 어떻게 알 수 있는가?

추정만이 가능

과거의 cpu burst time을 이용해서 추정 (exponential averaging)

Priority Scheduling (우선순위 스케줄링)

우선순위가 가장 높은 프로세스에 cpu 할당

preemptive : 우선순위가 가장 높은 프로세스에 cpu를 할당한 후 뺏기 가능

nonpreemptive : 우선순위가 가장 높은 프로세스에 cpu를 할당한 후 뺏기 불가능

​

SJF는 일종의 우선순위 스케줄링

우선순위 = 예측한 다음 cpu burst time

​

Priority Scheduling의 문제점

기아 현상(Starvation) : 우선순위가 낮은 프로세스가 지나치게 오래 기다리게 됨

→ 영원히 cpu할당을 못받을 가능도 있음

 해결책

노화 (Aging) : 아무리 낮은 우선순위를 갖고 있더라고 시간이 지나면 우선순위 조금씩 높여주기

Round Robin(RR)

각 프로세스는 동일한 크기의 할당 시간(time quantum)을 가짐 (일반적으로 10-1000 milliseconds)

​

할당 시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을 섬 

→ 응답시간이 빨라짐

​

n개의 프로세스가 ready queue에 있고 할당 시간이 q time unit인 경우 각 프로세스는 최대 q time unit단위로 CPU 시간이 1/n을 얻음

어떤 프로세스도 (n-1)q time unit이상 기다리지 않음 

→ 대기시간이 cpu사용의 시간과 비례

​

Performance

q large(할당시간을 크게) → FCFS

q small(할당시간을 작게) → context switch 오버헤드가 커짐

​

Round Robin(RR) with time quantum = 20

Process    Burst Time

P1           53

P2           17

P3           68

P4           24

P1   P2   P3   P4   P1   P3   P4   P1   P3   P3

0,20 ,37  ,57  ,77  ,97 ,117 ,121 ,134 ,154 ,162

일반적으로 SJF보다 평균 소요시간은 길지만 응답 시간은 더 짧다

​