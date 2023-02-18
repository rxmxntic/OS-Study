System Structure & Program Execution

Computer 

- CPU

- Memory

 : cpu의 작업공간

cpu는 클럭 사이클마다 메모리에서 기계어를 하나씩 읽어 실행

​

I/O device

- Disk

ex) 키보드 : input device

        프린터, 모니터 : output device

        하드디스크 : 보조기억장치

                             데이터를 읽어 메모리로 읽어주기(input device 역할)

                             처리 결과를 disk의 파일 시스템에 저장(output device 역할)

​

I/O device

I : (input) device의 데이터가 컴퓨터 안으로 들어가는 것

O : (out) 데이터를 받아 computer에서 처리하고 결과를 device로 내보내는 것

device controller

 : 해당 I/O device를 관리하는 일종의 작은 CPU

   제어 정보를 위해 control register(지시하기 위함), status register를 가짐

    local buffer를 가짐 

→  I/O는 실제 device와 local buffer 사이에 일어남

→ device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림

​

local buffer : device controller의 작업공간

register​ : memory보다 빠르고 정보를 저장 가능한 공간

​

device driver(장치구동기)

: OS코드 중 각 장치별 처리루틴 → software

device controller(장치제어기)

: 각 장치를 통제하는 일종의 작은 CPU → hardware

mode bit 

: CPU안에 존재,

 cpu에서 실행 되는 것이 운영체제인지 사용자 프로그램인지 구분해주는 것 

사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호장치 

​

mode bit가 1인 경우

        사용자 모드 : 사용자 프로그램 수행

mode bit가 0인 경우

         모니터 모드 : OS코드 수행 (= 커널 모드, 시스템 모드)

​

보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행 가능한 특권명령으로 규정

Interrupt나 Exception 발생 시 하드웨어가 mode bit를 0으로 바꿈

사용자 프로그램에게 CPU를 넘기기 전에 mode bit를 1로 셋팅

interrupt line : cpu에 붙어있음

인터럽트 발생 → 다음 기계어를 실행하기 전, interrupt line에 인터럽트 들어온게 있는지 확인 함 → 인터럽트가 왔으면, 자동적으로 CPU는 운영체제에게 넘어감 

​

프로그램이 디스크에서 데이터 가져와야 하는 상황시

cpu가 disk의 controller에 데이터 확인 요청 → disk에서 데이터 출력  → 자신의 local buffer에 저장

(이 과정동안 cpu는 쉬지않고 memory에 접근)

​

키보드(i/o device)에서 입력을 받아야하는 경우 cpu를 얻는 과정

키보드에 입력된 데이터가 buffer에 입력 → 키보드의 컨트롤러가 cpu에 인터럽트 → cpu의 제어권이 운영체제로 넘어감 → 입력된 키보드의 데이터를 요청받은 프로그램의 메모리에 복사 

 timer

 : 특정 프로그램이 cpu를 독점하는 것을 막기 위함

처음엔 os가 cpu차지 → timer에 값 세팅후 → 프로그램 실행시 cpu를 넘김 : timer에 할당 된 시간만 사용 가능

→ 인터럽트 라인 체크 → 인터럽트 들어온게 없으면 다음 명령 수행 (단, cpu를 뺏을 순 없음!)

정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생시킴

타이머는 매 클럭 틱(clock tick) 때마다 1씩 감소

타이머 값이 0이 되면 타이머 인터럽트 발생

CPU를 특정 프로그램이 독점하는 것으로부터 보호

​

클럭 틱(clock tick) : 정기적으로 일어나는 특별한 인터럽트

​

→ 타이머는 time sharing을 구현하기 위해 널리 이용됨

→ 타이머는 현재 시간을 계산하기 위해서도 사용 

Memory controller : 동시 접근 방지 & 접근 정리

입출력(I/O)의 수행

모든 입출력 명령은 특권 명령

사용자 프로그램은 어떻게 I/O를 하는가?

시스템콜(System Call) : 사용자 프로그램은 운영체제에게 I/O요청

trap을 사용하여 인터럽트 벡터의 특정 위치로 이동

제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동

올바른 I/O 요청인지 확인 후 I/O 수행

I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김

사용자 프로그램은 직접 입출력 장치에 접근 X →  운영체제를 통해서만 접근 가능!

​

시스템콜(System Call)

사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것  

인터럽트(Interrupt)

인터럽트 당한 시점의 레지스터와 program counter를 저장한 후 CPU의 제어를 인터럽트 처리 루틴에 넘김

넓은 의미

- interrupt(하드웨어 인터럽트) : 하드웨어가 발생시킨 인터럽트

- Trap(소프트웨어 인터럽트) : Exception - 프로그램이 오류를 범한 경우

                                               System call - 프로그램이 커널 함수를 호출하는 경우

 →  현대의 운영체제는 인터럽트에 의해 구동됨

​

인터럽트 관련 용어

인터럽트 벡터

 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음

인터럽트 처리 루틴 (= Interrupt Service Routine, 인터럽트 핸들러)

 : 해당 인터럽트를 처리하는 커널 함수

동기식 입출력과 비동기식 입출력

동기식 입출력(synchronous I/O)

I/O 요청 후 입출력 작업이 완료된 후에 제어가 사용자 프로그램에 넘어감

구현 방법 1

   - I/O가 끝날 때까지 기다림

   - 매시점 하나의 I/O만 일어날 수 있음

   - CPU를 낭비시킴

구현 방법 2

   - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음

   - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움

   - 다른 프로그램에게 CPU를 줌

​

비동기식 입출력(asynchronous I/O)

I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

​

→ 두 경우 모두 I/O의 완료는 인터럽트로 알려줌

DMA(Direct Memory Access)

빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용

CPU의 중재 없이 device controller가 device의 buffer 저장소의 내용을 메모리에 block 단위로 직접 전송

바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴

→ cpu가 인터럽트 당하는 빈도를 줄여 빠르게 사용 가능

​

서로 다른 입출력 명령어

I/O 를 수행하는 special instruction에 의해

→ 메모리에 접근하는 명령어와 I/O를 수행하는 명령어 각 따로 존재

memory mapped I/O에 의해

→ 파일의 일부를 프로세스의 메모리 영역에 mapping하여 사용

​

저장장치 계층 구조

Primary    - Registers

                            ↑ ↓

                 - Cache Memory

                            ↑ ↓

                 - Main Memory

                             ↑ ↓

Secondary - Magnetic Disk

                               ↑ ↓

                    - Optical Disk

                               ↑ ↓

                    -  Magnetic Tape

​

→ 위로 갈수록 빠름,  단위공간당 가격이 비쌈(용량이 적음)

​

Primary                                                                        Secondary

 : 휘발성                                                                        : 비휘발성   

 : CPU에서 직접 접근가능                                            →  섹터 단위의 접근으로 바로 실행 불가능

→  바이트 단위의 접근으로 바로 실행 가능

​

​

Caching 

- 명령어와 데이터를 캐시 기억 장치 또는 디스크 캐시에 일시적으로 저장하는 것

- 필요할 때마다 바로바로 데이터를 전송하는 기술

​

virtual memory(가상 메모리)

 당장 실행해야 하는 부분만 주기억장치에 넣고 나머지는 보조기억장치에 넣어 동작하도록 하는 것

​

file system의 하드 디스크 

전원이 나가더라도 내용 유지 →  비휘발성

​

swap area의 하드디스크

전원이 나가면 의미 X, 전원이 나가면 프로세스 종료 →  휘발성

 → 메모리 연장공간으로 사용

​

kernel address space

code(커널 코드)

 - 시스템콜, 인터럽트 처리 코드

 - 자원관리를 위한 코드

 - 편리한 서비스 제공을 위한 코드

data

 - 운영체제가 사용하는 자료구조

 - cpu, memory, disk, process 관리 및 통제

stack

- 사용자 프로그램마다 커널 스택 별도 사용

​

사용자 프로그램이 사용하는 함수(function)

사용자 정의 함수

 - 자신의 프로그램에서 정의한 함수

​

라이브러리 함수

 - 자신의 프로그램에서 정의하지 않고 갖다 쓴 함수

 - 자신의 프로그램의 실행 파일에 포함

​

커널 함수

 - 운영체제 프로그램의 함수

 - 커널 함수의 호출 = 시스템 콜

​

→  사용자 정의 함수와 라이브러리 함수는 프로그램안의 코드 함수

→  커널 함수는 커널 코드 안의 함수

​

프로그램의 실행

프로그램 시작 → A의 주소공간의 user mode에 CPU 존재 , 사용자 정의 함수 호출

→ 시스템 콜  →  커널의 주소공간의 kernel mode → 시스템 콜 종료

→ A에 CPU제어권,  user mode에서 함수 실행, 라이브러리 함수 호출

→ 시스템 콜  →  커널에 CPU제어권,  kernel mode → user mode → kernel mode →...   → 시스템 콜 종료

​

user mode와 kernel mode를 반복수행 후 프로그램 종료

​