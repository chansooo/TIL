# Thread-1

## Chapter 4. Threads

학습목표

1. Process 와 Thread의 차이점
2. Thread를 설계할때 기본 Issue
3. User Level Threads VS. Kernel Level Threads
4. 윈도우7, Solaris, Linux 에서 Threads 관리.

### Process VS Threads

Process의 characteristics(2가지)

- Resource Ownership
    - Process는 자신의 Process Image(PCB, User공간 ...)를 위한 Virtual address space를 가진다.
    - 그리고 때때로는 Main Memory, I/O devices, files들을 위한 Ownership을 가짐.
    - OS는 Process들끼리 원치 않는 interference(간섭)이 발생하지 않도록 Protection Function을 실행시킴.
    - Process → 자원 소유 하는 단위.
- Scheduling/Execution
    - Process Execution은 다른 Process들하고 interleaving(번갈아가면서)이 된다. → 즉 스케쥴링이 된다.
    - Process는 그래서 Running, Ready 같은 State를 갖게됨, Priority도 갖게됨
    - Process는 Scheduled, Dispatched되는 단위가 되는거임.
    - 스케쥴링 → interleaving하면서 우선순위 정하는...

이 두 가지 Characteristics는 서로 독립적임. 그래서 OS는 독립적으로 관리하게 됨.

- Dispatching의 단위(Unit) → Thread or Lightweight Process라고 부름
- Resource Ownership의 단위 → Process or Task라고 부름

**Multithreading** → 하나의 single process에 여러개의 동시에 실행 가능한 Path를  OS는 제공하게 되는데  이걸 Multithreading이라고 함.  (Ex. Word 문서작업에서 문서편집 path, 5분에 한번씩 저장하는 path... 이렇게 여러개의 Path가 한 Process에 존재하고 동시에 실행가능하다.)

![스크린샷 2022-04-04 오후 10.53.06.png](Thread-1%2030207/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.53.06.png)

 

Single Threaded Approach → 왼쪽 두개.

- 예전에는 thread 컨셉을 잡을 필요가 없었음
- 하나의 Process에는 하나의 Thread. → 실행 Path가 하나였다.
- MS-DOS에서 single user process밖에 지원하지 않았음
- Old version of UNIX에서는 multiple user process를 지원함.

MultiThreaded Approach → 오른쪽 두개.

- 한개의 Process에 여러개의 Thread
- JAVA Run-time환경에서 제공.
- 우측 하단 → multiple processes multiple threads per process. 
Window, Solaris, Linux에서 제공.

Process

- Multithreaded Environment에서 Resource Allocation의 단위이고 Protection의 단위이다.
- Virtual Address Space에서 Process Image를 저장하기 위해 Space를 할당 받게됨.
- Protected Access (보호해서 접근해야함)
    - Processors
    - Other proccess(for interprocess communication)
    - Files
    - I/O resources(devices and channels)
    

Thread가 가지는 것

- An execution state (Running상태, Ready상태)
- Running이 아닐때 Thread Context를 저장할 수 있어야함
- Thread가 수행 될때 필요한 Stack
- Thread가 수행할 때 Local Variable 저장 할 수 있는 공간
- 모든 Thread들이 Process의 자원들을 Share하기 때문에 Process의 Memory나 Resouce에 접근 할 수 있어야함.

![스크린샷 2022-04-04 오후 11.10.42.png](Thread-1%2030207/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.10.42.png)

TCB → Priority, Thread에 대한 ID ...

Thread → Lightweight Process라고 불리는 이유는 TCB안에 저장되어 있는 정보들이 PCB에 있는 정보보다 훨씬 Lightweight하게 들어있기 때문에 Lightweight Process라고 불린다. 그래서 Context Switch가 발생했을 때 PCB를 바꾸는 것 보단 TCB를 바꾸는게 Process의 Idle한 시간을 줄일 수 있다. 그래서 관리되는 정보가 작은 TCB로 바꾸면 성능을 높일 수 있다!

Benefit of Thread

1. 프로세스 생성하는 것 보다 Thread 생성하는게 시간 더 적게 든다.(b.c 적은 정보)
자원을 공유하기도 해서 적은 정보!
2. 관리되는 정보가 작다보니 종료할때도 Thread부분만 종료하면 돼서 good
3. 정보가 작다보니 process switching보다 Thread Switching이 시간 더 적게든다.
4. Thread간 통신에 있어서 더 빠르다. → Thread들은 Main Memory를 공유(Share)하고 있음 그래서 통신하기 더 좋다. 
비교하자면 Process간 통신은 User Mode에서 Kernel Mode로 가서 통신하고 다시 User Mode로 가야함. Mode Switching 2번이나 필요... → 훨씬 느리다.

Thread Use in Single-User System

- Foreground(인터페이스) and Background Work → Thread를 분리해서 인풋 받고 뒤에선 처리하고..
- Asynchronous(비동기) processing → 워드 5분마다 저장같은거 Thread로 할 수 있음
    - 다른 스레드들이 다른 일을 할 수 있도록한다.
- Speed of execution
- Modular program structure → 프로그램들을 모듈화해서 성능 올림

Thread 사용..

- Scheduling, Dispatching 단위가 Thread단위로 수행됨
- 실행에 사용되는 상태 정보들은 Thread-level data structure로 관리됨(TCB)
- Process안에서 모든 Thread에 영향을 미치는 작업
    - Process가 Suspending 됐을때. → Process가 Main Memory에서 빠지게 된거임. 그러면 그 Process에 속한 모든 Thread들은 다 Suspending된다.
    - Process가 Terminate 됐을 때 Thread들 다 종료된다.

Thread Execution State(상태)

- Running
- Ready                                                     → 기본 상태
- Blocked
- suspend 절대 X → 프로세스 수준이므로 절대 연관지으면 안됨.

---

<기본 상태들로 전환하기 위한 상태>

- Spawn → 생성되면 Ready로
- Block → I/O를 대기할때
- Unblock → Event 다
- Finish →  다 완료.

![스크린샷 2022-04-04 오후 11.42.21.png](Thread-1%2030207/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.42.21.png)

![스크린샷 2022-04-04 오후 11.43.01.png](Thread-1%2030207/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-04_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.43.01.png)

### Thread Synchronization(쓰레드 동기화)

- 꼭 필요함.
- 모든 Thread는 same address space와 resource들을 Share하고 있음
- 어떤 자원에 접근할 때 다른 Thread에 영향을 줘서는 안됨. → 그래서 공유된 정보를 동기화 해서 다른 쓰레드들이 뭐를 쓰는지 알려주는거임.