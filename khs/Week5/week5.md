- 프로세스 생성
    - 부모 프로세스가 자식 프로세스 생성
    - 프로세스의 트리(계층 구조)형성
    - 프로세스는 자원을 필요로 함
        - 운영체제로 부터 받는다
        - 부모와 공유한다
    - 자원의 공유
        - 부모와 자식이 모든 자원을 공유하는 모델
        - 일부를 공유하는 모델
        - 전혀 공유하지 않는 모델
    - 수행
        - 부모와 자식은 공존하며 수행되는 모델
        - 자식이 종료(terminate)될 때까지 부모가 기다리는(Wait) 모델
    - 주소 공간(Address space)
        - 자식은 부모의 공간을 복사함 (binary and OS Data)
        - 자식은 그 공간에 새로운 프로그램을 올림
    - 유닉스의 예
        - **fork()** 시스템 콜이 새로운 프로세스를 생성
            - 부모를 그대로 복사(OS data except PID + binary)
            - 주소 공간 할당
        - fork 다음에 이어지는 **exec()** 시스템 콜을 통해 새로운 프로그램을 메모리에 올림
    
- 프로세스 종료 (Process Termination)
    - 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려줌**(exit)**
        - 자식이 부모에게 output data를 보냄(via **wait**)
        - 프로세스의 각종 자원들이 운영체제에게 반납됨
    - 부모 프로세스가 자식의 수행을 종료시킴 (**abort)**
        - 자식이 할당 자원의 한계치를 넘어섬
        - 자식에게 할당된 태스크가 더 이상 필요하지 않음
        - 부모가 종료(exit)하는 경우
            - 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않는다
            - 단계적인 종료
- **fork() 시스템 콜**
    
    ![](https://velog.velcdn.com/images/losie2/post/48a618a1-70fa-41a4-a316-38fcd69f94f7/image.png)

    
    - **A process is created by the fork() system call**
        - creates a new address space that is a duplicate of the caller
        - 부모, 자식 프로세스가 하는 일이 다를 수 있다라는 설명
    
    ![](https://velog.velcdn.com/images/losie2/post/16605c5c-bd56-4475-9f1d-6bfbd6d4edbc/image.png)

    
    - 자식 프로세스는 가장 맨 위 printf를 출력하지 않고, 아래 자식 프로세스의 printf를 실행한다
- **exec() 시스템 콜**
    
    ![](https://velog.velcdn.com/images/losie2/post/2b0bd1e6-ad47-48fc-90a7-a0e474d1ff5d/image.png)

    
    - **A process can execute a different program by the exec() system call.**
        - replaces the memory image of the caller with a new program.
        - execlp를 실행한다면 이전의 프로그램으로 돌아올 수 없다.
            - bin/data로 살아야한다는 의미
    
    ![](https://velog.velcdn.com/images/losie2/post/10e9d8f1-1b78-48de-a4ec-e0f141461563/image.png)

    
    - printf(”2”);는 영원히 실행이 안된다!
    
- **Wait() 시스템 콜**
    - 프로세스 A가 wait 시스템 콜을 호출하면
        - 커널은 child가 종료될 때 까지 프로세스 A를 sleep 시킨다(block 상태)
        - child process 가 종료되면 커널은 프로세스 A를 깨운다 (ready 상태)
    
    ![](https://velog.velcdn.com/images/losie2/post/293e4a73-8ea7-4cfc-85ec-e3c2aefbc0ae/image.png)

    
    - 부모 프로세스는 wait() system call을 갖기 때문에 sleep상태를 갖게 된다.
        - 자식이 종료될 때 까지 기다린다.
- **exit() 시스템 콜**
    - **프로세스의 종료**
        - **자발적 종료**
            - 마지막 statement 수행 후 exit() 시스템 콜을 통해
            - 프로그램에 명시적으로 적어두지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어준다
        - **비자발적 종료**
            - 부모 프로세스가 자식 프로세스를 강제 종료시킴
                - 자식 프로세스가 한계치를 넘어서는 자원 요청
                - 자식에게 할당된 태스크가 더 이상 필요하지 않음
            - 키보드로 kill, break 등을 친 경우
            - 부모가 종료하는 경우
                - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨
- **프로세스와 관련한 시스템 콜**
    
    ![](https://velog.velcdn.com/images/losie2/post/71004c96-25e3-4896-bd56-5a4a8c01eeba/image.png)

    
- **프로세스 간 협력**
    - **독립적 프로세스 (Independent process)**
        - 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함
    - **협력 프로세스 (Cooperating process)**
        - 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음
    - **프로세스 간 협력 메커니즘 (IPC: Interprocess Communication)**
        
        ![](https://velog.velcdn.com/images/losie2/post/3d86d7da-0df8-48f8-9b10-cd035a06f89b/image.png)

        
        - 메시지를 전달하는 방법
            - **message passing**: 커널을 통해 메시지 전달
        - **주소 공간을 공유하는 방법**
            - **shared memory:** 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음
            - **thread**: thread는 사실상 하나의 프로세스 이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능

- **Message Passing**
    - **Message system**
        - 프로세스 사이에 공유 변수(shared variable)을 일체 사용하지 않고 통신하는 시스템
    - **Direct Communication**
        - 통신하려는 프로세스의 이름을 명시적으로 표시
            
            ![](https://velog.velcdn.com/images/losie2/post/90064ffa-f3fe-4021-b310-df908818790e/image.png)

            
    - **Indirect Communication**
        - mailbox (또는 port)를 통해 메시지를 간접 전달
            
            ![](https://velog.velcdn.com/images/losie2/post/b7e71be2-595d-4fd2-8449-e34747ff151a/image.png)

            

- **CPU and I/O Bursts in Program Execution**
    
    ![](https://velog.velcdn.com/images/losie2/post/2a659e54-a8cb-410c-b450-1cb1217e3cbe/image.png)

    
    - I/O Burst가 끼어든다
    - 프로그램의 종료에 따라선 빈도와 길이가 다르다

- **CPU-burst Time의 분포**
    
    ![](https://velog.velcdn.com/images/losie2/post/3b2bee50-61c8-4373-b1ca-3572c4dda945/image.png)

    
    - **여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케줄링이 필요하다**
        - Interactive job에게 적절한 response 제공 요망
        - CPU/와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용
        - job의 종류가 섞여있다는 것을 보여주는 그래프
        - **I/O bound job**
            - **사람과 계속 인터렉션을 하는 job**
                - 가능하면 사람과 interaction을 하는 job에게 CPU를 우선적으로 준다는 것이 스케줄링의 주요한 필요성
                
- **프로세스의 특성 분류**
    - 프로세스는 그 특성에 따라 다음 두 가지로 나눔
        - **I/O bound process**
            - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
            - **(many short CPU bursts)**
        - **CPU-bound process**
            - 계산 위주의 job
            - **(few very long CPU bursts)**
            
- **CPU Scheduler & Dispatcher**
    - **CPU Scheduler**
        - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다
    - **Dispatcher**
        - CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다
        - 이 과정을 context switch(문맥 교환)라고 한다
    - **CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다**
        
        ![](https://velog.velcdn.com/images/losie2/post/d8940b08-504a-4732-8a6a-f222c6575eef/image.png)
