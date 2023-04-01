오늘 주제는 프로세스 동기화 문제이다.
동기화? 문제가 뭘까???

## 프로세스 동기화 문제
👉 공유 데이터에 동시에 접근하는경우, 문제가 발생한다.

## 해결 방법
👉 entry section을 사용하여 해결할 수 있다!

### entry section
공유 데이터에 접근하기 전에 lock을 걸어 동시 접근을 제어한다.
그렇다면 이 entry section은 어디에 있을까?

### 프로세스 구조
🔻 프로세스는 다음과 같은 구조로 이루어져있고, 그 안에 entry section이 존재한다.

```
do{
	entry section
    critical section
    exit section
    remainder section
} while(1);
```

하지만 위와 같은 구조를 가졌다고 프로세스의 모든 부분이 독립적인 것은 아니다.
프로세스들은 수행의 동기화를 위해 몇몇 변수를 공유한다. 이를 synchronization variable이라고 한다.

---
## 문제 해결 충족 조건
### 1. Mutual Exclusion (상호 배제)
한 프로세스가 critical section부분을 수행 중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안된다.

### 2. Progress
아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있으면 들어가게 해주어야 한다.

### 3. Bounded Waiting (유한 대기)
프로세스가 critical section에 들어가려고 요청한 후 부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다.

---
위에서 '공유데이터에 접근하기전에 lock을 건다'고 했는데, **lock**을 어떻게 걸까?

이에 대한 세 가지 알고리즘이 있다.

---
🔻 소프트웨어적으로 lock/unlock하는 알고리즘
## Algorithm 01

### 1. synchronization variable
```
int turn;
initially turn = 0; # (if turn == 1: p can enter its critical section)
```

### 2. process p
```
do {
	while (turn != 0);
	critical section
	turn = 1;
	remainder section
} while (1);

```

🚫 1번 알고리즘은 충족 조건 중 상호 배제는 만족하지만 **progress를 만족하지 못함**
 👉 **process 를 만족하지 못함**의 뜻: critical section에 아무도 없게됨
 👉 왜 이런 상황이 발생해? 서로 너무 양보만 해서

## Algorithm 02
### 1. synchronization variable
```
boolean flag[2];
initially flag[모두] = false; # (true일 때 critical section에 입장 가능)
```

### 2. process p
```
do {
	flag[i] = true; # 1. 본인(i)이 critical section에 들어간다! 는 의사표현
    while (flag[j]);  # 2. 엥근데 상대(j)가 이미 들어가있으면(true이면)
    critical section
	flag[i] = false;  # 3. 본인(i)은 좀 대기
} while (1);
```

🚫 마찬가지로, 충족 조건 중 상호 배제는 만족하지만 **progress를 만족하지 못하는 상황이 발생 가능**

## Algorithm 03(Peterson's Al.)
### process p
```
do {
	flag[i] = true;   # 1. 본인(i) = 들어간다~
    turn = j;         # 2. turn을 상대방에게 줌
    while (flag[j] && turn == j);  # 3. 근데 만약 상대가 기다리고있고 다음이 상대방 차례인 경우 -> 본인은 기다림
	critical section
	flag[i] = false;  # 4. critical section을 나올 때 본인의 flag를 false로 바꿔 상대방이 들어갈 수 있게 함
	remainder section
} while (1);
```

⭕ 세 가지 조건 만족 :)

근데 이 알고리즘에도 문제가 있다.
### Busy waiting(= spin lock)
- 계속 lock을 걸어 상대방이 못들어가게 막음
- 계속 CPU와 memory를 사용하며 기다려서 비효율적! (while문에서 계속 사용)

---
🔻위 문제를 하드웨어적으로 간단하게 해결할 수 있다.

## Synchronization Hardware
- Test & modify를 atomic하게 수행할 수 있도록 지원

이게 무슨말이냐하면~

### Test_and_set(a)
1. Test_and_set이라는 함수를 만든다. 

2. a라는 변수를 넣어 데이터의 현재값을 읽어내고, 그 a의 값을 1(true)로 바꿔주는 걸 하나의 instruction으로 수행한다.

3. 만약 a = 1인경우, 1을 읽고 그 값을 다시 1로 세팅 

👉 읽기와 세팅의 작업을 Test_and_set이 한번에 가능하게 해줌

### 코드
#### Synchronization variable
```
boolean lock = false;  
# lock이 걸려있는지 체크
# 만약 걸려 있지 않다면 본인이 lock을 걸고 작업 실행
```

#### Process Pi
```
do {
    while (Test_and_Set(lock));  
	critical section
	lock = false; 
	remainder section
} 
```