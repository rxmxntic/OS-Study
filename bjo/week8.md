글을 쓰는 목적 

🔎 Process Synchronization이란 무엇이고, 임계영역은 무엇인가, 임계영역문제는 어떻게 해결하는가에 대해서

알아보기 위해

 

Process Synchronization(프로세스 동기화)
두 개 이상의 프로세스가 공유된 데이터에 동시 접근하면 이 공유된 데이터를 신뢰할 수 없는데

프로세스 사이에서 실행 순서를 지켜 자원의 일관성을 보장하는 매커니즘을

Process Synchroniztion이라고 합니다.

 

Critical Section(임계 영역)
멀티 스레딩에 문제점에서 나오듯, 동일한 자원을 동시에 접근하는 작업 (ex. 공유하는 변수사용

동일 파일을 사용하는 등)을 실행하는 코드 영역을 Critical Section이라 칭한다.

문제는 이렇게 공유하는 영역에 두 개 이상의 프로세스 또는 쓰레드가 접근해서 작업을 하면

의도하지 않은 상황들이 발생할 수 있게 된다. 이는 굉장히 치명적이기 때문에,

임계 영역을 구현하기 위해선 아래와 같은 3가지 조건을 충족해야 합니다.

Mutual Exclusion(상호 배제)
프로세스 P1이 Critical Section에서 실행 중이라면 다른 프로세스들은 그들이 가진 Critical Section에서 실행 될 수 없다.
Progress(진행)
Critical Section에서 실행중인 프로세스가 없고, 별도의 동작이 없는 프로세스들만 Critical Section 진입 후보로서 참여될 수 있다.
Bounded Waiting(한정된 대기)
P1가 Critical Section에 진입 신청 후 부터 받아들여질 때까지, 다른 프로세스들이 Critical Section에 진입하는 횟수는 제한이 있어야 한다.
해결책
Mutex Lock

동시에 공유 자원에 접근하는 것을 막기 위해 Critical Section에 진입하는 프로세스는 Lock을 획득하고 Critical Section을 빠져나올 때, Lock을 방출함으로써 동시에 접근이 되지 않도록 한다.
한계

다중처리기 환경에서는 시간적인 효율성 측면에서 적용할 수 없다.
Semaphores(세마포)
소프트웨어상에서 Critical Section문제를 해결하기 위한 동기화 도구
종류

OS는 Counting/Binary 세마포를 구분

카운팅 세마포
가용한 개수를 가진 자원에 대한 접근 제어용으로 사용되며, 세마포는 그 가용한 자원의 개수로 초기화 된다. 자원을 사용하면 세마포가 감소, 방출하면 세마포가 증가 한다.
이진 세마포
MUTEX라고도 부르며, 상호배제의(Mutual Exclusion)의 머릿글자를 따서 만들어졌다. 이름 그대로 0과 1사이의 값만 가능하며, 다중 프로세스들 사이의 Critical Section문제를 해결하기 위해 사용한다.
단점

Busy Waiting(바쁜 대기)
Spin lock이라고 불리는 semaphore초기 버전에서 Critical Section에 진입해야하는 프로세스는 진입 코드를 계속 반복 실행해야 하며, CPU시간을 낭비 했었다. 이를 Busy Waiting이라고 부르며 특수한 상황이 아니면 비효율적이다. 일반적으로 Semaphore에서 Critical Section에 진입을 시도했지만 실패한 프로세스에 대해 Block시킨 뒤, Critical Section에 자리가 날 때 다시 깨우는 방식을 사용한다. 이 경우 Busy waiting으로 인한 시간낭비 문제가 해결된다.
Deadlock(교착상태)

세마포가 Ready Queue를 가지고 있고, 둘 이상의 프로세스가 Critical Section 진입을 무한정 기다리고 있고, Critical Section에서 실행되는 프로세스는 진입 대기 중인 프로세스가 실행되야만 빠져나올 수 있는 상황을 지칭한다.