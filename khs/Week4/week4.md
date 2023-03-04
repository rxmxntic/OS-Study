## 💡 지난 내용 복습
![](https://velog.velcdn.com/images/losie2/post/46a8e18a-cc17-422f-b74e-a3df25420ac2/image.png)
    
   - **동기식 입출력과 비동기식 입출력**
        - **동기식 입출력**
            - I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
            - 구현 방법 1
                - I/O가 끝날 때까지 CPU를 낭비
                - 매시점 하나의 I/O만 일어날 수 있다.
            - 구현 방법 2
                - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
                - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
                - 다른 프로그램에게 CPU를 줌
        - **비동기식 입출력**
            - I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감
        - 두 경우 모두 I/O의 완료는 인터럽트로 알려준다.
        



- **스케줄러(Scheduler)**
    - **Long-term scheduler(장기 스케줄러 or job scheduler)**
        - 시작 프로세스 중 어떤 것들을 **ready queue**로 보낼지 결정한다
        - 프로세스에 memory를 주는 문제
        - time sharing system에는 보통 장기 스케줄러가 없다
    - **Short-term scheduler(단기 스케줄러 or CPU scheduler)**
        - 어떤 프로세스를 다음번에 running 시킬 지 결정
        - 프로세스에 CPU를 주는 문제
        - 충분히 빨라야함(millisecond 단위)
    - **Medium-Term Scheduler(중기 스케줄러 or Swapper)**
        - 여유 공간 마련을 위해 프로세스를 메모리에서 디스크로 보낸다.
        - 프로세스에게서 memory를 뺏는 문제
## 💡 Thread
   ![](https://velog.velcdn.com/images/losie2/post/5e20439d-5ec4-48c3-8dee-7623ddeb60b4/image.png)
   
   - Thread
   	- 주소 공간
      - stack, data, code로 이루어져있다.
    - PCB
        - program counter와 register를 포함한다.
        - 각 쓰레드(CPU 수행단위)마다 register에 어떤 값을 넣고,  program counter가 코드 어느 부분을 가리키고 실행되는가를 별도로 유지한다.
        - 프로세스 하나 당 공유할 수 있는 것을 최대한 공유한다.
        - 각종 자원들도 쓰레드들 끼리는 공유한다.
        - 별도로 가지고 있는 것
            - CPU 수행과 관련된 정보
            - Program Counter
            - Stack
            
    - **Lightweight Process**    
    
    - **구성**    	
       - program counter
       - register set
       - stack space
       
    - **동료 Thread와 공유하는 부분(=task)**
        - code section
        - data section
        - OS resources
        - **즉, 하나의 프로세스 안에 thread가 여러개 있다면 task는 하나만 있다는 뜻.**
            - 프로세스를 별도로 두는 것 보다 프로세스 안에 Thread를 여러개 두는 것이 훨씬 가볍기 때문에 효율적이다.
    - **전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있다.**
    - **다중 스레드로 구성된 Task 구조**에서는 하나의 서버 스레드가 **blocked(waiting)**상태인 동안에도 동일한 태스크 내의 다른 스레드가 **실행(running)**되어 빠른 처리를 할 수 있다
    - 동일한 일을 수행하는 다중 스레드가 협력하여 **높은 처리율(throughput)**과 성능 향상을 얻을 수 있다
        - 메모리 등 자원들을 아낄 수 있다는 의미이다.
            - 같은 일을 하는 프로세스를 여러개 띄워놓는다면 각각의 메모리에 올라가야 하기 때문에 메모리 낭비가 심하다.
    - 스레드를 사용하면 **병렬성**을 높일 수 있다.
        
       - 이례적인 경우.
            - CPU가 여러개 달린 컴퓨터에서만 얻을 수 있는 장점.
       		- 1000*1000 행렬 곱의 경우 독립적인 연산인데, CPU가 하나라              면 곱을 순차적으로 연산한다. 하지만 여러개 있다면 곱을              부분을 나눠서 수행한 후 마지막에 합친다.
            ![](https://velog.velcdn.com/images/losie2/post/824e0c7c-9d5f-4149-8e11-b88d9c61b866/image.png)
          🧨 위 처럼 (4 x 2)행렬과 (2 x 3)행렬의 경우 총 열 두번의 연산이 필요하다.
            🧨 열 두번의 연산을 CPU가 세 개인 경우 네 개씩 나누어 처리하기 때문에 연산이 빨라진다는 의미.
    ![](https://velog.velcdn.com/images/losie2/post/75e4ffff-b099-4ea3-ac97-42de300a43dc/image.png)
       - process는 하나이기 때문에 PCB는 하나만 생성
         - 쓰레드는 프로세스 마다 가지고 있는 정보 중 CPU 수행과 관련된 정보만 별도로 가지고 있게 된다.**(lightweight)**
    
- **Thread의 장점**
    - **Responsiveness**
        - 응답성. 사용자 입장에서 빠르다
        - 웹 브라우저가 스레드를 여러개 가지고 있게 되면 하나의 스레드는(이미지 등을 불러오는 스레드) 다른 스레드는 유지되고 있다.
            - 일종의 비동기식 입출력으로 이해해도 무방함
    - **Resource Sharing**
        - n threads 는 code, data, process의 자원들을 공유할 수 있다.
    - **Economy**
        - creating & CPU switching thread (rather than a process)
        - process 하나를 만드는 것은 overhead가 크지만, thread를 하나 추가하는 것은 overhead가 작다.
        - context-switch가 일어날 때, 하나의 프로세스로부터 또 다른 프로세스로 CPU가 넘어가는 것은 overhead가 크다.
            - Thread간 switch가 일어나는 것은 간단하다. 대부분의 문맥은 그대로 사용할 수 있기 때문이다.
    - **Utilization of MP Architectures**
        - CPU가 여러개 있는 경우 얻을 수 있는 장점
        - 각 스레드가 다른 프로세서에서 병렬로 실행될 수 있다면 처리 시간이 빨라지고 성능이 향상될 수 있습니다. 이는 동시에 실행할 수 있는 더 작고 독립적인 하위 작업으로 나눌 수 있는 작업의 경우 특히 그렇습니다. 여러 프로세서를 활용하면 각 스레드가 해당 하위 작업을 동시에 처리할 수 있으므로 전체적으로 완료 시간이 빨라집니다.