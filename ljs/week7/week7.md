Process Synchronization 1

동기화 문제

공유 데이터에 접근하는 코드 

: critical section (임계 구역)

​

공유데이터에 동시에 접근하면 안되기 때문에 entry section 사용

​

두 개의 프로세스가 있다고 가정 P0, P1

프로세스들의 일반적인 구조

﻿do {
	entry section # 공유데이터 접근 이전에 lock을 걸어 동시 접근 제어
	critical section
	exit section # lock을 풀어 다른 프로세스가 critical section 접근 허용
	remainder section
} while (1);
프로세스들은 수행의 동기화를 위해 몇몇 변수를 공유할 수 있다 → synchronization variable

​

프로그램적 해결법의 충족 조건

Mutual Exclusion (상호 배제)

프로세스가 Pi가 critical section부분을 수행 중이면 다른 모든 프로세스들은 그들의 sritical section에 들어가면 안된다.

2. Progress

아무도 critical section에 있지 않은 상태에서 critical section에 들오가고자 하는 프로세스가 있으면 critical section에 들어가게 해주어야 한다

3. Bounded Waiting (유한 대기)

프로세스가 critical section에 들어가려고 요청한 푸부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다.

 

→ 가정

모든 프로세스의 수행 속도는 0보다 크다.

프로세스들 간의 상대적인 수행 속도는 가정하지 않는다.

소프트웨어 적으로 lock/unlock하는 알고리즘 3가지

Algorithm 1 

변수 : int turn

           initially turn = 0

process P0

int turn;
do {
	while (turn != 0); # my turn, 내차레가 될때까지 while문 실행 -> 1번 process 차례
	critical section
	turn = 1; # your turn
	remainder section
} while (1);
Process P1

do {
	while (turn != 1); # my turn
	critical section
	turn = 0; # your turn
	remainder section
} while (1);
→ 충족조건의 첫번째인 상호배제는 만족하지만 progress를 만족하지 못함

​

즉, 과잉양보 : 반드시 한번씩 교대로 들어가야만 함 (swap-turn)

                        그가 turn을 내값으로 바꿔줘야만 내가 들어갈 수 있음

Algorithm 2

변수 : boolean flag[2]; → flag중 하나는 i에, 다른 하나는 j에

           initially flag [모두] = false;

process Pi

do {
	flag[i] = true;   #critical section에 들어가기 위해 본인의 flag를 true로
    while (flag[j]);  # 상대가 critical section에 있는 경우 기다림
	critical section
	flag[i] = false;  # critical section 나올 때 본인의 flag를 false
	remainder section
} while (1);
i가 본인의 flag인 상태에서 cpu를 뺏기면 j와 i모두 flag를 들고 있는 상황이 되어 

아무도 critical section을 수행하지 못하는 상황 발생

​

→ 충족조건의 첫번째인 상호배제는 만족하지만 progress를 만족하지 못함

Algorithm 3 (Peterson's Algorithm)

= 알고리즘 1 + 알고리즘 2

process Pi

do {
	flag[i] = true;   #critical section에 들어가기 위해 본인의 flag를 true로
    turn = j;         # turn을 상대방으로 바꿔줌
    while (flag[j] && turn == j);  # 상대가 기다리고, 상대방의 차례인경우 -> 본인은 기다림
	critical section
	flag[i] = false;  # critical section 나올 때 본인의 flag를 false로 상대방이 들어갈 수 있게 함
	remainder section
} while (1);
→ 3가지 조건 충족

​

문제점

Busy Waiting (= spin lock)

 : 계속 CPU와 memory를 쓰면서 기다리기 때문에 비효율적 (while문에서 계속 사용하고 있음)

Synchronization Hardware

데이터를 읽는 것과 쓰는 것을 하나의 명령어로 처리할 수 없어 발생하던 문제

→ 하드웨어적으로 Test & modify를 atomic하게 수행할 수 있도록 지원하는 경우 앞의 문제 해결 가능!


Test_and_set(a) : a라는  데이터의 현재값을 읽어내고 a의 값을 1로 바꿔주는 걸 하나의 instruction으로 수행

ex. a = 1인경우, 1을 읽고 그 값을 다시 1로 세팅 → 읽기와 세팅의 작업을 Test_and_set이 한번에 가능하게 해줌

Synchronization variable:
    boolean lock = false;  # lock을 1로 걸고 들어감, lock이 걸려있는지 체크, 안걸려 있다면 내가 lock을 걸고 작업 실행
Process Pi
    do {
        while (﻿Test_and_Set(lock));  # lock이 0이라면 아무도 없는 경우로 내가 lock을 걸고 들어감
	    critical section
	    lock = false; 
	    remainder section
       } 
​

Process Synchronization 2

Semaphores

앞의 방식들을 추상화시킴

공유자원을 획득하고 반납하는 역할

​

Semaphore S

2가지 atomic연산에 의해 접근

P연산 : semaphore변수 값 획득 

while (S <= 0) # S가 0이하인 경우에 while문 실행, 기다림 -> 양수가 되면
S--;
ex. S = 5, P연산 한번하면 자원을 가져가 4로 세팅 → 3 → 2 → 1 : 총 5번 수행

→ busy wait문제 발생

​

2. V연산 : 사용 후 반납하는 과정

ex. S = 4, V연산 수행 후 5로 세팅 → 원래 상태로 돌아옴

​

Critical Section + Semaphore

do {
	P(mutex);  # 들어갈때 P연산
	critical section
	V(mutex);  # 나올때 V연산
	remainder section
} while (1);
→ busy-wait(= spin lock) : lock을 얻지 못하면 splin(회전)하면서 lock을 얻길 기다림

→ Block & Wakeup (=Sleep lock) : lock을 얻지 못하면 그 프로세스는 block처리

Block / Wakeup Implemention

Semaphore 정의

typedef struct{
	int value;      # semaphore 변수값
	struct process *L;	 # semaphore에 의해 멈춰있는 프로세스들을 연결하기 위한 큐
} semaphore;
block 

커널은 block을 호출한 프로세스를 suspend시킴

이 프로세스의 PCB를 semaphore에 대한 wait queue에 넣음

wakeup(P)

block된 프로세스 P를 wakeup시킴

이 프로세스의 PCB를 ready queue로 옮김


→ 누군가가 semaphore를 쓰고 나서 반납하면 block된 프로세스 중 하나를 깨워서 wakeup 사용

Implementation

Semaphore연산의 정의

P(S) : 자원을 획득하는 과정

S.value--;     # 자원에 여분이 있다면 바로 획득
if(S.value < 0) # 없다면
{
	add this process to S.L;
	block(); # 잠들기
}
V(S) : 자원을 다 쓰고 반납

자원을 기다리면서 잠들어 있는 프로세스가 있다면 깨워주기

S.value++;
if(S.value <= 0)
{
	remove a process P from S.L;
	wakeup(P);
}
어느 것을 쓸것인가요?

​

Busy-wait  vs  Block/Wakeup

→  Block/Wakeup이 더 효율적 (CPU이용률 관점)

​

Block/wakeup overhead vs Critical section 길이

→ Critical section의 길이가 긴 경우 Block/Wakeup이 적당

→ Critical section의 길이가 매우 짧은 경우

 Block/Wakeup 오버헤드가 busy-wait 오버헤드보다 더 커질 수 있음

→ 일반적으로는  Block/Wakeup방식이 더 좋음

Semaphore의 2가지 타입

​

1. Counting semaphore

   도메인이 0이상인 임의의 정수값

   주로 resource counting에 사용

​

2. Binary semaphore (=mutex)

   0 또는 1 값만 가질 수 있는 semaphore

   주로 mutual exclusion (lock/unlock)에 사용

Deadlock and Starvation

Deadlock

둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상

​

ex. S와 Q가 1로 초기화된 semaphore라 하면

P0 → P(S) → P(Q) → ...... → V(S) → V(Q)

P1 → P(Q) → P(S) → ...... → V(Q) → V(S)

​

P0가 cpu를 얻어 S라는 semaphore를 운영 중 CPU를 P1에 뺏겨 Q의 semaphore를 획득

P1이 S를 획득하려고 했더니 P0에서 운영중이기 때문에 P1이 기다리게 됨 

→ 이 기다림은 P0가 S를 다 쓰고 반환할때까지!

Starvation

indefinite blocking. 프로세스가 suspend된 이유에 해당하는 세마포어 큐에서 빠져나갈 수 없는 현상

(특정 프로세스들만 자원을 공유하면서 다른 프로세스들은 자기 차례가 오지 못하도록 하는 경우)

​

해결 방법

자원의 획득 순서를 맞춰줌

