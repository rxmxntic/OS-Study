- 현대적 CPU Scheduling은 선점형이다.
- **성능 척도**
    - **CPU utilization(이용률)**
        - CPU를 가능한 빠르게 유지한다
        - CPU가 놀지 않고 일 한 비율
    - **Throughput(처리량)**
        - 주어진 시간 동안 과연 몇 개의 작업을 완료했는지?
    - **Turnaround time (소요시간, 반환시간)**
        - CPU를 쓰러 들어와서 다 쓰고 나갈 때 까지 걸린 시간.
            - CPU Burst다 끝나고 나갈 때 까지 걸린 시간
    - **Waiting Time(대기 시간)**
        - CPU 순서를 기다리는 시간
            - 선점형의 경우 CPU를 빼앗기기도 하기 때문에 기다린 시간을 전부 다 합친 시간.
    - **Response Time(응답 시간)**
        - Ready  Queue에 들어와서 처음으로 CPU를 얻기까지 기다린 시간
            - 최초의 CPU를 얻기까지 기다린 시간

- **FCFS(First-Come First-Served)**
    - 비선점형 스케쥴링
    
    ![](https://velog.velcdn.com/images/losie2/post/faaa73d9-a815-4fed-a265-6772ca0cbb35/image.png)

    
    ![](https://velog.velcdn.com/images/losie2/post/28248cca-6765-4831-88c0-d3ab6376437e/image.png)

    
    - **Waiting time** = P3 = 3;  P2 =  0; P3= 6;
    - **Average Waiting Time** = (6 + 0 + 3) = 3
    - 이전 예시보다 더 효율적이다.
    - **Convoy effect : 긴 프로세스 하나 때문에 짧은 프로세스들이 오래 기다려야 하는 것.**

- **SJF(Short Job First)**
    - 각 프로세스의 다음번 **CPU Burst Time**을 가지고 스케줄링에 활용
    - **CPU Burst Time**이 가장 짧은 프로세스를 제일 먼저 스케줄
    - **Two schemes:**
        - **비선점형**
            - 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점당하지 않음
        - **선점형**
            - 현재 수행중인 프로세스의 남은 **burst time**보다 더 짧은 **CPU burst time**을 가지는 새로운 프로세스가 도착하면 CPU를 **빼앗김**
            - SJF의 **선점형** 방법을 **Shortest-Remaining-Time-First(SRTF)**이라고도 부른다
    - **SJF is optimal(선점형)**
        - 주어진 프로세스들에 대해 **minimum average waiting time**을 보장
    
- **비선점형 SJF**
    
    ![](https://velog.velcdn.com/images/losie2/post/5dba7623-b20f-40c1-a7eb-308a27010a86/image.png)

    
    - **CPU 스케줄링이 언제 이루어지는가?**
        - CPU를 다 쓰고 나가는 시점
- **선점형 SJF**
    
    ![](https://velog.velcdn.com/images/losie2/post/54e511ca-adae-454a-afd0-8dc3a4ad86c2/image.png)

    
    - **CPU 스케줄링이 언제 이루어지는가?**
        - 새로운 프로세스가 도착하면 언제든지 스케줄링이 이루어짐.

- **다음 CPU Burst Time의 예측**
    - 다음번 CPU burst time을 어떻게 알 수 있는가?
    - **추정(estimate)만 가능**
    - **과거의 CPU burst time을 이용해서 추정**
    
    > [https://charles098.tistory.com/82] (https://charles098.tistory.com/82)의 글 참고.
    

- **Priority Scheduling(우선순위 스케줄링)**
    - 각 프로세스 별로 우선순위가 주어진다
    - 높은 우선순위를 가진 프로세스에게 CPU 할당
    - 낮은 정수 값 == 높은 우선순위
        - 선점형
        - 비선점형
    - **SJF는 일종의 priority scheduling이다.**
        - **priority = predicted next CPU burst time**
    - **문제점**
        - **Starvation(기아 현상):** 낮은 우선순위의 프로세스들은 영원히 실행되지 않는다
    - **해결법**
        - **Aging(노화):** 우선순위가 낮은 프로세스의 우선순위를 점점 증가시킨다.

- **Round Robin(RR)**
    - 가장 현대적인 스케줄링 기법
    - 응답시간이 빨라진다는 장점이 있음.
    - 각 프로세스는 동일한 크기의 **할당 시간(time quantum)**을 가짐
        - 일반적으로 10-100 milliseconds
    - 할당 시간이 지나면 프로세스는 **선점(preempted)**당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다
    - n 개의 프로세스가 **ready queue**에 있고 할당 시간이 q time unit인 경우 각 프로세스는 최대 **q time unit** 단위로 **CPU시간의 1/n**을 얻는다
        - 어떤 프로세스도 **(n-1)q time unit** 이상 기다리지 않는다
    - **Performance**
        - **q 가 커지면 FCFS가 된다.**
        - **q 가 작아지면 context switch 오버헤드가 커진다**
        - 적당한 q의 크기를 주는 것이 중요함
        
        ![](https://velog.velcdn.com/images/losie2/post/7360b8ff-be4f-4b75-9593-b0a3b1484e67/image.png)


  - 예제 설명
  	
    - P2는 **Time Quantum(20)보다 Burst Time이 작기 때문에** 작업을 완료한다.
    - 때문에 다음 burst는 37에 시작한다.
    - 모든 작업은 **Time Quantum(20)**까지만 진행할 수 있으며, 초과시 다음 프로세스에게 CPU를 넘겨준다.
  - 일반적으로 SJF보다 **average turnaround time**이 길지만 **response time**은 더 짧다
  
  
> - 출처: http://www.kocw.or.kr/home/cview.do?mty=p&kemId=1046323