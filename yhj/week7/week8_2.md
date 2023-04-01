🔻 사전 지식
### 추상 자료형
- 자료구조에서 등장
- object와 operation으로 구성
- 컴퓨터에서 실제로 형태를 구현하기보다는 논리적으로 정의된 자료형

---
# Semaphores
오늘 배울 세마포어가 이 추상 자료형에 속한다.

## 사용 경우
- lock을 걸 때
- 공유자원의 획득과 반납 처리

## 연산
### P 연산
- 공유자원을 획득하는 과정
- lock을 걺

### V 연산
- 공유자원을 다 사용하고 반납하는 과정
- lock을 풂

---
## Critical Section에서의 활용
### 1. Busy-wait 방식
👉 lock을 얻지 못하면 회전하면서 lock을 얻길 기다림

#### 1) Synchronization variable
```
semaphore mutex;
```

#### 2) process p
```
do {
	P(mutex);
	critical section
	V(mutex);
	remainder section
} while (1);
```

### 2. block & wakeup (=sleep lock)
👉 lock을 얻지 못하면 해당 프로세스 block 처리

#### P(S)
- 자원을 획득하는 과정

```
S.value--; # 자원에 여분이 있으면 획득
if(S.value < 0) # 없으면
{
	add this process to S.L;
	block(); # block
}
```

#### V(S)
- 자원을 반납하는 과정

```
S.value++;
if(S.value <= 0) # 자원을 기다리면서 잠들어 있는 프로세스가 있으면
{
	remove a process P from S.L;
	wakeup(P); # 깨워주기
}
```

### 비교
- 보통, block & wakeup 방법이 더 효율적
- 하지만 critical section의 길이가 짧다면 Busy-wait이 더 적합

---
## Semaphore 종류
### 1. Binary semaphore (=mutex)
- 0 또는 1 값만 가지는 semaphore
- 주로 mutual exclusion (lock/unlock)에 사용

### 2. Counting semaphore
- 도메인이 0이상인 임의의 정수값
- 주로 resource counting에 사용
