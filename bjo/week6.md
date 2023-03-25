블로그 카테고리 목적 :

🔎 CPU Scheduling의 방법, 특징을 이해하기 위해

 

복습 :

프로그램 실행시 Cpu burst 와 I/O burst가 번갈아 가면서 실행이 됩니다.


이때,

I/O 작업이 별로 없는 경우에는 CPU에서 Instruction을 실행하는 단계인

Cpu burst가 길게 나타납니다.

반대로,

I/O 작업이 많은 경우에는 CPU를 연속적으로 쓰는 단계가 짧아지게 되고,

I/O burst가 길게 나타나게 되는데 이는 Interrupt가 빈번히 발생하기 때문입니다.

(결국 CPU Scheduling은 이런 CPU busrt 시간과 I/O burst 시간을 적절히 조절하기

위해 필요합니다..!)

 

[CPU Schduler]

현재 ready queue에 들어가 있는 프로세스 중에서

어떤 프로세스에게 CPU를 줄지 결정하는 매커니즘 입니다.

 

[CPU Schduler의 2가지 이슈]

어떤 프로세스에게 CPU를 줄 것인가?
프로그램에게 CPU를 준 후 계속사용하게 할 것인가? 중간에 CPU를 뺐을 것인가?
(nonpreemptive//preemptive)

(CPU burst가 긴 Process에게 CPU를 줬는데 ready queue에 짧게 쓰고 나갈 I/O

instruction이 있다면 어떻게 줄 것인가?에 대해서 효율성을 잘 따져봐야 될 것입니다..!)

 

[Scheduling Criteria]

시스템 입장에서의 성능 척도(CPU 1개 가지고 최대한 일을 많이 시켜보자!)
CPU Utilization (전체 시간 중에서 CPU가 일한시간이 얼마나 되는가) 이용료
최대한 CPU에게 많이 일을 시켜서 전체 시간 중 CPU가 일한 시간의 비율을 높이는 게 좋음
Throught (주어진 시간 중에서 몇개의 일을 처리했는지에 대한 비율)
주어진 시간 중에서 CPU가 일을 많이 처리 하는 것이 좋음
2. 고객 입장에서의 성능 척도(프로세스가 빨리 처리되는 것이 중요!)

Turnaround Time (CPU를 쓰러와서 다 쓰고 나갈 때까지 걸린 시간)
Waiting Time과 CPU를 얻어서 작업을 끝내고 빠져나갈 때까지 걸린 총 시간
Waiting Time (ready queue에서 순수하게 기다린 시간)
CPU의 할당 반납 과정중 Waiting Time의 총합
Response Time (ready queue에 들어와서 처음으로 CPU를 얻기까지 걸린시간)
 

[Algorithm of CPU Schduler]

nonpreemptive (CPU를 강제로 빼았지 않는다.)
preemptive (CPU를 강제로 빼았는다.)
(현대적인 스케듈러 방식은 거의 대부분 preemptive한 방법을 사용하고 있다고 하네요!)

FCFS(First-Come First-Served) ✨ 먼저 온 순서대로 처리 (nonpreemptive)
FCFS 방식의 경우 앞에 어떤 프로세스가 위치하느냐에 따라서
Waiting Time에 상당한 영향을 끼치게 됩니다.

Convoy effect : 긴 프로세스들이 먼저 도착해서 짧은 프로세스들이 지나치게 많은 시간을 기다려야 하는 것

 

2. SJF (Shortest-Job-First) ✨ 짧은 프로세스를 먼저 배치하여 CPU관리 (nonpreemptive,preemptive)


non-preemptive 경우 :

SJF 스케줄링은 ready queue에 있는 프로세스 중에서 실행 시간이 가장 짧은

작업부터 CPU를 할당하는 비선점형 방식으로 최단 작업 우선 스케줄링이라고도 합니다.

(FCFS 스케줄링의 convoy effect를 완화하여 시스템의 효율성을 높였다고 하네요..!)

 

preemptive 경우 :

새로 들어온 process가 수행 중이던 job을 중단시키고 수행시킬 수 있습니다.

 

🚨 SJF 방식의 2가지 문제점 :

Starvation 문제 : CPU사용이 긴 프로세스는 영원히 서비스를 못 받을 수도 있는데
이는 SJF방식이 극단적으로 CPU사용이 짧은 Job을 선호하기 때문입니다.

2. CPU사용 시간을 추정할 수는 있지만 CPU사용시간을 미리 알 수 없음

 


 
​(오래된 사용량일 수록 예측할 때 가중치가 줄어들어 평가됩니다)
 
3. Priority Scheduling 우선순위 스케줄링 (nonpreemtive, preemtive)

(우선순위가 높은 process에게 먼저 CPU를 줄게)

preemtive :

우선순위가 더 높은 process가 들어오면 cpu 할당

nonpreemtive :

우선순위가 더 높은 process가 들어오더라도 cpu 보장

 

🚨 Priority Scheduling의 문제점

우선순위가 낮은 프로세스는 지나치게 오래 기다릴 수 있습니다.

해결책 :

Aging기법을 통해 낮은 프로세스들의 우선순위를 높여줄 수 있습니다.

기준은 기다린 정도입니다.

 

4. Round Robin 현대적인 CPU 스케줄링 기법 (preemtive)

Round Robin방식에서 각 프로세스마다 CPU가 동일한 할당 시간으로 셋팅됩니다.

할당 시간이 끝나면 Timer interrupt에 의해 CPU를 빼았기게 되고 ready queue에

맨 뒤로 밀려나게 되는 과정을 반복합니다.

 

✨Round Robin방식의 좋은점

CPU를 최초로 얻기까지의 과정이 빠르다.

모든 프로세스가 CPU를 할당 받을 수 있다.

 

이상으로 CPU Scheduling 발표를 마치겠습니다.

출처: http://kocw.net/home/search/kemView.do?kemId=1046323

강의: 이화여자대학교, 반효경(운영체제)