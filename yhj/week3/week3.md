> ⭐Process = 사용중인 프로그램⭐

## 1. 프로세스의 문맥(context)
### 의미
- 현재 상태를 나타냄
- 프로세스가 어디까지 수행했고, 어느 주소공간에 있는지 알려줌

### 구분
- CPU의 수행 상태를 나타내는 하드웨어 문맥
   - Program Counter
   - register

- 프로세스의 주소 공간
    - code
    - data
    - stack
    
- 프로세스 관련 커널 자료구조 : 커널 상태에 따라 수행 조절
    - PCB(Process Control Block)
    - Kernel Stack
    
---
## 2. 프로세스의 상태
프로세스는 상태를 변경해가며 프로그램을 수행한다. 
그 상태는 아래와 같다.

#### 🌞Active
- **New**
프로세스가 생성중인 상태

- **Running**
CPU를 잡고 instruction을 수행중인 상태

- **Ready**
다른 조건들을 다 만족하고 CPU를 얻으려 기다리는 상태

*보통 running과 ready 상태를 번갈아가며 CPU를 사용한다

- **Blocked(wait, sleep)**
    CPU를 얻어도 당장 instruction을 수행할 수 없는 상태
    - Process 스스로가 요청한 이벤트(ex. I/O)가 즉시 만족되지 않아 기다리는 상태
    	- ex. 디스트에서 file 읽어오기
- **Suspended(stopped)**
외부에서 프로세스를 정지한 상태(CPU를 강제로 뺏음) -> swap out

📍 _Blocked VS Suspended_
Blocked: 스스로가 문제(자신이 요청한 이벤트가 완료되면 ready)
Suspended: 남이 문제(외부에서 재개해줘야 active)

---
#### 🌚Inactive
- **Terminated**
수행을 끝내고 정리중인 상태



--- 
## 3. PCB
앞서 1절 프로세스 관련 커널구조에 소개되었던 PCB에 대해 자세히 알아보자.
### 의미
운영체제가 각 프로세스를 관리하기 위해 유지하는 정보

### 구성요소

1) OS가 사용하는 정보: 관리 용도
- process state, process ID
- scheduling information, priority

2) CPU 수행 관련 하드웨어 값
- program counter, registers

3) 메모리 관련: 위치 정보 제공
- code, data, stack

4) 파일 관련 
open file descriptors...

---
## 4. 문맥 교환
CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정

### 과정
1. A 프로세스의 문맥 저장

2. B 프로세스로 CPU 넘겨줌

3. 다시 A로 돌아올 때 저장된 문맥 부분에다가 반환
---

## 5. 큐
큐는 아래와 같이 나뉘고, ready와 device 큐 중 하나에만 해당할 수 있다.
- job queue
    - ready queue
    - device queue
   
---
  
## 6. 스케줄러
- Long-term scheduler
    - 메모리를 어떤 프로세스에 줄지 결정
    - 메모리에 올라온 여러 프로세스들(degree of multiprogramming)을 제어

- Short-term scheduler
    - 짧은 시간 단위로 스케줄이 이루어짐
    - 다음번에 CPU를 어떤 프로세스에 줄지 결정
    
- Medium-term scheduler
    - degree of multiprogramming을 제어
    - 너무많은 프로세스가 있다면 여유공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄