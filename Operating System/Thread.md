# Thread

## 학습 목표

- 프로세스와 쓰레드의 차이를 이해
- 쓰레드와 관련된 기본 설계 이슈 설명할 수 있다
- 사용자 수준의 쓰레드와 커널 수준의 쓰레드의 차이를 설명할 수 있다
- windows 8의 쓰레드 관리 기능을 설명할 수 있다
- solaris의 쓰레드 관리 기능을 설명할 수 있다
- Linux의 쓰레드 관리 기능을 설명할 수 있다.

## 프로세스와 쓰레드

- 프로세스의 두가지 특성
    - 자원 소유권
        - 프로세스는 프로세스 이미지를 위한 가상 주고 공간을 포함
            - 프로세스 이미지: PCB에 정의되어 있는 프로그램과 데이터, 스택, 속성들의 집합
        - 메모리, io채널, io장치, 파일과 같은 자원들에 대한 제어와 소유권을 프로세스에 할당할 수 있다
            - OS는 보호기능을 수행해서 프로세스 간에 자원에 대한 불필요한 간섭을 막음
    - 스케줄링/수행
        - 프로세스 수행은 하나 이상의 프로그램을 통과하는 track을 따른다.
        - 한 프로세스는 다른 프로세스들과 번갈아가면서(interleave) 수행될 수 있다
        - 따라서 프로세스는 수행 상태와 dispatching 우선 순위를 가지며 OS에 의해 스케줄되고 디스패키된다
    - 위의 두가지 특성은 서로 독립적이고 운영체제에 의해 독립적으로 취급될 수 있다
- 위의 두 가지 특성을 구병하기 위해
- 위에서 dispatching의 단위
    - Thread 또는 lightweight process(경량프로세스)라고 함
- 자원 소유권의 단위
    - Process 또는 Task라고 함

![Screen Shot 2022-04-09 at 1.54.37 AM.png](Thread%20093ed/Screen_Shot_2022-04-09_at_1.54.37_AM.png)

### 싱글 스레드

- 한 프로세스 → 한 스레드를 single-threaded approach(단일 스레드 접근 방법)이라고 함
- 스레드 개념 확실하지 않음
- MS-DOS가 유일한 싱글 유저 프로세스
- 옛날 버전의 UNIX는 다중 사용자 프로세스를 지원하지만 프로세스당 하나의 스레드만 지원

### 멀티쓰레딩

- 운영체제가 하나의 프로세스 내에서 수행되는 여러개의 쓰레드를 지원하는 지능
- 자바 실행 환경은 하나의 프로세스가 여러 스레드를 지원하는 시스템
- 윈도우 솔라리스 리눅스 모두 miltiple processe, miltiple threads를 지원
- 멀티스레드 환경에서의 프로세스
    - 보호의 단위와 자원 할당의 단위
- 프로세스와 연계된 것
    - 프로세스 이미지를 유지하는 가상 주소 공간
    - 처리기와 다른 프로세스, 파일, 입출력 자원에 대한 접근 제어
- 쓰레드가 포함하는 사항
    - Execution state(Running, ready....)
    - 수행 중이 아닐 때 저장되어 있는 스레드 문맥
        - 스레드를 보느 ㄴ관점 중 하나는 쓰레드를 한 프로세스 내부에서 동작하는 독립된 PC로 보는 것
    - Execution Stack(수행 스택)
    - 지역 변수 저장을 위해 각 스레드가 사용하는 정적 저장소(static storage)
    - 프로세스의 메모리 및 자원에 대한 접근
        - 메모리 및 지원은 프로세스 내의 모든 스레드에 공유됨
    
    ![Screen Shot 2022-04-09 at 1.58.59 AM.png](Thread%20093ed/Screen_Shot_2022-04-09_at_1.58.59_AM.png)
    
- 스레드의 장점
    1. 새로운 프로세스를 생성하는 것보다 새로운 스레드 생성하는게 시간이 더 짧음
    2. 프로세스 종료시간보다 스레드 종료시간이 더 빠르다
    3. 프로세스 간 교환보다 같은 프로세스에 있는 두 스레드 간 교환이 더 효율적이다
    4. 스레드는 서로 다른 수행 프로그램 간 통신에서도 효율적이다
        - 스레드는 메모리 및 파일을 공유하기 때문에 커널을 호출하지 않아도 서로 통신 가능
        - 프로세서는 커널리 개입돼야함
- 단일 사용자 멀티 프로세싱 시스템에서 스레드를 사용하는 예
    - foreground and background work
        - 하나가 스프레드 시트 작업할 동안 다른 스레드는 갱신하고 뭐 그럴 수 있음
    - asynchronous processing(비동기 처리)
        - 다른 스레드들이 다른 일을 할 수 있도록
        - 근데 이거랑 fore back이랑 뭐가 다르지?
    - speed of execution
        - 어떤 데이터 묶음을 계산하면서 동시에 다음 데이터 묶음 읽을 수 있음
    - modular program structure
        - 다양한 활동 혹은 입출력 연산에 대한 다양한 출발, 목적지를 포함하고 있는 프로그램의 경우 쓰레드들을 사용하여 설계하고 구현하는 것이 편리함(전담마크느낌)(전자담배마크 아님)

### 스레드

- 스레드를 지원하는 OS에서는 스케줄링과 디스패칭이 스레드를 기초로 하여 이루어짐
    
    → 실행에 관련된 대부분의 상태정보가 스레드 수준의 자료구조에 의해 유지됨
    
- 하지만 어떤 작업은 프로세스 내의 모든 스레드에 영향을 미침
    - 프로세서가 suspention되면 그 안의 모든 쓰레드가 suspendsion됨
    - 프로세스가 종료되면 모든 쓰레드 종료

## 스레드 기능

프로세스처럼 스레드도 수행상태(state)를 가지고 서로 동기화될 수 있음

### 쓰레드 상태

- 일반적으로 실행, 준비, 블록이 있음
- 보류(suspension)상태는 프로세스 수준의 개념이라서 쓰레드와 연관지으면 X
- 프로세스가 메모리로부터 스왑아웃 당하면 모든 스레드가 주소 공간을 공유하므로 모든 스레드가 스왑아웃됨
- 스레드 연산
    - Spawn(생성)
        - 프로세스가 생성될 때, 스레드가 스레드를 생성할 때
        - 스레드가 스레드를 생성할 때 새로운 애를 위해 명령 포인터와 인자들을 제공함
        - 새로운 스레드는 자신의 레지스터 문맥과 스택 공간을 가지고 ready 큐에 위치
    - Block(블록)
        - 쓰레드가 어떤 사건을 기다려야할 때 블록됨
            - 이따 자신의 사용자 레지스터, PC, 스택 보인터를 저장함)
            - 그 후 프로세서는 동일 프로세스 내에 있거나 다른 프로세스 내에 있는 준비 상태의 다른 스레드를 수행할 수 있음
    - Unlock(비블록)
        - 쓰레드가 블록되어 기다리던 이벤트가 발생하면 블록에서 레디큐로 이동함
    - Finish(종료)
        - 쓰레드가 작업을 완료하면 레지스터 문맥과 스택이 해제됨
- 쓰레드 하나가 블록되면 다른 것도 블록될까?
    
    ![Screen Shot 2022-04-09 at 3.13.06 AM.png](Thread%20093ed/Screen_Shot_2022-04-09_at_3.13.06_AM.png)
    
    - 단일 스레드 프로그램은 block을 기다렸다가 다른 RPC를 진행해야함
    - 각각의 RPC마다 스레드를 배정하면 둘 중 하나만 run을 하는 것은 맞지만 다른 하나가 응답을 받을 때 block이 되더라도 다른 스레드는 run을 할 수 있다
    - RPC: 서로 다른 기계 상에서 수행되는 두 개의 프로그램이 프로시저 호출/복귀 구문과 의미를 사용하여 상호작용하는 기법
    
    ![Screen Shot 2022-04-09 at 3.17.02 AM.png](Thread%20093ed/Screen_Shot_2022-04-09_at_3.17.02_AM.png)
    
    - 단일 처리기에서는 여러 프로세스 내의 여러 쓰레드들이 번갈아가며 수행될 수 있음
    - 두 프로세스 내의 세 쓰레드가 한 프로세서 내에서 번갈아 수행됨
    - 수행 중인 스레드가 블록되거나 정해진 시간 할당량을 모두 소비하면 다른 스레드로 넘어간다
    - 프로세스간 인터리빙
    

### 스레드 동기화

- 프로세스 내의 모든 쓰레드는 주소공간과 열린 파일과 같은 자원을 공유함
- 하나의 스레드에 의한 자원의 변경은 같은 프로세스의 모든 스레드에 영향을 미침
- 그래서 스레드들의 행위를 동기화해줘야함(하나가 선점하면 나머지는 기다리도록)