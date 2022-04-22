# Ch05-22S-2

### Mutual Exclusion의 요구사항

- 특정한 리소스를 쓰기 위해서는 상호 배제가 꼭 필요하다
    - 그러기 위해서는 하나의 프로세스만 돌아갈 수 있는 Critical Section이 있어야한다
- non-critical section에서 중지된(halt) 프로세스는 다른 프로세스들한테 영향을 끼쳐선 안된다
    - == 다른 프로세스들은 크리티컬 섹션에 들어갈 수 있어야함
- Critical section에 들어가려고 기다리는 놈들이 무한정 기다리게 해선 안된다
    - == 교착, 기아 피해야한다
- Critical section에 아무 프로세스도 없으면 어떤 프로세스가 들어가려고 하면 즉시 들어갈 수 있어야한다(delay X)
- Mutual Esclusion을 할 때 프로세스의 개수나 스피드에 대한 가정은 없다고 생각
- Critical section에 들어간 친구들은 특정 시간까지만 있게 하고 나오게 만들어야함
→ Starvation, Deadlock 가능!

### 하드웨어 지원으로 Mutual Exclusion

- **Interrupt Disabling(인터럽트 금지)**
    - 유니프로세스 시스템에서 프로세스는 overlapping을 할 수 없다!(interleaving만 가능)
    - 프로세스가 OS service를 호출 or interrupt 되기 전까지 계속 수행이 가능함
    - 즉 interrupt가 발생하지 않으면 그 동안은 한 프로세스의 계속적인 실행을 보장.
    - Mutual Exclusion을 보장하려면 프로세스가 인터럽트를 금지하면 된다.
    - 인터럽트만 금지하게 하면 mutual exclusion을 실행할 수 있다 → Kernel에서 제공
    - 단점:
        - 인터리빙 제한하니까 수행(execution) 효율성이 많이 떨어진당
        - 멀티프로세서에서는 Hardware지원을 통한 Mutual Exclusion을 사용할 수 없음
            
            → 인터럽트가 금지된 상황에서도 서로 다른 프로세스가 공유 자원을 동시에 접근하는 경우가 가능하기 때문.
            
- **특별한 기계 명령어(두개의 기능을 원자적으로 처리)**
    - 멀티프로세서 환경에서 여러 프로세서들은 공통의 Main memory를 공유
    - 그래서 이때 각 프로세서는 동등한 관계에서 독립적으로 동작. ⇒ 프로세서들 간에 Mutual Exception 할 수 있는 인터럽트 기법은 없다.
    - Compare & Swap Instruction
        
        ![Screen Shot 2022-04-14 at 2.41.33 AM.png](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_2.41.33_AM.png)
        
        - 메모리에 저장된 값(`*word`)과 테스트의 값(`testval`)이 같다면 메모리에 저장된 값(`*word`)을 새로운 값(`newval`)으로 변경하겠다
        - 기존에 저장된 값(`*word`)을 `oldval`로 옮기고 `oldval`랑 `testval`를 비교.
        - 같으면 메모리에 저장된 값(`word`)에 새로운 값(`newval`)넣어버림
        - `return` 값은 `oldval`
        - atomically하게 수행됨 → 수행되는 동안 interrupt 발생할 수 없음

![IMG_295830E5498B-1.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_295830E5498B-1.jpeg)

- `compare and swap`은 앞 두개가 같은값이면 맨 앞의 값을 뒤에 값으로 바꿔준다.
- P(k)를 먼저 수행 → critical section에 들어가 있음
- 그 다음 P(1)을 실행 → P(k)가 아직 critical section이 안끝났기 때문에 끝나고 `bolt = 0`을 하기 전까지 계속 while문에 갇혀있다!

![IMG_4ABC562CD3DB-1.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_4ABC562CD3DB-1.jpeg)

- P(k)가 들어가서 `bolt`의 값을 1로 바꿔준다.
- 그 이후 P(k)는 Critical Section 수행..
- 그 때 P(1)이 들어와서 exchange만나면 `bolt`가 1이기 때문에 while문에 갇힌다!!(=기다린다)
- `bolt`  → 공유변수, Critical Section에 진입 할 수 있는 유일한 프로세스는 이 변수가 0일때
`compare and swap`명령을 호출한 프로세스이다.
- 다른 프로세스들 → busy waiting을 수행
busy waiting : Critical Section에 들어가기 위한 허가를 받기 위해 변수를 테스트하는 걸 반복 실행하는 것.
- `bolt = 0` : 임계영역에 프로세스가 없다는 의미
- `bolt = 1` : 하나의 프로세스가, `key`값이 0인 프로세스가 Critical Section에 있다는 의미.

- 특별한 기계 명령어(special machine instruction) 사용의 장점
    - Interrupt disable(인터럽트금지) 는 Multiprocessor에서는 사용할 수 없었다.
    - 변수를 Memory를 Share해서 사용하고 있으니까 Processor와 상관없이 Critical section에는 한 processor만 사용한다!!
    - 변수를 사용할 수 있으면 변수값에 따라 Processor와 상관없이 Mutual Exception을 적용할 수 있음
    - 간단하고 검증하기 쉽다.
    - multiple critical sections 사용 가능
        - 각각의 critical section에 대해서 각자의 `bolt`값을 만들어줘버림
        - CS1 - `bolt1`    CS2 - `bolt2`
- 특별한 기계 명령어 사용의 단점
    - 대기하는 놈들 while문 오지게 돌아서 Busy waiting 상태가 됨 → processor time 소비
    - Starvation 가능
        
        → 프로세스가 CS를 떠날 때 한개 이상의 Process들이 기다리고 있으면 기다린 순서랑 상관없이 아무거나 실행됨... 어떤놈은 계속 선택 안될 수도.
        
    - Deadlock 가능
        
        → P1이 자원 R1를 사용하고 있는데(CS에 들어가있는데) 우선순위가 높은 P2가 실행되면 OS에서는 우선순위가 높은 P2를 스케쥴링을 통해 실행시킨다. P2가 실행중에 P1이 사용하고 있는 R1를 쓰고 싶다고 하면 CS에는 P1이 들어가 있어서 P2는 계속 기다리고 P1은 P2가 끝날때까지 CS에 죽치고 앉아있기 때문에 P1, P2 둘다 아무고토못하고 Deadlock 쌉가능
        
    

## Concurrency Mechanisms

- Semaphore
    - 프로세스 간에 시그널을 주고받기 위해 사용되는 정수 값.
    - counting semaphore(general semaphore): 값을 3 이상도 가질 수 있음
- Binary semaphore
    - 값을 0 or 1만 가짐.
    - lock을 건 Process와 Unlock을 하는 Process가 달라도 된다.
- Mutex
    - binary semaphore와 비슷
    - lock을 건(=0) Process와 unlock(=1) 하는 Process가 같다.
- Monitor → 뒤에 계속..
- Mailboxes/Messages → 뒤에계속...
- Spinlock

## Semaphore(중요합니다!)

- integer value를 가지는 변수라고 볼 수 있음
- 여기에 연관된 opreation 3개가 정의돼있음
    - 세마포어 초기화 : semaphore 음이 아닌 정수로 초기화해주는 애
    - `semWait`: 세마포어 값을 감소시키는 애, 음수가 되면 프로세스를 블락시킴
    - `semSignal`: 세마포어 값을 증가 시키는 애, 
    0 or 음수면 값을 증가시키면서 블록된 프로세스들을 깨운다.

**<세마포어 동작 방식>**

1. 세마포어 0 or 양수값으로 초기화됨
2. 양수라면 그 값은 `semWait`를 호출 한 후 대기 없이 수행 할 수 있는 프로세스의 개수.
3. 0이면
    1. 초기화에 의해 0일수도 있고
    2. 초기화된 양수 값에 해당하는 개수의 프로세스들이 다 `semWait`를 호출 → 0
4. 0이후 `semWait`가 더 호출하는 프로세스들은 다 block되고 세마포어의 값은 음수가 됨.
5. 그 이후 `semWait`가 계속 호출 되면 계속 세마포어 값이 감소됨.
6. 이때 세마포어의 값의 절대값은 블록상태인 프로세스의 개수임.
7. `semSignal`이 호출되면 세마포어 값은 증가하며 만일 대기하는 프로세스가 있으면 하나씩 큐에서 remove한다!

- 결과
    1. 프로세스가 세마포어를 감소(`semWait`실행)시키기 전까지 그 프로세스가 block될지 안될지 알 수 없음.
    2. 프로세스가 세마포어를 증가(`semSignal` 실행)시켜 블락된 프로세스를 깨우면 이 두 프로세스가 모두 수행가능 상태가 되는데, 둘 중에 뭐가 먼저 수행될지는 알 수 없다.
    3. 세마포어에 시그널을 보낼 때(`semWait` or `semSignal`) 우리는 다른 프로세스가 대기 중인지 여부를 알 수 없음.
    즉 블록되어 있는 프로세스의 개수는 0또는 1일 수 있다.

### semaphore(노말버전)

- 세마포어는 구조체임
    - `count`(int value)
    - block된 process를 저장하는 `queue`가 있음
- `semWait`
    - count- -  → 세마포어값 감소.
    - count가 0보다 작으면 Process를 queue에 넣어버림.
    - 그리고 그 Process는 block함
    - 만약 0 이상이면 계속 그 Process를 실행시킨다.
- `semSignal`
    - count++ → 세마포어값 증가.
    - count ≤ 0이면 큐에 있는 Process 하나 remove한다.
    - 그 Process를 ready list로 옮김
    - 만약 0보다 크면 block되어 있는 놈이 없다는 거임
- 코드
    
    ![IMG_A0F6B099B3EF-1.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_A0F6B099B3EF-1.jpeg)
    

### Binary semaphore

- 값을 0, 1만 가질 수 있음
- Binary 세마포어는 값이 1로 초기화된다.
- Process block하는 queue존재.
- val == 1 이면 Critical Section 수행할 수 있다
- val == 0 이면 Critical Section 이미 다른 Process가 수행하고 있다.
- `semWaitB`
    - val == 1 이면 val을 0으로 바꿔줌.
    - val == 0이면 큐에 넣어버림 → block
        - 만약 들어 왔을 때 val==0의 의미? 다른 Process가 Critical Section 수행 중이다~
        - val=0 을 가지고 들어오면 block돼서 queue에 들어간다.
- `semSignalB`
    - queue가 비어있으면 val을 1로 만들어버림 → 다른 Process가 사용 할 수 있게함.
        - 무슨 의미? 사용하고 있는 Process 없다!
    - queue 안 비어있으면
        - queue에서 프로세스를 지우고 ready 리스트에 넣음
- 코드
    
    ![IMG_A0F6B099B3EF-1.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_A0F6B099B3EF-1%201.jpeg)
    
- Mutex와 Binary semaphore 차이점: 
Binary semaphore는 `semWaitB`를 호출하는 Process와 `semSignalB` 호출하는 Process가 달라도됨.
Mutex는 `semWaitB`를 호출하는 Process와 `semSignalB` 호출하는 Process가 같아야함.

### Strong / Weak Semaphore

- Strong Semaphore
    - 순서가 정해져있어서 큐에 있는 block된 Process를 Ready List에 넣는 순서가 있는 거 
    (FIFO → 가장 오래된 애가 먼저 나옴, 먼저 들어간애가 제일 먼저 나온다.)
    - Starvation(기아)가 발생할 수 없다
- Weak Semaphore
    - FIFO가 아니라 우선순위 or 랜덤으로 나온다.
    - 순서가 없으니까 Starvation(기아)가 발생할 수도 있다

### Strong Semaphore

- 그림
    
    ![KakaoTalk_Photo_2022-04-14-22-21-17.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/KakaoTalk_Photo_2022-04-14-22-21-17.jpeg)
    
- A, B, C →
- A, B, C는 sem Wait 발생, D는 semSignal발생
- s가 0보다 작아지면 ready가 아닌 blocked로.
- s가 하나 올라갈수록 ready queue에 하나씩 올리기(s가 음수여도 올라간다~!)
- strong 세마포어는 일정 시간이 지나면 다시 실행됨. 기아상태 x

![세마포어를 이용한 Mutual Exclusion](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_1.34.03_PM.png)

세마포어를 이용한 Mutual Exclusion

- p1이 들어오면 semWait때문에 s==0이 되고 critical section을 수행.
- 그 와중에 p2가 들어와서 `semWait`을 조지면 s == -1이 되고 그러면 block됨
- 그러고 p1이 CS끝내면 `semSignal`해서 s의 값을 증가시킴.

![세마포어를 이용하여 보호되는 공유데이터를 접근하는 프로세스들의 수행 과정.](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_80B31C75CAE9-1.jpeg)

세마포어를 이용하여 보호되는 공유데이터를 접근하는 프로세스들의 수행 과정.

- 공유 자원에 접근(Critical Section) → s값 감소 -1
    - 근데 여기서 키가 음수이다??? → 바로 큐에 박아버린다!
- critical section 끝나면  s 값 증가 +1 ⇒ 큐안에 있던 놈 CS할 수 있게함. ⇒ 끝나면 다시 s값 +1...

### Producer/Consumer Problem(생산자, 소비자 문제)

- 일반적인 상태
    - 하나 또는 그 이상의 생산자가 데이터를 생성하고 버퍼에 저장
    - Single 소비자가 버퍼로부터 정보 꺼내옴
    - 생산자나 소비자중 한사람만 버퍼에 접근 가능하다(두명이상 접근 안됨)
- 문제의 요구조건.
    - 생산자나 소비자중 한사람만 버퍼에 접근 가능하다(두명이상 접근 안됨)
    - buffer가 full이라면 생산자는 data 추가 X
    - buffer가 empty라면 소비자는 데이터 가져올 수 X
    - → 이 문제를 세마포어로 풀어볼거야!!
    
- 생산자 소비자 문제에서 무한 버퍼 구조.
    
    ![생산자/소비자 문제에서 무한 버퍼 구조.](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_1.48.02_PM.png)
    
    생산자/소비자 문제에서 무한 버퍼 구조.
    
    - in은 생산자가 data 생성해서 넣을 수 있는 곳
    - out은 소비자가 data를 가져갈 수 있는 곳
    - 파란색 → data있는곳
    - in > out 소비자가 data 갖고 갈 수 있는상태
    - in = out data가 empty인 상황.
    - 항상 in이 앞에 있어야한다!
    
    <무한 버퍼에서 binary semaphore를 이용해서 생산자 소비자 문제를 해결 한 방법 - 부정확한 방법>
    
    ![IMG_CA482B90D582-1.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_CA482B90D582-1.jpeg)
    
    - s : semaphore
        - 생산자는 메세지를 추가하기 전에 Mutual Exception을 위해 세마포어를 감소해서 소비자가 접근 못하게함. (버퍼에 데이터 추가할 때 소비자가 버퍼를 못사용하게 하는 것)
    - n : 메세지개수 (in - out)으로 구함.
        - 생산자는 Critical Section 내에 있는 동안 n의 값을 증가.
        - 증가 이후 n = 1 이 되면, 증가 이전에 n = 0 이었다면 데이터를 버퍼에 추가하기 바로 전에는 버퍼가 비어있었다는 뜻이니까 생산자는 소비자에게 버퍼에 데이터가 채워졌다는 사실을 알려주기 위해 delay값을 증가시킴.
    - delay : semaphore Delay → 동기화...?
        - d=1 소비자에게 버퍼에 데이터가 채워졌다는 사실을 알려준다.
        - d=0 버퍼에 데이터가 없다. → block당해~
    
    ![IMG_FBB66C2278A4-1.jpeg](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/IMG_FBB66C2278A4-1.jpeg)
    
    14번이 9번다음에 실행됐어야하는데 그냥 생산자가 갑자기 들어와버려서 문제가 생긴거다!!!!
    
- 정확한 답
    
    ![Screen Shot 2022-04-14 at 2.12.50 PM.png](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_2.12.50_PM.png)
    
    - consumer에 로컬 변수 m을 줘버림
    - critical section 안에서 m에 n 값을 넣어줌으로써 세마포어가 끝나자마자 다른놈이 끼어들면 그 전의 값을 저장해서 위처럼 혼선을 안 줄 수 있다.
    임계영역에서 나오기 전에 n값을 다른변수에 저장을 해주는거임.
        - 나중에 돌아왔을 때 n값이 producer에 의해 변했더라도 m값으로 판단을 내릴 수 있다.
    - 만약 if 문을 semaphore로 넣어준다면 →
    
- Counting Semaphore를 이용한 방법. → 단순화..
    
    ![Screen Shot 2022-04-14 at 2.52.56 PM.png](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_2.52.56_PM.png)
    
    - 근데 이런 Infinity buffer는 현실적이지가 않다!
    - 그래서 우리는 유한한 공간에서 진행을 해야하고
    - 그것을 위해서 Circular Buffer를 사용해야한다
    
- Circular Buffer(중요해요!)
    
    ![Screen Shot 2022-04-14 at 2.58.08 PM.png](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_2.58.08_PM.png)
    
    ![Screen Shot 2022-04-14 at 3.15.10 PM.png](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_3.15.10_PM.png)
    
    - e=sizeofbuffer 생긴 것이 핵심
    - s, e, n, 총 세 개의 키가 존대

### Semaphore의 구현

- `semWait`와 `semSignal`은 반드시 atomic하게 구현되어야함(다른 애들이 중간에 끼어들면 안됨)
- 그러기 위해서는 hardware와 firmware로 구현
- software는 dekker’s나 peterson’s로 구현하는데 이걸 쓰면 부하가 높아짐
- 대안으로 하드웨어의 지원을 받으면서 mutual exclution구현하면 됨

![Screen Shot 2022-04-14 at 3.20.38 PM.png](Ch05-22S-2%20c305e4b018374946b80edafa0579165f/Screen_Shot_2022-04-14_at_3.20.38_PM.png)

- (a): while로 감싸면서 atomic하게 수행이 가능함
- (b): interrupt를 설정해서 코드 겉에 막고, 허용함으로써 사이에 있는 아이들에서 인터럽트를 일어나지 않게 할 수 있음
    - 인터럽트가 중간에 실행 안 됨으로써 실행 효율성을 떨어뜨리는 문제가 발생

→ (a)가 낫다!!!