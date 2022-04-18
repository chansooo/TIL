# Thread-3(in OS)

## Windows에서 프로세스와 스레드 관리

---

- application : 하나 이상의 프로세스로 구성
- process
    - 자원 (가상 주소 공간, 코드, security context, PID, 변수 등등)을 가짐
    - 하나 이상의 스레드
    - 각 프로세스는 싱글 스레드 (primary thread)로 시작됨
- thread
    - 스케쥴링을 위한 단위
    - 예외 처리, 스케쥴링 우선순위, 스레드 local storage, TID를 가짐
- **job object**
    - 프로세스를 그룹 단위로 관리하기 위해서 사용
- **thread pool**
    - 자주 사용되는 (비동기적) callback 함수들을 스레드풀로 묶고 여러 application이 사용 → application을 줄일 수 있음
    - 공용으로 쓰는 스레드 그룹을 만든다고 생각
    

### Windows Process

- 객체로 구현됨
- 새로 생성 or copy 가능 (`fork` 사용)
- 한 개 이상의 스레드로 이루어짐
- 프로세스와 스레드 둘다 동기화에 대한 기능을 가지고 있음
- 프로세스 객체의 구성
    
    ![스크린샷 2022-04-10 오후 11.13.49.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-10_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.13.49.png)
    
    - access token : OS에 접근한 user의 권한을 담고 있음
    - virtual address descriptors : 자신에게 할당된 메모리 블럭들을 linked list의 형태로 관리
    - handle table : 각 handle은 할당된 자원에 대한 포인터를 가짐

### 프로세스와 스레드의 차이점

프로세스 : 자원을 소유하는 단위 (user job이나 application)

스레드 : dispatch되는 단위 (실제로 실행되고 스케줄링되는 단위)

### 프로세스 객체 attribute 목록

- PID
- Security descriptor : 객체를 생성한 사용자, 액세스 권한
- Base priority : 기본적인 우선순위
- Dafult processor affinity : 스레드가 실행될 수 있는 프로세서 목록
- Quota limits : 최대 가용 메모리, 최대 가용 프로세서 시간
- Execution time : 총 실행 시간
- IO counter : 수행한 IO 작업의 수와 타입
- VM operation counter : 수행한 가상 메모리 작업의 수와 타입
- Exception/debugging port : 예외를 처리하기 위한 프로세스 간 통신 채널
- Exit status : 종료한 이유

### 스레드 객체 attribute 목록

- TID
- Thread context : 레지스터 값들
- Dynamic priority : 동적으로 부여된 우선순위
- Base priority : 우선순위의 최소값 (하한)
- Thread processor affinity : 프로세서 목록
- 등등

### 멀티스레딩

- 한 프로세스 내의 스레드들은 공통 주소 공간을 이용해서 정보 교환 & 자원 공유
- 다른 프로세스의 스레드는 프로세스 간 shared 공간을 통해 정보 공유
- 단일 프로세스 내에서 스레드를 병렬적으로 처리함

### 스레드 상태 (6state)

![스크린샷 2022-04-11 오전 12.46.20.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.46.20.png)

- Ready : 실행될 준비가 됐음
- Standby : 우선순위에 의해 선점된 상태 (스케줄링에 의해)
    - Standby > Running : Running을 Ready로 보내고 Standby를 실행
    - Standby < Running : Running이 끝날 때까지 기다렸다가 Standby를 실행
- Waiting : Running 중 block 또는 suspend된 스레드들
- Transition : 이벤트는 발생해서 wakeup은 됐는데, 메모리가 아직 할당 안됐을 때
- Terminated : 종료

## Solaris의 프로세스

---

- 스레드에 연관된 개념들
    
    ![스크린샷 2022-04-11 오전 12.56.39.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.56.39.png)
    
    - 프로세스 : 자원을 할당받는 단위 (PCB, stack,m user address space 등 포함)
    - ULT : user application에 의해 생성되는 스레드
    - LWP : ULT-KLT를 매핑시켜줌 (여기까지 user가 볼 수 있음)
    - KLT : kernel단에서 스케쥴링과 dispatch를 위해서 생성되는 스레드
        - LWP말고도 커널에서만 쓸 수 있는 KLT도 존재
    
    LWP에서 syscall()을 할 때 System calls 인터페이스 → 커널 레벨에서 KLT를 실행해서 주는 것으로 예상 (LWP → KLT x)
    

- 초창기 UNIX와 Solaris 비교
    
    ![스크린샷 2022-04-11 오전 1.03.21.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.03.21.png)
    
    - UNIX는 스레드가 하나만 있었음
    - Solaris는 여러 LWP들을 linked list로 관리
    

### LWP의 구조

- LWP ID
- 스케쥴링 우선순위
- 커널로부터 오는 signal 처리에 대한 mask (accept or reject)
- user-level register 정보
- 커널 스택
- 자원 사용량
- 연관된 커널 스레드의 포인터
- 자신의 포인터

### Solaris 스레드 status

![스크린샷 2022-04-11 오전 1.09.49.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.09.49.png)

- RUN : =Ready
- ONPROC : =Running
- SLEEP : =Blcoekd
- PINNED : ONPROC 수행 중 인터럽트 발생하면 처리하기 위해서 잠깐 멈추는 곳
    - 인터럽트 스레드를 실행할 때 context switching으로 발생되는 오버헤드를 막는다
    - why?
        
         
        
- STOP : 디버깅 때문에 종료된 스레드
- ZOMBIE : 종료된 스레드
- FREE : 자원 release 완료

## Linux Task

---

리눅스에서는 프로세스를 task라고 부른다

### Task의 구조

- State : 프로세스의 상태 (executing, ready, suspended, stopped, zombie)
- Scheduling information : normal or real time
- Identifiers
    
    : PID, Group id(그룹과 관련된 권한을 처리하기 위해서)
    
- Links : parent, sibling, children 프로세스에 대한 link들
- Times and timers : 프로세스가 생성된 시간, 프로세서를 사용한 양
- File system : 사용하고 있는 파일에 대한 정보
- Address space : 자신에게 할당된 가상 주소 공간
- Processor specific context : 레지스터 정보

### Linux 프로세스/스레드 상태

![스크린샷 2022-04-11 오전 1.38.56.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.38.56.png)

- Running
    - Ready
    - Executing
- Wating
    - Uninterruptible ⇒ 사실상 suspend
        - 하드웨어에 대한 이슈
        - swap되어서 storage로 가있을 때
        - main memery로 와야 ready 상태가 된다
    - Interruptible ⇒ 사실상 block
        - 일반적인 인터럽트 (ex IO 인터럽트)의 시그널을 기다리는 상태
        - 시그널 받으면 Ready로 간다
- Stop
    - 디버깅 모드로 중지된 경우
- Zombie
    - 종료됨

### Linux 스레드

- Unix는 스레드라는 개념 없음 (single thread process)
- 그래서 Linux는 user-level에서 스레드를 만들고, 이를 kernel-level의 프로세스에 매핑시킴
- **clone** 으로 구현됨
    - `fork` : 자식 프로세스를 생성할 때 자원을 그대로 복제해서 주고, 두 프로세스는 분리된다. → **프로세스**
    - `clone` : 자원을 공유하는 프로세스를 생성한다. → **스레드 같은 프로세스**
- 결국 스케쥴링 단위는 프로세스
    - 하지만 같은 자원을 공유하는지의 여부로 스레드처럼 동작하도록
    

## Android의 프로세스와 스레드

---

- 응용프로그램은 app으로 구성
    
    ![스크린샷 2022-04-11 오전 2.13.51.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.13.51.png)
    

### 4가지 component

- Activity : 화면
- Service : 전원, 배터리 관리 등등을 제공함
- Content provider : 외부 앱에 컨텐츠를 제공 (ex 파일을 저장소에 저장)
- Boradcase receiver : 시스템 전반적인 알람을 수신

### Activity state (라이프사이클)

![스크린샷 2022-04-11 오전 2.14.30.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.14.30.png)

### 프로세스와 스레드

![스크린샷 2022-04-11 오전 2.16.26.png](Thread-3(i%2094baf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-11_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.16.26.png)

- 스레드의 우선순위가 계층구조로 이루어짐
1. Foreground process : 액티비티 + 연관된 프로세스
2. Visible process : foreground만큼은 아니지만 그래도 user에게 보여지는 프로세스
3. Service process : 백그라운드에서 수행 중
4. Background process : 사용자와 상호작용이 아예 없는 상태
5. Empty process : 종료된 이후 component가 하나도 없음
    - 빈번하게 사용되는 앱이라면 메모리에 남겨둠
    - 메모리 가용량이 줄어들면 이녀석부터 죽임