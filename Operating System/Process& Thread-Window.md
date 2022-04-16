# Process& Thread-Window

### 실제 Window에서 Process와 Thread를 어떻게 관리하는지.

- Application ⇒ 하나, 그 이상의 Process로 관리.
- Process ⇒ 각각의 Process는 Program을 실행하기 위한 Resource를 가진다.
    - Virtual Address Space
    - Executable Code
    - Open Handles to System Objects
    - Security Context
    - Unique Process Identifier
    - Environment Variable
    - Priority Class
    - Minimum and Maximum Working Set Sizes
    - 적어도 한개의 Thread를 가진다.
- Primary Thread
    - 각 Process가 시작할 때 생성되는 Single Thread
- Thread
    - Execution Schedule을 위한 단위(entity) → Dispatch되는 것 Thread통해!
    - 구성요소
        1. Exception Handler
        2. Scheduling Priority
        3. Thread Local Storage
        4. Thread Identifier(ID)
        5. System에 필요한 Structure Sets를 가진다.
- Job Object ~~다시공부하기 설명 짧게하심~~
    - Process를 그룹단위로 관리하기 위해 쓰는 것
    - Process들의 그룹을 지정하고 스케쥴링에 필요한 Priority를 처리..같은 일 할때 쓴다.
- Thread Pool(공용으로 쓰는 Thread)
    - Asynchronous(비동기적) Callback 함수에 사용
    - 많이 사용하는 Callback함수들을 Thread로 묶어 놓고 Application에서 사용 할 수 있도록 한다.
    - Applicaiton Thread의 개수를 줄일 수 있다.
    - Callback함수?
        - Click 했을 때 팝업창 뜨게 한다거나
        - BackGround에서 5분마다 저장해라..이런 종류
        - 각 어플리케이션에서 만드는게 아니라 Callback함수를 Thread Pool에 넣어서 많이 쓰는 function기능들을 사용할 수 있도록 한다.

### Window Process

- Object(객체)로 구현되어 있음.
- Process는 새로운 Process로 생성이 되거나 아니면 기존의 Process를 복사해서 생성됨.
- Process는 한개 이상의 Thread를 가지고 있다!!
- Process와 Thread는 자체적인 Synchronization(동기화)하는 능력을 가지고 있다.

### Window에서의 Process와 Resources

![IMG_5E6AB3C2D199-1.jpeg](Process&%20T%20f689a/IMG_5E6AB3C2D199-1.jpeg)

- Access Token : 유저가 OS에 접근하게 되면 그 유저의 권한에 대한 Access Token이 부여됨. 권한의 종류에 따라 이 Process가 접근 할 수 있는 한계가 있음.
이 Access Token을 가진 사용자가 만든 모든 Process들은 모두 같은 권한을 갖는다.
- Handle Table : 나에게 할당된 자원들에 접근할 수 있는 Pointer를 가진다.
- Virtual Address Descriptors : Process가 Memory Mangement로부터 할당 받은 Virtual Memory가 Linked List 형식으로 관리되고 있음

### Window에서의 Process와 Thread의 차이점.

- Process : User Job 이나 Applicaiton의 작업 Resource를 가지는 단위
- Thread : Dispatch 되는, 실행(execute), 스케쥴링, Interrupt의 단위

### Window Process 속성들

1. Process ID : 고유 ID
2. Security Descriptor : 누가 이 객체(Process)를 생성했는지, 누가 access권한을 가지는지.
3. Base Priority : Process의 기본적인 Priority
4. Default Processor Affinity : 내가 어떤 Processor에 할당돼서 사용할 수 있는지
5. Quota Limits : 내가 Memory를 얼만큼 쓸 수 있는지, Processor를 얼마나 사용할 수 있는지
6. Execution Time : 총 실행시간이 얼마나 되냐
7. I/O counters : I/O를 얼마나 썼는지 기록
8. VM Operation Counter : Virtual Mem에 대한 type, 얼마나 사용했는지를 기록
9. Exception/Debugging Ports : 예외처리가 Thread로 발생했을 떄 그 Thread와 통신하기 위한 Channel
10. Exit Status : 왜 종료 됐는지에 대한 값을 저장하는 속성.

### Window Thread 속성들

1. Thread ID : Thread 고유 ID
2. Thread Context : Register 값을 가짐.
3. Dynamic Priority : 상황에 따라 동적으로 부여되는 Priority
4. Base Priority : 기본적인 이 Thread가 가지는 하한값. 우선순위가 이것보다 더 내려갈 수 없다.
5. Thread Procesor Affinity : 어떤 Processor에 할당돼서 사용 할 수 있는지
6. Thread Execution Time   이밑으로는 기타정보라고 말씀하심~~~
7. Alert Status
8. Suspension Count
9. Impersonation Token
10. Termination Prot
11. Thread Exit Status

### Window에서의 Multithreading.

- 윈도우에서는 Muliple Process없이도 동시에 쓰레드 병렬 처리 가능 → 병렬처리 가능!
    - 같은 Process안에 있는 Thread들은 Thread들 간에 Shared된 공간을 통해서 일반적으로 정보들을 적게되면 다른 Thread가 바로바로 갖고 갈 수 있음.
    - 다른 Process에 있는 Thread간의 통신은 Process들 사이에 Shared된 공간을 통해 정보를 교환

### Window에서 Thread State(상태)

![IMG_2F2123272A78-1.jpeg](Process&%20T%20f689a/IMG_2F2123272A78-1.jpeg)

- Standby State
    - Running상태로 갈 수 있게 이미 선점된 놈.
    - Priority가 Running보다 높으면 Running에 있던 놈은 Ready상태로 감.
- Transition State
    - Waiting State에서 기다리다가 Ready State로 갈 수 있게 되었는데 내가 쓸 수 있는 Memory(Resource)가 Block(부족한상태)면 Resource(자원)을 쓸 수 있을때까지 기다리러 Transition State로 간다.