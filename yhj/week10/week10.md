# Deadllock 
- 교착상태
🔻예시 상황
![](https://velog.velcdn.com/images/ofohj/post/c8ef7175-4e23-41ab-9cda-a7515afc0335/image.png)
👉 누군가 양보(희생)하면 교착상태가 발생하지 않으나, 그렇지 않은 경우 발생
👉 컴퓨터 상에서의 경우, 프로세스가 가진 자원과 요구하는 자원이 둘 다 존재할 때 발생

---
## 개념
일련의 프로세스들이 서로가 가진 **자원**을 기다리며 잠들어 있는 상태

### 자원
#### 종류
- 하드웨어 자원
	ex. I/O device, CPU, RAM

- 소프트웨어 자원
	ex1. memory space: 주기억장치(RAM) 관리하고, 데이터나 코드 저장 및 액세스
    ex2. semaphore: 상호 배제 및 동기화를 위한 도구

#### 사용 절차
1. Request(요청)
2. Allocate(할당)
3. Use(사용)
4. Release(반납)

---
## Deadlock의 4가지 발생 조건
#### 1. Mutual exclusion(상호 배제)
: **하나의 프로세스**만 자원 사용이 가능하나, 그렇지 못한 경우

#### 2. No preemption(비선점)
: 프로세스는 자원을 강제로 빼앗기지 않고 **스스로 내놓기**때문에 발생 가능
👉 자원을 빼앗기는 특성이 있었다면, deadlock은 발생하지 X

#### 3. Hold and wait(보유대기)
: 자원을 가진 프로세스가 다른 자원을 기다릴 때 **보유 자원을 놓지 않고 계속 가지고** 있는 경우

#### 4. Circular wait(순환대기)
: 자원을 기다리는 프로세스간에 **사이클**이 형성되는 경우
ex.
📍프로세스 $P_0, P_1, ..., P_n$이 있을 때
$P_0$은 $P_1$이 가진 자원을 기다림
🔻
$P_1$은 $P_2$가 가진 자원을 기다림
🔻
$P$n-1은 $P_n$이 가진 자원을 기다림
🔻
$P_n$은 $P_0$가 가진 자원을 기다림

---
이러한 Deadlock의 발생 여부를 알아보기 위해!

## 자원 할당 그래프
Resource-Allocation Graph

### 그래프 구성
그래프는 다음과 같은 요소로 구성되어 있다.
![](https://velog.velcdn.com/images/ofohj/post/90098607-8b05-470e-8e64-e80ad631ef22/image.png)

그래서 이 그래프로 어떻게 deadlock 발생 여부를 알 수 있을까?

앞서 deadlock의 발생 조건 중 하나가 **사이클 형성**이였다.
그리고 위 그래프에서는 사이클이 발견되지 않는 것을 보아, 이는 deadlock 상태가 아니라고 할 수 있다.

다음으로 제시된 그래프를 살펴보자.
![](https://velog.velcdn.com/images/ofohj/post/4c24e7bd-1a42-42cc-8cff-331a9ec0fdd2/image.png)

위 그래프에는 사이클이 발견되므로, 두 개 모두 deadlock 상태라고 판단할 것이다.

✋ 하지만! 아니다!

여기서 알아야 할 것이 있다.
> 💡 **자원의 개수가 1개** 일 경우, 프로세스 A가 얻고자 하는 자원과 B가 이미 가지고 있는 자원이 겹칠 수 있어 deadlock 발생 가능성이 크다.
👉 그러나, **자원의 개수가 여러개**일 경우 그 가능성이 적어진다!


그리고 왼쪽 그래프의 경우, 자원이 여러개지만 이를 원하는 프로세스 또한 한 개 이상이기 때문에 deadlock 상태가 맞습니다.

하지만 오른쪽 그래프의 경우 각 자원 하나에 한 개의 프로세스가 매치될 수 있으므로, deadlock 상태가 아닙니다.

---
## Deadlock의 처리 방법

### deadlock을 미리 방지
#### 1. Deadlock Prevention
🪄 가장 강력한 방법!
- 개념
: Deadlock의 4가지 조건 중 하나가 발생하지 않도록 막아 Deadlock 방지

- 문제점
: 조건을 막는 과정에서 Utilization 저하, throughput 감소, starvation 문제 발생 가능
: 생길지 말지 모르는 Deadlock을 방지하는 것이 비효율적

cf.
📍 Utilization 저하
: 시스템의 자원을 사용하는 데 필요한 시간이 증가하여 자원 활용도가 감소하는 현상
📍 Throughput 감소
: 시스템이 처리하는 작업의 수나 처리 속도가 감소하는 현상
📍 Starvation 문제
: 우선순위가 낮은 작업이 자원을 할당받지 못하여 계속해서 대기하는 현상

#### 2. Deadlock Avoidance
- 개념
: 자원 요청에 대한 부가정보를 이용해 자원 할당으로 인한 deadlock 발생 가능성을 확인한 후, 문제가 없으면 할당
: 항상 안전한 상태를 유지해 deadlock을 방지

- 부가정보
: 현재에 당장 할당 받을 자원 외에도, 평생 할당 받아야 할 자원까지 고려한 정보
👉 자원이 한 개인 경우: 자원 할당 그래프 활용
👉 자원이 여러개인 경우: Banker's Algorithm 활용

### deadlock 발생 후 처리
#### 3. Deadlock Detection and Recovery
- 개념
: deadlock발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견 시 회복하도록 함

- detection
: 2번과 비슷하다.
👉 자원이 한 개인 경우 자원 할당 그래프 또는 wait for graph 활용
![](https://velog.velcdn.com/images/ofohj/post/f437d31c-0251-4953-aef6-ae2585c2bcca/image.png)

👉 여러개인 경우 뱅커스 알고리즘 사용

- recovery
    - process termination
    : deadlock에 관련된 프로세스들을 한꺼번에 또는 차례로 죽임
    - resource preemption
    : 안전한 상태로 돌아가 프로세스를 restart시킴
    : starvation 발생 가능

### deadlock 무시
#### 4. Deadlock Ignorance
- 개념
: deadlock을 무시해버리는 방법
💡 deadlock은 잘 발생하지 않는 문제이다. 따라서, 이를 항상 예방하기 위한 처리를 하는 것 보다 그냥 무시하는 것이 효율적이라 판단한 것이다.

- 방법
: deadlock이 발생한다면 시스템의 비정상적 작동을 사람이 느끼고 직접 프로세스를 죽임

---

출처: http://kocw.net/home/m/search/kemView.do?kemId=1046323