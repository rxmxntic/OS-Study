Deadlock1

Deadlock = 교착상태

일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 사태

Resource(자원)

하드웨어, 소프트웨어 등을 포함하는 개념 

 ex) I/O device, CPU cycle, memory space, semaphore 등

프로세스가 자원을 사용하는 절차

1. 자원 요청

2. 자원 획득

3. 자원 사용

4. 자원 반납

​

Deadlock 발생 조건

Mutual exclusion (상호 배제)

매 순간 하나의 프로세스만이 자원을 사용할 수 있음

2. No preemption (비선점)

프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음

3. Hold and wait (보유대기)

자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음

4. Circular wait (순환대기)

자원을 기다리는 프로세스간에 사이클이 형성되어야 함

ex) 프로세스 P0, P1, ..., Pn이 있을 때

P0은 P1이 가진 자원을 기다림

P1은 P2가 가진 자원을 기다림

Pn-1은 Pn이 가진 자원을 기다림

Pn은 P0가 가진 자원을 기다림

​

→ 어느 하나를 만족하지 않으면 deadlock은 발생하지 않는다!

​

Deadlock이 발생했는지 알아보기 위해 자원할당그래프이용

​

Resource-Allocation Graph (자원할당그래프)



프로세스


자원

자원에서 프로세스로 가는 화살표 : 자원이 프로세스에 속해있는 방향

프로세스에서 자원으로 가는 화살표 : 프로세스가 자원을 요청한 상황

자원안의 점 : 자원의 인스턴스

​

P1(프로세스1)은 2번 자원을 가지고 있으면서 1번자원을 기다리는 상황, 그 1번 자원은 프로세스2가 가지고 있다. 프로세스2는 2번 자원을 하나 가지고 있으면서 3번 자원을 기다린다. 그 3번 자원은 프로세스 3한테 있는 상황이다.

​

Deadlock인지 아닌지 알 수 있는 방법

: 그래프에 cycle이 없으면 deadlock이 아니다.

→ 위 사진은 deadlock이 아니다!

​

Cycle이 있는 경우 


자원당 인스턴스가 하나밖에 없는 경우  그래프에 cycle이 생긴 경우 → deadlock

​

자원당 인스턴스가 여러개인 경우 → deadlock일 수도 아닐수도 


자원이 여러개 인스턴스가 있어 사이클은 있지만 deadlock이 아닌 경우에 속한다. 

Deadlock의 처리 방법

Deadlock Preventioin

자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

Deadlock  Avoidance

자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당

Deadlock Detection and recovery

Deadlock발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock발견시 recover

Deadlock Ignorance

Deadlock을 시스템이 책임지지 않음

UNIX를 포함한 대부분의 OS가 채택

​

Deadlock이 생기지 않게 방지 : Deadlock Preventioin, Deadlock  Avoidance 

Deadlock Preventioin

Mutual Exclusion

공유해서는 안되는 자원의 경우 반드시 성립해야 한다.

​

Hold and Wait

프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.

방법 1. 프로세스 시작 시 모든 필요한 자원을 할당받게 하는 방법

           → 모든 자원을 다 받아놓기 때문에 자원의 비효율 발생

방법 2. 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청

​

No Preemption

process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선정된다.

모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다.

State를 쉽게 save하고 restore할 수 있는 자원에서 줄 사용(CPU, memory)

​

Circular Wait

모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당

예를 들어 순서가 3인 자원 Ri를 보유 중인 프로세스 순서가 1인 자원 Rj을 할당받기 위해서는 우선 Ri를 release해야 한다.

​

→ Utilization 저하, throughput 감소, starvation 문제 발생 가능

Deadlock  Avoidance

자원 요청에 대한 부가정보를 이용해서 자원 할당이 deadlock으로부터 안전(safe)한지를 동적으로 조사해서 안전한 경우에만 할당

가장 단순하고 일반적인 모델은 프로레스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법

프로세스가 시작될때 쓸 최대량을 미리 알고 있다고 가정

→ 시스템이 unsafe state에 들어가지 않는 것을 보장

​

safe state

시스템 내의 프로세스들에 대한 safe seuence가 존재하는 상태

​

safe sequence

프로세스의 sequence < P1, P2, ..., Pn > 이 safe하려면 Pi (1 <= i <= n)의 자원 요청이

 "가용 자원 + 모든 Pj (j<i)의 보유 자원"에 의해 충족되어야 함

​

시스템이 safe state에 있으면 → no deadlock

시스템이 unsafe state에 있으면 → possibility of deadlock

​

Avoidance 알고리즘

자원의 인스턴스가 하나밖에 없는 경우 : Resource Allocation Graph algorithm

자원의 인스턴스가 여러개인 경우 : Banker's Algorithm

​

Resource Allocation Graph algorithm

이용가능한 자원을 다 내어주지 않고 deadlock의 위험성이 있는 경우에 애초에 자원 할당하지 않음


​

점선 : 지금은 아니지만 언젠간 자원을 요청할 수 있다라는 의미

​

프로세스 1번이 R1자원을, 프로세서 2번이 R2자원을 가지고 있음을 의미 → no deadlock

프로세스 1번이 자원 2번을 요청하여 점선이 실선으로 바뀐다면 cycle형성 → deadlock

​

Banker's Algorithm


P0는 B자원 1개, P1은 A자원 2개...를 가지고 있는 상황

최대로 P0는 A는 7개, B는 5개, C는 3개로 할당 받아놓는 상황

​

(Banker's Algorithm은 최악의 경우를 가정하고 있어)

자원을 요청하면 남아있는 자원을 줄 순 있지만 deadlock이 발생 가능성이 있다면 자원을 주지 않는다.


Deadlock의 처리 방법

Deadlock Preventioin

자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

Deadlock Avoidance

자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당

Deadlock Detection and recovery

Deadlock발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock발견시 recover

Deadlock Ignorance

Deadlock을 시스템이 책임지지 않음

UNIX를 포함한 대부분의 OS가 채택

Deadlock Detection and recovery

​

Deadlock Detection

- Resource type 당 single instance인 경우

자원할당 그래프에서의 cycle이 곧 deadlock을 의미 (뱅커스 알고리즘을 쓰는 편이 더 간단하긴 함!)

- Resource type 당 multiple instance인 경우

Banker's algorithm과 유사한 방법 활용

​

Wait-for graph 알고리즘

Resource type 당 single instance인 경우

Wait-for graph

자원할당 그래프의 변형

프로세스만으로 node 구성

Pj가 가지고 있는 자원을 Pk가 기다리는 경우 Pk → Pj

Algorithm

Wait-for graph에 사이클이 존재하는지를 주기적으로 조사

O(n2)  → n제곱

​

deadlock인 자원할당 그래프를 자원을 빼고 프로세스들로만 간단히 그린 버전


​

Banker's algorithm과 유사한 방법 활용


현재 상황은 데드락 없음. 

​

P1에서 A를 2개 가지고 있는데 A와 C 2개를 추가로 요청했지만 가용자원이 없어

이 요청을 받아드릴 수 없게 됨

​

P0과 P2가 요청한 자원이 없으므로 실행이 끝나면 알아서 할당된 걸 반납하므로 Available이 (0,1,0)+(3,0,3)=(3,1,3)이 되어

이후 사용할 프로세스 하나씩 실행하면 됨

​

만약)

P2에게 요청 (0,0,1)이 된다면!

: P0의 자원을 받아도 실행할 수 있는게 없으므로 데드락

​

Deadlock이 발견이 되면 Recovery 실행

​

Recovery

Process termination

→ 프로세스를 종료시키는 방법

방법1 ) deadlock인 모든 프로세스 없애기

방법2 ) deadlock인 프로세스를 하나씩 없애기

​

Resource Preemption

→ deadlock인 자원들을 뺏는 방법

비용을 최소화할 희생양을 선정

safe state로 rollback하여 프로세스를 다시 시작

starvation 문제

동일한 프로세스가 계속해서 희생양으로 선정되는 경우

cost factor에 rollback횟수도 같이 고려

Deadlock Ignorance

Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않음

: Deadlock을 생기지 않게 하기 위해 예방하는 것도 자원의 비효율적인 방법이기도 하고 

: Deadlock을 Detection하는 거 또한 시스템의 avoid가 크기 때문에 그렇게 하지 않고 운영체제와 시스템은 책임지지 않고 조치를 취하지 않는다!

​

Deadlock이 매우 드물게 발생하므로 deadlock에 대한 조치 자체가 더 큰 overhead일 수 있음

만약, 시스템에 deadlock이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 process를 죽이는 등의 방법으로 대처

UNIX, Windows 등 대부분의 범용 OS가 채택

​
