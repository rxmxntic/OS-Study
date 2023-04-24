# Scheduling Algorithm

✅우선순위에 따른 스케줄링 방식으로 multilevel queue와 Multilevel Feedback Queue가 있다.

## Multilevel Queue
- 아래와 같은 우선순위에 따라 CPU 할당
- 철저한 계급제 👉 높은 순위가 무조건 먼저 CPU 사용가능!

![](https://velog.velcdn.com/images/ofohj/post/d72d16b9-b503-4e2f-b932-5b22c62657a5/image.png)

1. Ready queue를 여러 개로 분할

2. 각 큐는 독립적인 스케줄링 알고리즘을 가짐

3. 큐에 대한 스케줄링
	- 우선순위 고정(강력한 우선순위제)
    	👉 단점: starvation
        👉 해결 방안: time slice(80%는 RR, 20%는 FCFS 적용 등의 방식으로 시간 분배)
        
## Multilevel Feedback Queue
- Multilevel queue의 신분극복불가 제도에 문제가 있다고 판단해 등장한
새로운 스케줄링 방법
- 우선순위가 존재하지만, 순위 변경 가능
- CPU 사용 시간이 짧은 시간 순으로 우선순위 설정

![](https://velog.velcdn.com/images/ofohj/post/e7b82211-a43e-4631-b5cf-07ad31defa78/image.png)

---
✅ CPU가 여러개인 경우의 스케줄링이다. 이 경우 스케줄링은 더욱 복잡해진다.

## Multi-Processor Scheduling
### Homogeneous processor
- Queue에 프로세스들을 한 줄로 세우고 각 프로세서가 알아서 꺼내가게 함
- 반드시  특정한 프로세서에서 수행되어야 하는 경우에는 문제가 더 복잡

### Load Sharing(Load Balancing)
👉 Load Sharing(Load Balancing)이 잘 되어야 함
- 특정 CPU만 일하고 나머지 CPU가 놀고 있지 않게 여러 CPU가 골고루 일을 해야함

- 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유

- 방법
    - 별개의 Queue를 두는 방법(각자 다른 방 사용)
    - 공동 Queue를 사용하는 방법(한 방을 사용하려고 한줄서기)

### Symmetric Multiprocessing (SMP)
- 모든 프로세서가 동등
- 각 프로세서가 알아서 스케줄링을 결정

### Asymmetric Multiprocessing (ASMP)
- 여러개의 CPU 중에서 하나의 CPU가 전체적인 컨트롤 담당
- 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서들은 거기에 따라야 한다.

---
## Real-Time Scheduling
- deadline을 제공하는 스케줄링

### Hard Real-Time Scheduling
- deadline 무조건 보장(지켜야함)되도록 스케줄링

### Soft Real-Time Scheduling
- deadline 조금 어겨도 됨
- 일반 프로세스에 비해 높은 우선순위를 얻음

---
## Thread Scheduling
### local Scheduling
- os가 아닌 사용자 프로세스가 어떤 thread를 스케줄할지 결정

### global Scheduling
- 운영체제가 thread의 스케줄링을 처리

---
---

# Algorithm Evaluation
알고리즘 평가 방법은 다음과 같다.

## Queueing  models
- 자주 사용되진 않지만 가장 이론적인 평가 방법
- 확률 분포로 주어지는 Arrival rate와 Service rate를 통해 처리량, 대기 시간 등의 performance index 계산

## Implementation(구현) & Measurement(성능 측정)
- 실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대한 성능 측정 및 비교
- 더 빨리 끝나는 시스템이 성능이 더 좋다고 판단!

## Simulation (모의 실험)
- Implementation(구현) & Measurement(성능 측정)과는 다르게 실제 실행x
- 모의 실험
- 알고리즘을 모의 프로그램으로 작성 후, **trace**을 입력으로 하여 결과를 비교하는 방식
📍 **trace**
: 실제 값들을 진짜 프로세스가 동작할 때와 같은 값들로 집어넣는 작업
