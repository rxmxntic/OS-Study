Process Synchronization 3,4

Sunchronization의 고전적 문제 3가지

Bounded-Buffer Problem (Producer-Consumer Problem)

 Readers and Writers Problem 

 Dinig-Philosophers Problem

Bounded-Buffer Problem (Producer-Consumer Problem)

: 버퍼의 크기가 유한한 경우

buffer : 임시로 데이터를 저장하는 공간


Producer의 역할 : 공유 버퍼에 데이터를 만들어서 넣는 역할

Consumer 역할 : 버퍼에 있는 일을 생산된 순서대로 한개씩 처리

​

발생 가능한 문제

생산자 2개가 동시에 도착해서 비어있는 같은 위치의 자원을 보고 데이터를 집어넣게 되면서 동기화 문제 발생 가능

생산자가 비어있는 버퍼를 확인하고 데이터를 집어 넣기 전, 버퍼에 lock → 데어터 넣기 → 비어있는 버퍼의 포인터를 하나 증가 → 버퍼에 unlock

소비자가 동시에 꺼내가면 문제가 발생가힌깐 같이 자원을 사용하고 싶다면 버퍼에 lock을 걸어 데이터를 꺼낸 후 unlock

공유 버퍼가 가득 차 있는 상태에서 더 이상 데이터를 넣지 못하는 상황이지만 생산자가 또 도착해서 데이터를 넣고 싶은 상황, 생산자 입장에서는 사용할 수 이는 자원이 없는 상태

세마포어가 자원의 갯수를 counting하는 목적으로 사용

버퍼의 개수가 n개라고 하면 생산자가 도착해서 버퍼 n개를 가득채운 후, 그 다음 생산자가 도착하더라고 데이터를 넣을 수 있는 비어있는 버퍼가 없다.

비어있는 버퍼에 넣을 수 있는 타이밍은?

소비자가 내용을 꺼낼 때 버퍼에 데이터 넣기 가능

​

세바포어가 해야할 일

1. 동시에 공유 데이터의 접근을 막기 위한 lock, unlock고정

2. 버퍼가 가득 차거나 비었을 때 가용자원의 수를 세는 counting 역할

​

shared data : 공유 버퍼 자체가 공유 데이터

buffer 자체 및 buffer 조작 변수 (empty/full buffer의 시작 위치)

​

Synchronization variables​​ : lock을 걸고 푸는 용도 & 자원의 수를 counting하는 용도

mutual exclusion → Need binary semaphore (shared data의 mutual exclusion을 위해)

Resource count → Need integer semaphore (남은 full/empty buffer의 수 표시)

​

변수

full : 내용이 들어있는 버퍼를 세기 위한 변수

empty : 비어있는 버퍼의 개수

mutex : lock을 걸기 위한 변수


아이템 x를 다음 버퍼에 넣기 전에 P로 비어있는 버퍼가 있는지 확인 → empty로 비어있다면 버퍼 전체에 lock을 걸고 버퍼에 데이터 넣기 → 버퍼의 조작이 끝났으면 lock을 풀고 V인 full로 소비자 자원 1 증가

→ 생산자와 소비자는 대칭되는 과정으로 반대로 진행

 Readers and Writers Problem 

한 process가 DB에 write중일 때 다른 process가 접근하면 안됨, read는 동시에 여럿이 해도 됨

: Writer는 동시 접근시 문제가 생길 수 있는데 Read는 동시에 접근해도 문제가 되지 않기 때문이다.

​

공유 데이터를 DB라고 한다. 데이터를 읽는 프로세스와 쓰는 프로세스 2종류가 존재한다. Reader와 writer가 하나만 있는 상황이 아니라 여러 개 있는 상황이다. 공유데이터에 동시 접근을 하면 안되니깐, lock을 걸어 하나의 프로세스만 접근하게 하고 데이터를 다 쓰고 나오면 unlock을 해준다

​

shared data

DB 자체

readcount; : 현재 DB에 접근 중인 Reader의 수

​

Synchronization variables

mutex : 공유 변수 readcount를 접근하는 코드(critical section)의 mutual exclusion보장을 위해 사용

db : reader와 writer가 공유 DB 자체를 올바르게 접근하게 하는 역할

​

writer를 보면 P(db)연산을 통해 lock을 걸고 DB에 쓴 다음 다시 unlock을 해준다.


Writer를 보면 P(db)연산을 통해 lock을 걸고 DB를 쓴 다음 다시 unlock을 해준다.

​

Reader에서도 lock을 걸어야 한다. lock을 걸지 않으면 writer가 해당 데이터를 써버릴 수 있기 때문이다.

읽으려고 lock을 걸었으면 다른 Reader가 왔을 때에는 읽을 수 있게 해줘야 한다. 그래서 몇 명이 읽고 있는지에

 대한 readcount변수를 두고 최초의 reader가 읽으러 들어갈때 db에 lock을 건다. readcount가 1인 reader가 와서 이미 lock을 걸어줬기 때문에 최초의 reader가 아니라면 lock을 걸 필요가 없다.

​

readcount도 공유 데이터이기 때문에 여러 reader가 동시에 증가시킬 수 있기 때문에 readcount를 바꾸기 위해서도 mutex라는 세마포어 변수로 lock을 걸어줘야 한다. readcount가 0이면 DB를 마지막으로 읽고 나가는 reader이므로 DB에 건 lock을 풀어줘야 한다.

​

→ 이 코드 상으로는 writer가 너무 오래 기다릴 수 있기 때문에 starvation 발생 가능

​

해결책

writer가 DB에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기 중인 Reader들을 다 DB에 접근하게 해준다.

Writer는 대기 중인 Reader가 하나도 없을 때 DB 접근이 허용된다.

일단 Writer가 DB에 접근 중이면 Reader들은 접근이 금지된다.

Writer가 DB에서 빠져나가야만 Reader의 접근이 허용된다.

Dinig-Philosophers Problem

철학자 다섯명이 원탁에 앉아있다. 다섯 명의 철학자는 생각하는 일, 밥 먹는 일 2가지를 행한다.

배가 고차지면 자신의 왼쪽과 오른쪽에 있는 젓가락을 들고 밥을 먹으며, 다 먹고 나면 젓가락을 내려놓고 생각하는 일을 반복한다. 밥을 먹으려면 젓가락 2개를 모두 잡아야 하며 젓가락이 공유자원이다.


​

위 코드는 데드락의 위험! → 모든 철학자가 동시에 배가 고파져 왼쪽 젓가락을 집어버린 경우

​

만약 철학자 모두가 오른쪽의 젓가락을 든다고 하면 철학자 모두가 왼쪽 젓가락은 잡지 못하고 오른쪽 젓가락만 가지고 있기 때문에 식사를 하지 못한다.

철학자가 밥을 먹을 때에만 태이블에 앉게 하거나 젓가락을 양쪽 다 얻을 수 있는 상황에서만 젓가락을 잡을 수 있는 권한으로 해결할 수 있다. 

또는 비대칭의 방법으로 짝수 철학자는 왼쪽 젓가락만 잡도록 하고 홀수 철학자는 오른쪽 젓가락만 잡도록 한다.

​

해결방안

4명의 철학자만이 테이블에 동시에 앉을 수 있도록 한다.

젓가락을 두개 모두 집을 수 있을 때에만 젓가락을 잡을 수 있게 한다.

비대칭(짝수나 홀수 철학자는 왼쪽 혹은 오른쪽 젓가락부터 잡도록)

​

​


다섯명의 철학자가 하는 일은 먹거나 생각하는 일

먹기 전에는 Pickup(), 먹은 후에는 Putdown()

​

세마포어 변수 self[5] : 각각 다섯명의 철학자가 젓가락을 양쪽 다 잡을수 있는지 없는지에 따라 권한을 주는 변수

self[5] 는 철학자의 상태를 나타내며 생각하는 상태, 먹는 상태, 배고픈 상태로 나눈다. mutex라는 Lock은 공유 데이터의 접근을 막기 위한 변수이다.

​

젓가락을 잡는 함수를 호출하게 되면 자신의 상태를 변경하기 전에 다른 철학자가 상태를 변경할 수 없게끔 lock을 먼저 걸고 배고픈 상태로 바꾼 후 test()함수로 간다. test함수는 양쪽 철학자가 밥을 먹지 않고 내가 배고픈 상태를 만족할 상태일때 젓가락을 잡을 권한을 얻게 된다.

Monitor

Semaphore의 문제점 

코딩하기 힘들다

정확성(correctness)의 입증이 어렵다

자발적 협력(voluntary cooperation)이 필요하다

한번의 실수가 모든 시스템에 치명적 영향

→ 세마포어를 사용하더라도 불편한점이 있어 모니터 제공

​


동시 수행중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한 high-level synchronization construct

​

monitor

모티너 내에서는 한번에 하나의 프로세스만이 활동 가능

프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요없음

프로세스가 모니터 안에서 기다릴 수 있도록 하기 위해 condition variable 사용 → condition x, y;

Condition variable은 wait와 signal연산에 의해서만 접근 가능 

x.wait() : x.wait()을 invoke한 프로세스는 다른 프로세스가 x.signal()을 invoke하기 전까지 suspend된다.

x.signal() : 정확하게 하나의 suspend된 프로세스를 resume한다.

                               suspend된 프로세스가 없으면 아무일도 일어나지 않는다. 잠들어 있는 프로세스 깨우기

​


세마포어는 생산자가 버퍼에 데이터를 넣기 위해서 버퍼 전체에다 lock을 걸어 다른 생산자가 접근하지 못하게 한다. 그 후 버퍼에 데이터를 넣고 lock을 풀어준다. Monitor에서는 생산자든 소비자든 들어와 코드를 실행하는 중에 다른 프로세스의 접근을 모니터가 막아주기 때문에 굳이 공유 버퍼에 대해 lock을 걸거나 푸는 코드는 필요하지 않다. 생산자인 경우에는 버퍼가 비어있다면 빈 버퍼를 기다리는 줄에 가서 sleep상태로 있게 한다.

​


full은 내용이 들어있는 버퍼를 기다리는 프로세스들의 줄

empty는 빈 버퍼를 기다리는 프로세스들 줄을 세워놓는 condition variable이다.

​

condition variable은 값을 가지지 않고 자신의 큐에 프로세스에 sleep시키거나 큐에서 프로세스를 깨우는 역할만 한다.

​

공유 버퍼에 자원을 넣는 역할이 생산자 프로세스, 공유 버퍼에서 데이터를 꺼내가는 역할은 소비자 프로세스

생산자가 버퍼에 데이터를 넣기 위해서 전처 버퍼에 lock을 걸어 다른 생산자와 소비자가 접근하지 못하게 한 후 만들어진 데이터를 버퍼에 넣고 소비자는 데이터를 꺼내기 위해 버퍼 전체에 lock을 걸어 다른 프로세스의 접근을 막고 데이터 꺼내간다.


식사하는 철학자의 monitor버전

​