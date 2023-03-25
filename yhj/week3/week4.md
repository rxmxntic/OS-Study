# Thread
## 정의 
- 프로세스 내부에 CPU 수행 단위가 여러 개 있는 경우
- CPU의 구성 단위

## 구성 요소
### **[독립적 구성요소]**
- Program Counter
- Register Set
- Stack Space

### **[공유 구성요소(task)]**
👉 하나의 프로세스에 thread가 여러 개 있을 경우, 동료 thread와 공유하는 부분
- code selection
- data section
- OS resources

### **[비교(light vs heavy)]**
🐰 하나의 프로세스에 여러개의 thread가 있는 것이 훨씬 가벼우며, 이를 **lightweight process**라고 부른다.

🐻 반면, 전통적인 프로세스는 thread를 하나만 사용하며 **heavyweight process**라고 한다.

## Lightweight Process
앞서 말한 lightweight process에 대해 더 살펴보자!

운영체제는 프로세스마다 하나의 **PCB가 생성**되어 관리된다.
🔻
이 프로세스 안에 스레드가 여러개라면
🔻
CPU마다 정보가 복사된 **프로그램 카운터나 레지스터가 생성**된다.
🔻
즉, 각 **cpu 관련 정보가 별도로 담긴 thread**가 여러개 생기는 것이다.

 👉 **Lightweight Process**


## 장점
### 1. 응답성(Responsiveness)
- 다중 스레드의 경우, 하나의 스레드가 blocked(waiting) 상태여도 다른 스래드가 실행되어 빠른 처리가 가능하다.

### 2. 자원 공유(Resource Sharing)
- 같은 일을 하는 프로세스들을 관리해야 하는 경우, 프로세스 하나에 다수의 cpu를 두어 thread들이 각종 자원을 공유할 수 있게 함.

### 3. 경제성(Economy)
- 속도가 빠르다는 의미이나, 응답성(1)과는 다르다. 
- 적은 노력을 가지고 높은 성능을 나타낼 수 있다는 의미이다. (또는 재사용 가능)

### 4. 멀티프로세스 구조의 활용(Utilization of MP Architectures)
📍 위의 세 가지는 단일 cpu에서도 해당했지만 이번 장점은 다중 cpu 환경에만 해당
- 병렬 처리 가능

## 운영 방법
thread의 운영 방법은 아래 세 가지 방식이 있다.

### 1. Kernel Threads
- 커널 형태로 지원
- thread가 여러개 있다는 사실을 운영체제가 앎
- 하나의 스레드에서 다른 스레드로 cpu가 넘어가는 것을 커널이 스케줄링 하듯 넘겨줌 

### 2. User Threads
- 라이브러리 형태로 지원
- thread가 여러개 있다는 사실을 운영체제가 모름
- 커널 대신 user program이 알아서 관리

### 3. real-time Threads