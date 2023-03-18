> 프로세스가 어떻게 만들어지는가!

## 프로세스 생성(Copy on Write(COW))
1. 부모 프로세스가 자식 프로세스를 생성
	- 1단계) 자식이 부모의 주소공간 복사
    	🔺 부모가 직접 생성이 아닌 운영체제에게 부탁 = fork() 시스템 콜
    - 2단계) 복제된 공간에 새로운 프로그램을 올림 = exec() 시스템 콜
    
	👉 생성 과정에서 프로세스의 트리(계층 구조) 형성

2. 프로세스의 실행을 위해서 자원이 필요
   - 운영체제로부터 받음
   - 부모와 공유
    	: 원칙적으로, 프로세스끼리는 서로 경쟁관계이기 때문에 공유하지 않지만, 가능하다면 공유하는 것이 효율적이다.
        - 모든 자원 공유
        - 일부 공유
        - 공유하지 않음
        
3. 수행(Execution)
    - 공존하며 수행
    - 자식이 종료(terminate)될 때까지 부모가 기다림(wait(blocked))
    
4. 종료
	- 자발적 종료(system call = exit)
	: 자식이 먼저 종료된 후에 부모가 종료됨
    	- 1) 자식이 부모에게 output data를 보냄
    	- 2) 프로세스의 자원이 반납됨
        .
    - 비자발적 종료(system call = abort): 강제종료
    	- 자식이 할당 자원의 한계치를 넘을 때
    	- 자식이 필요하지 않을 때
        -키보드로 kill, break 등을 친 경우
    	- 부모가 종료해야할 때 자식먼저 종료시킴
---
### 1. 생성 - fork() 시스템 콜
프로세스를 만들어달라는 요청에 대한 fork의 코드이다.

1. 기존 부모 코드
```
int main(){
	int pid;
}
```

2. 부모가 fork 요청
```
int main(){
	int pid;
    pid = fork();
}
```
부모(pid)가 fork를 요청하면 위 함수와 똑같은 코드가 새로 생성된다.

📍주의점
부모와 자식이 같은 코드를 가지게 되었지만, 부모는 처음부터 코드가 실행되고 자식은 fork() 이후부터 실행된다는 차이가 있다.

3. 부모, 자식 구분
부모와 자식을 구분하기 위해 pid 값에 따라 다른 결과를 출력한다.
```
int main(){
	int pid;
    pid = fork();
    if(pid == 0)
    	print("hello, l am child")
    else if (pid > 0)
    	print("hello, l am parent")
}
```

---
### 3. 수행 - exec() 시스템 콜

exec 시스템 콜은 복제된 프로세스가 완전히 새로운 프로세스가 되도록 한다. 복제된 fork 코드 중 **자식** 프로세스 부분에서 사용된다.

```
int main(){
	int pid;
    pid = fork();
    if(pid == 0){
    	print("hello, l am child")
        👉execlp("/bin/date", "bin/date",(char*)0);
    }
    else if (pid > 0)
    	print("hello, l am parent")
}
```
위와 같은 exec() 코드가 입력되면, 복제된 내용이 모두 지워지고 함수 안에 작성된 코드에 대한 기능을 수행하게 된다. 즉, 위 코드는 부모를 복제한 자식에게 새로운 기능을 덮어씌우는 코드이다. 

💻[코드]
```
execlp("/bin/date", "bin/date",(char*)0);
#execlp("원하는 기능", "원하는 기능","널포인터")
```
작성된 exec() 함수는 현재 날짜를 출력하는 기능을 수행한다.
exec() 이후의 코드는 실행되지 않는다!

---
### 3. 수행 - wait() 시스템 콜
프로세스를 잠들게(blocked 상태) 한다.
보통 자식 프로세스를 생성한 후 입력되는 코드로, 자식 프로세스가 종료될 때까지 부모 프로세스를 sleep시킨다. exec()과는 다르게, 복제된 fork 코드 중 **부모** 프로세스 부분에서 사용된다.

```
int main(){
	int pid;
    pid = fork();
    if(pid == 0)
    	print("hello, l am child")
    else if (pid > 0){
    	print("hello, l am parent")
        wait();
    }
}
```
---
### 4. 종료 - exit() 시스템 콜
exit() 시스템 콜은 종료하고싶은 부분에서 실행된다. exit() 시스템 콜을 넣어주지 않아도, 프로그램 내에서 종료가 필요한 부분에 넣어주게 된다.

보통, 자발적 종료 시에 실행된다.

---

## 프로세스 간 협력

🔻 원칙적으로 프로세스는 독립적!!
🔻 하지만 경우에 따라 협력을 해야할 때도 있다.

### 프로세스 간 협력 메커니즘(IPC: Interprocess Communication)
프로세스 간 정보를 주고받을 수 있는 것을 IPC라고 한다.
IPC는 두 가지 방법이 있다.

- 메시지 전달(message passing): **커널**을 통해 프로세스 a, b 간 메시지를 주고받음
    - Direct communication: 전달받는 프로세스의 이름 명시O
    - Indirect communication: 전달받는 프로세스의 이름 명시X

- 주소 공간 공유(shared memory): **커널**에게 공유를 요청한 후 서로 다른 프로세스 간 일부 공간을 공유

---
## CPU Scheduling

### 필요성
프로그램이 실행될 때, CPU burst와 I/O burst가 번갈아서 사용된다.
이 때, 두 버스트의 사용량에 따라 프로그램의 종류가 나뉜다.

- I/O bound job: CPU에 비해 I/O의 빈도 수가 많은 프로그램(보통 더 빈번!)
- CPU bound job: I/O에 비해 CPU의 빈도 수가 많은 프로그램

💡컴퓨터 안에는 여러 종류의 bound job이 섞겨있고 이를 관리해야하기 때문에 CPU Scheduling이 필요하다.
- 보통, CPU보다는 **사용자와 상호작용해야하는 I/O bound job** 때문에 스케줄링이 이루어짐
- I/O가 너무 오래사용되는 것을 막아 시스템 자원을 골고루! 효율적으로! 사용

### 프로세스의 분류
프로세스는 job의 사용량에 따라 분류된다.
- I/O bound process
: 짧은 CPU 버스트 사용시간

- CPU bound process
: 계산 위주의 job으로, 긴 CPU 버스트 사용시간

### 기능
#### 1. CPU Scheduler
ready 상태의 프로세스 중 어떤 프로세스에게 CPU를 줄지 선택한다.

#### 2. Dispatcher
CPU의 제어권을 선택된 프로세스에게 넘긴다. 👉 context switch(문맥 교환)

### 스케줄링이 필요한 경우
- ⭐**nonpreemptive[비선점성]**⭐(CPU 자진 반납)
    - Running -> Blocked (ex. I/O를 요청하는 시스템 콜이 들어올 때)
    - Terminate (프로세스가 종료될 때)

- ⭐**preemptive[선점성]**⭐(CPU 강제로 빼앗음)
    - Running -> Ready (ex. 할당된 시간이 만료되었을 때)
    - Blocked -> Ready (ex. 요청된 I/O 완료 후 더 중요한 프로세스에게 CPU 할당)
    