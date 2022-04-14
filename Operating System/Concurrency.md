# Concurrency

- Mutual Exclusion: 상호 배제
- Syncronization: 동기화

## Multiple Process

- OS의 메인 주제는 스레드와 프로세스의 관리와 연관
    - Multiprogramming
        - 단 하나의 프로세서에서 여러개의 프로세스들이 동시에 수행되도록
    - Multiprocessing
        - 여러개의 프로세서에서 여러개의 프로세스를 관리하는 것 (오버래핑까지)
    - Distributed Processing
        - 독립적인 분산된 여러개의 컴퓨터들을 상호간에 프로세스를 처리
        

## Concurrency와 연관된 단어

- atomic operation
    - 더이상 분할할 수 없는 단위 & 1개 이상의 inturuction으로 구성된 function들
    - 더이상 쪼갤 수 없으니까 이건 실행하거나 못하거나 둘 중 하나다!
- citical section: 임계 영역
    - 소스코드를 프로세스가 실행하고 있을 때 다른 프로세서가 거기에 접근할 수 없도록
- deadlock: 교착
    - 다른 녀석이 가지고 있는 걸 (메시지, 자원)을 대기하면서 계속 대기하는 상황
        
        ![Screen Shot 2022-04-13 at 8.47.53 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_8.47.53_PM.png)
        
    - 서로 다른 프로세스가 서로의 진행을 기다리면서 있는거~
    - 서로 요구하고 있는 상태
- livelock
    - 2개 이상의 상태가 계속 상황을 바꾸고 있는데 실행된 결과가 없는 것
- mutual exclusion
    - 한 프로세스가 critical section에 접근했을 때는 다른 프로세스는 여기에 접근할 수 없음
- race condition
    - 두 개 이상의 프로세스가 동시에 공유 자원에 접근하려고 할 때 결과적으로 result가 실행 순서에 따라 달라질 수 있음
- starvation: 기아
    - 무한정 대기하고 있는 상태

## Livelock

![Screen Shot 2022-04-13 at 8.53.27 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_8.53.27_PM.png)

- while문 안의 것이 없다면 deadlock
- 동시에 실행하다 보니 while문을 빠져나가지 못하고 계속 돌고 있음

## Race Condition

![Screen Shot 2022-04-13 at 8.55.48 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_8.55.48_PM.png)

- 순서에 따라 결과가 달라진다!
- 경쟁상태

## Mutual Exclusion

- mutual exclusion 제공하는 방법은 여러가지가 있당

### Mutual Exclusion: Software

- single process나 메모리 공유하는 multiprocesser machine에서 실행하고 있음
- 메모리 엑세스 레벨에서  Mutual Exclusion지원
    - 동시에 접근하려고 하면 메모리 관리자가 순서를 정해줌
    - 정해진 순서가 있는게 아니라 상황에 따라 다르다~
    - 하드웨어, OS, PL에서는 support하는 것 없다고 가정
- Dekker algorithm
    - Mutual Exclusion 해결하기 위한 첫번째 알고리즘
    - turn이라는 녀석을 씀
        - 동시에 critical section 들어가기 위해서 어떤 한 녀석이 들어갈 수 있도록
        - turn을 가진 녀석이 들어가게 됨
    - 어떤 녀석이 그 이미 critical section에서 실행중이면 다른 녀석은 busy wait 상태가 됨
        - 끝났는지 계속 확인하며 기다려야 하기 때문에
        - flag[1]이 true면 1번 프로세스가 들어갈 수 있게
    - 첫번째 시도
        
        ![Screen Shot 2022-04-13 at 9.11.51 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_9.11.51_PM.png)
        
        - 프로세스 0번은 turn이 0이 아닐 때 수행. 1번은 turn이 1이 아닐 때 수행
        - 0번 turn 마치면 turn을 1로. 1번 turn 마치면 turn 0으로
    - 그럼 프로세스 1번과 0번의 일하는 양이 다르다면?, process0이 turn을 1로 바꾸기 전에 꺼져버린다면?
    - 두번째 시도
        
        ![Screen Shot 2022-04-13 at 9.25.27 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_9.25.27_PM.png)
        
        - 그래서 flag 사용!
        - flag를 배열로 만들어서 각 프로세스에 flag를 할당. 할당 조건은 다른게 사용 하나가 사용 중이면 나머지는 무한루프 돌고 있어서 못 들어가용~
        - 여기서 상호 배제가 안되는 경우가 있다!
            - 프로세스 0이 수행되고 false여서 while문 건너뛰었을 때 인터리빙이나 스케줄링이 돼서 프로세스 1번으로 넘어간다?
            - false false이기 때문에 둘 다 critical section 들어와벌임!@!@!@!!
    - 세번째 시도
        
        ![Screen Shot 2022-04-13 at 9.25.51 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_9.25.51_PM.png)
        
        - 대기하도록 하는 while 실행 전에 나의 flag를 true로 한다면?
        - 프로세스 0과 1이 수행될 때 둘 다 true로 설정해버려서 while에서 둘 다 걸려버려서 waiting된다 → deadlock에 걸려버린당~
    - 네번째 시도
        - 대기하도록 하는 while 안에 내 flag를 잠시 false로 만들어줘버리면 다른 프로세스가 while을 쏙 지나가서 deadlock 피할 수 있당!
        - 하지만 3번째와 동인할 문제가 생김 프로세스 1,2가 동시에 실행되면 둘 다 while문 오지게 돌고 있음 → false true 왔다갔다 → 이거시 livelock이다!@!@!@!@!!
    - 올바른 알고리즘
        
        ![Screen Shot 2022-04-13 at 9.36.23 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-13_at_9.36.23_PM.png)
        
        - 앞에서 서로 먼저해~ 하니까 동시에 진행되니까 누가 먼저 양보해라!를 정해준다
        - flag + turn을 해버리자!!!!!
        - while 둘 다 flag true여서 실행돼도 turn값이 있으니 자기 turn 아닌 친구는 자기 flag false로 바꾸고 기다리고 turn으로 돌리는 다른 while문 돌리면서 있어버림.
        - 다른 친구가 critical section 실행하고 나서 그 뒤에 turn을 바꿔주면 while문 돌던 친구가 나와버림
    - 두개로 나눠버리면
        
        ![Screen Shot 2022-04-14 at 1.11.32 AM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.11.32_AM.png)
        
        - 피터슨 알고리즘!
        - 첫번째 while문 들어오면 true 박아버리고 turn을 상대로 줘버림!
        - race condition 상태가 되는데 두번째 while에서 flag가 true이고 turn이 상대꺼면 while문 반복하며 대기해버림!
        

## principle of Concurrency

- Interleaving overlapping
    - interleaving: 멀티 프로그래밍에서 프로세스가 교환을 하면서 하는거(uniprocesser)
    - overlapping: 동시간에 수행(multiprocessor)
- 프로세스 진행 스피드를 예상을 할 수가 없음. 왜?
    - 다른 프로세스의 활동에따라 달라짐
    - OS가 인터럽트 핸들을 어떻게 하냐에 따라
    - OS의 schedule때문에

## Difficulties of Concurrency

- 병행성 제공하다보면 글로벌 리소스 공유할 때 문제가 생길 수 있음
    - 순서가 엇갈리면 큰일나~
- OS가 리소스 할당하는데 있어서 어렵다
    - 잘못하면 deadlock
- 잘못된 프로세싱으로 인해서 오류가 간헐적으로 발생하면 찾기 어려움

## Race Condition

- 많은 프로세스들이나 스레드들이 데이터를 읽거나 작성할 때 일어남
- 마지막 결과는 순서에 의해서 결정됨
    - race의 loser(늦게 들어온 녀석)이 결과값을 결정한다!

### OS가 고려해야할 사항

- 다양한 프로세스의 track에서 pcb를 통해 유지
- 리소스 할당하고 회수하는 것과 동시에 엑세스하는 걸 관리
- 피지컬 리소스와 데이터를 보호
- 프로세스의 기능과 결과는 다른 동시에 수행 중인 프로세스로부터 영향을 받지 않고 독립적으로 진행돼야함

### Resource Competition

- 경쟁 왜 발생?
    - concurrent process들이 같은 자원 사용하기 위해 경쟁하면 충돌이 발생하는데 그때 문제가 생김
- 3가지 문제 발생
    - 상호 배제가 필요해짐
    - 상호배제때문에
        - deadlock
        - starvation발생

### Mutual Exclusion

![Screen Shot 2022-04-14 at 1.27.00 AM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.27.00_AM.png)

- 이렇게 critical section 보호해줄 수 있따!

### 프로세스 간의 “Share”를 통한 Cooperation

- 프로세스가 다른 프로세스에 대해 명시적으로 인식하지 않고도 수행이 상호작용하도록 해야함
- 프로세스가 다른 애들을 참조하지 않더라도 사용할 수 있고, 업데이트 할 수 있어야한다
- 프로세스는 공유된 자원이 잘 관리되도록 협력해야함
- control mechanism을 통해 접근함으로서 일관성을 유지하도록 보장
- mutual exclusion, deadlock, starvation같은 애들이 생겨버림!

### 프로세스 간의 “Communication”를 통한 Cooperation

- share는 서로를 모르지만 communication(통신)을 할 때는 서로가 누군지 안다!
- 특정 목적을 가지고 묶여있다~
- commmunication을 통해 다양한 활동(synchronize, coordinate,등..)을 한다~
- 특정 메시지들로 구성해서 사용한다?
- 상호간에 메시지를 주고받을 때 아무것도 공유하지 않는다
    - mutual exclusion필요 없음
    - deadlock, starvation 발생(쟤가 메시지 주겠네? 기다려야징, 1,2번만 서로 소통하고 3번은 왕따 ㅠㅠ)
    

### Mutual Exclusion의 요구사항

- 특정한 리소스를 쓰기 위해서는 상호 배제가 꼭 필요하다
    - 그러기 위해서는 하나의 프로세스만 돌아갈 수 있는 critical section이 있어야한다
- non-critical section에서 종료된 프로세스는 다른 프로세스들한테 영향을 끼쳐선 안된다
    - == 다른 프로세스들은 크리티컬 섹션에 들어갈 수 있어야함
- 한정없이 기다리게 해선 안된다
    - == 교착, 기아 피해야한다
- 크리티컬 섹션에 아무 프로세스도 없으면 어떤 프로세스가 들어가려고 하면 바로 들어갈 수 있어야한다
- 프로세스의 개수나 스피드에 대한 가정은 없다고 생각
- 크리티컬 섹션에 들어간 친구들은 특정 시간까지만 있게 하고 나오게 만들어야함

### 하드웨어 지원으로 Mutual Exclusion

- Interrupt disabling
    - 유니프로세스 시스템에서 프로세스는 오버래핑을 할 수 없다!(interleaving만 가능)
    - 프로세스가 OS를 호출하거나 interrupt발생 안하면 계속 수행이 가능함
    - 인터럽트만 disable하다면 mutual exclusion할 수 있다
    - 단점:
        - 인터리빙 제안하니까 효율성이 많이 떨어진당
        - 멀티프로세서에서는 사용할 수 없음
            - 다른 쪽에서는 돌아가고 있으니깐
- 특별한 명령어로!
    - compare & swap Instruction
        
        ![Screen Shot 2022-04-14 at 2.41.33 AM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_2.41.33_AM.png)
        
        - 메모리에 저장된 값과 데스트의 값이 같다면 새로운 값으로 변경하겠다
        - 기존에 저장된 값을 oldval로 옮기고 얘랑 테스트를 비교.
        - 같으면 word에 넣어버림
        - atomically하게 수행됨 → 수행되는 동안 interrupt 발생할 수 없음
            
            ![Screen Shot 2022-04-14 at 2.43.37 AM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_2.43.37_AM.png)
            
            b는 그냥 바꾼다 서로!
            
    - 특별한 명령어(special machine instruction 사용의 장점
        - 인터럽트를 사용했을 때는 멀티플 프로세스에서는 사용할 수 없는데
        - 변수값에 따라 어디에 있든 상호배제를 적용할 수 있음
        - 간단하고 검증 위움
        - multiple critical sections 사용 가능
            - 각각의 critical section에 대해서 각자의 variable만들어줘버림(키(bolt)를 하나씩 만들어줘버림
    - 단점
        - while문 오지게 돌아서 Busy waiting 상태가 됨 → process time 소비
        - starvation 가능
            - 프로세스가 떠날 때 하나 이상이 기다리고 있으면 기다린 순서랑 상관없이 아무거나 실행됨
        - deadlock 가능
            - p1이 자원 a를 사용하는데 우선순위가 높은 p2가 실행됐다고 하면 p1이 사용하고 있는 a를 요청해버리면 제어를 p2가 가지고 있어서 p1이 양보를 못해서 deadlock 쌉가능
        

## Concurrency Mechanisms

- semaphore
    - counting semaphore: 값을 3 이상도 가질 수 있음
    - binary semaphore: 값을 0 or 1만
- Binary semaphore
- Mutex
    - binary semaphore와 비슷하지만 lock을 건 애만 unlock해줄 수 있음
    - cf) Binary semaphore은 아무나 풀어줄 수 있음
- Monitor
- Moiaboxes/Messages

## Semaphore(중요합니다!)

- integer 변수를 가질 수 있는 변수라고 볼 수 있음
- 여기에 연관된 opreation 3개가 정의돼있음
    - semaphore 0이 아닌 int 밸류값으로 초기화해주는 애
    - semWait: decrement하는애. 음수가 되면 블락시킴
    - semsignal: increment하는 애
- 결과
    - decrement를 하기 전까지는 block될지 아닐지 알아볼 수없음
    - process가 semaphore을 increment하고 난 뒤에는 2개가 동시에 수행중일 쉬 있음. 그때 어떤 프로세스가 즉시 계속해서 수행될지는 알 수 없음
    - 세마포어에 신호를 보내면 다른 프로세스가 대기중인지 알 수 없어서 차단되지 않은 수의 프로세스는 없거나 1개 ?????????????????

### semaphore(노말버전)(general semaphore)

- 세마포어는 구조체임
    - count와 block돼있을 때 조작하는 queue가 있음
- semWait
    - count- -
    - count가 0보다 작으면 객체 queue에 넣어버림. block
- semSignal
    - count++
    - count ≤0이면 큐에 있는 녀석 하나는 올리게 됨
    - 걔를 ready list로 옮김

### Binary semaphore

- 값을 0, 1만 가질 수 있음
- 큐 있는거 같음
- semWaitB
    - val == 1이면 val =0
    - val == 0이면 큐에 넣어버림 → block
        - val==0의 의미? 누군가 수행 중이다~
- semSignalB
    - queue가 비어있으면 value를 1로 만들어버림
        - 무슨 의미? 사용하고 있는 애 없다!
    - queue 안 비어있으면
        - queue에서 프로세스를 지우고 ready 리스트에 넣음
- mutex와의 차이점: 뮤텍스는 0으로 만든 애만 1로 만들 수 있다!

### Strong / Weak Semaphore

- Strong Semaphore
    - 순서가 정해져있어서 큐에 있는 애를 깨우는 순서가 있는 거 (FIFO → 가장 오래된 애가 먼저 나옴)
    - 기아가 발생할 수 없다
- Weak Semaphore
    - FIFO가 아니라 우선순위나 랜덤이나 뭐 그렁게 나온다
    - 순서가 없으니까 기아가 발생할 수도 있다
    

### Strong Semaphore

![Screen Shot 2022-04-14 at 1.28.52 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.28.52_PM.png)

- A, B, C는 sem Wait 밝생, D는 semSignal 발생
- s가 0보다 작아지면 ready가 아닌 blocked로.
- s가 하나 올라갈수록 ready queue에 하나씩 올리기
- strong 세마포어는 일정 시간이 지나면 다시 실행됨. 기아상태 x

![Screen Shot 2022-04-14 at 1.34.03 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.34.03_PM.png)

- p1이 들어오면 semWait때문에 s==0이 되고 critical section을 수행.
- 그 와중에 p2가 들어오면 semWait을 조지면 s == -1이 되고 그러면 block됨

![Screen Shot 2022-04-14 at 1.40.43 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.40.43_PM.png)

- 쉽져?
- 공유 자원에 접근? 키 -1
    - 근데 여기서로다가 키가 음수이다??? → 바로 큐에 넣어버린다!
- critical section 끝나면  키 +1

### Producer/Consumer Problem

- 일반적인 상태
    - 한개 이상의 프로듀서가 데이터를 생성하고 버퍼에 저장
    - 한개 consumer가 버퍼로부터 정보 꺼내옴
    - 한개 producer나 consumer이 동 시간대에 1명만 버퍼에 접근 가능하다
- 문제점
    - buffer가 full이라면 producer는 data 추가 x
    - buffer가 empty라면 consumer는 데이터 가져올 수 x
    - → 요것들을 만족시켜줘야함
- infinite buffer 예시
    
    ![Screen Shot 2022-04-14 at 1.48.02 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.48.02_PM.png)
    
    - in은 무조건 out보다 앞에 있어야함~
    
    ![Screen Shot 2022-04-14 at 1.48.54 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_1.48.54_PM.png)
    
    - 여기서 n은 in - out으로 메시지 량
    - semWait → s = 1 → 0
    - append → n=0 → 1
    - if 문 실행 → delay = 0 → 1
    - semSignalB → s = 0 → 1
    
    consumer
    
    - semwait으로 delay= 1 → 0
    - semwait(s) : s = 1 → 0
    - take : ㅁㅔ시지 가지고 옴 (n- -)
    - semsignal (s) : s = 0 → 1
    - consume: 메시지 소비
    - n == 0이면 메시지 덜이상 소비하면 안되니까 semwaitB(delay)
    
    ![Screen Shot 2022-04-14 at 2.04.37 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_2.04.37_PM.png)
    
    - 정확한 답
        
        ![Screen Shot 2022-04-14 at 2.12.50 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_2.12.50_PM.png)
        
        - consumer에 로컬 변수 m을 줘버림
        - critical section 안에서 m에 n 값을 넣어버리면 된다!
            - 나중에 돌아왔을 때 n값이 producer에 의해 변했더라도 m값으로 판단을 내릴 수 있다.
    - binary semphore
        
        ![Screen Shot 2022-04-14 at 2.52.56 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_2.52.56_PM.png)
        
        - 근데 이런 Infinity buffer는 현실적이지가 않다!
        - 그래서 우리는 유한한 공간에서 진행을 해야하고
        - 그것을 위해서 circular buffer를 사용해야한다
- Circular Buffer(중요해요!)
    
    ![Screen Shot 2022-04-14 at 2.58.08 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_2.58.08_PM.png)
    
    ![Screen Shot 2022-04-14 at 3.15.10 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_3.15.10_PM.png)
    
    - e=sizeofbuffer 생긴 것이 핵심
    - s, e, n, 총 세 개의 키가 존대

### Semaphore의 구현

- semWait와 semSignal은 반드시 atomic하게 구현되어야함(다른 애들이 중간에 끼어들면 안됨)
- 그러기 위해서는 hardware와 firmware로 구현
- software는 dekker’s나 peterson’s로 구현하는데 이걸 쓰면 부하가 높아짐
- 대안으로 하드웨어의 지원을 받으면서 mutual exclution구현하면 됨

![Screen Shot 2022-04-14 at 3.20.38 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_3.20.38_PM.png)

- (a): while로 감싸면서 atomic하게 수행이 가능함
- (b): interrupt를 설정해서 코드 겉에 막고, 허용함으로써 사이에 있는 아이들에서 인터럽트를 일어나지 않게 할 수 있음
    - 인터럽트가 중간에 실행 안 됨으로써 실행 효율성을 떨어뜨리는 문제가 발생

→ (a)가 낫다!!!

## Monitors

- programming language에서 제공하는 구조체
- semaphore와 동등한 기능 제공
- 하나 이상의 프로시저와 초기화 시퀀스, 로컬 변수로 구성됨

### 모니터의 특징

- 모니터에 있는 local data 변수는 모니터 안에 있는 모니터 프로시저만 접근할 수 있음
    
    → 모니터 안에 있는 녀석들만 접근할 수 있다
    
- 프로세스는 모니터 안의 한 개의 프로시저를 호출함으로써 모니터 안에 들어갈 수 있음
- 특정 시간에 모니터 안에서 한 개의 프로세스만 실행함으로서 mutual exclusion 진행

### Synchronization(동기화)

- 모니터에서도 동기화를 제공함
    - 어떻게? condition variable(조건 변수)를 활용
        - 모니터 안에서만 접근 가능
        - condition variable은 특별한 데이터 타입의 형식.
        - condition variable은 두가지 함수로 접근 가능
            - cwait(c)
                - condition c일 때 실행된 프로세스를 중지
            - csignal(c)
                - condition c일때 block된 친구를 불러서 실행
            - 세마포어와 다른점?
                - 세마포어는 시그널 값을 바꾸면서 영향을 미치지만
                - 모니터는 csignal에 던졌을 때 해당 조건 queue에 없다면 아무것도 안함
                - 
- 모니터의 구조
    
    ![Screen Shot 2022-04-14 at 3.42.35 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_3.42.35_PM.png)
    
    - 프로세스가 모니터에 들어올 때 하나씩 큐에 대기하고 있던 애들이 들어옴
    - condition c1에 대해서 cwait가 발생해야한다면 해당 조건에 걸려있는
    - 어떤 녀석을 깨워야하면 csignal을 수행.
        - csignal을 호출한 애는 urgent queue에 넣고 원래 urgent queue에 있던 애를 실행
        - 뭔가가 끝나게 되면 urgent queue에 있는 애들에게 우선순위 부여해서 먼저 들어갈 수 있도록
        - urgent queue가 queue of entering보다 우선순위가 높다...!
- 유한 버퍼에서의 생성자 소비자 문제 해결
    
    ![Screen Shot 2022-04-14 at 3.55.55 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_3.55.55_PM.png)
    
    - 위에 코드를 알아보자
        - append
            - 카운트(버퍼에 있는 아이템 개수)가 가득 찼으면 → cwait(notfull)
                - 왜냐면 오버플로우 피할려고!
            - 버퍼의 새로운 위치에 생성된 x 값 삽입
            - nextin(다음에 시행될 포인터 값) 값 하나 증가하고 circular니까 %N해주기
            - 아이템 하나 더 들어갔으니까 count 하나 올려주기
            - csignal(notenpty)를 호출해서 이거 대기하고 잇는 애 있으면 큐에서 불러주기~
        - take
            - count가 0이라는 것은 버퍼에 아무것도 없다는 것이니까 cwait(notempty) → notempty상태로 대기
            - 버퍼에서 하나 가져와서 받아오기
            - nextout 하나 늘려주기
            - 아이템 뺐으니까 count - -
            - csignal(notfull) 호출해서 notfull 시그널 기다리고 있던 애가 있으면 실행되도록~
    - responsibility(책임소재)구별 가능
        - Mutual exclusion이 모니터를 통해 자동으로 제공(한 시점에 한 프로세스만 들어가게 되므로)
            - 프로그래머는 동기화 위치만 어디에 두면 될지 고민하면 됨
        - 반면 세마포어는 Mutual exclusion와 syncronization 둘 다 프로그래머가 해줘야함
        - 그래서 두개 중 밑의 코드를 보면 세마포어 코드와 다르게 임계영역 설정이나 싱크 맞추는 게 없다
            - 왜? 모니터 자체가 프로세서 하나만 움직이게 하기 때문에!
    - 장점
        - 모든 동기화가 모니터 안에 들어가있음!
            - 반대로 세마포어의 경우에는 임계영역 설정같은 것들을 정확하게 해줘야하는데 안하면 어디서 해야할지 어리버리타버림
            - 버그 찾거나 동기화가 정확하게 된 것 쉽게 검증 가능

## Message Passing

- 프로세스가 서로 상호작용을 할 때의 두가지 요구사항
    - 동기화(Synchronization)
        - 상호배제를 하기 위해 필요
    - 통신(Communicaiton)
        - 정보를 교환하기 위해 필요
- 두가지 요구사항을 해결해줄 수 있는 Message Passing 정말 좋은 친구다!@!@!@!@!
    - distributed에서도 적용 가능하고 다른 것들은 당연히 가능~!!~~!~!~!~!
- 메시지 패싱에서는 두가지 primitive제공
    - send(destination, message)
        - 받는사람, 메시지
    - receive(source, message)
        - 보낸 사람, 메시지
- 한 프로세스가 정보를 보내려고 할 때는 메시지를 보내게 되고, 수신자에게 보냄
- 프로세스가 받으려고 할 때는 메시지와 네,...
- Message system의 디자인 특성
    
    ![Screen Shot 2022-04-14 at 5.15.35 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_5.15.35_PM.png)
    
    - 저기서 뭘 선택할지 골라라~~~~
    - 대충 무슨 느낌인지 알잖아..
    
    ### Synchronization
    
    - 두 프로세스 간 통신은 동기화를 의미한다~
        - 리시버의 경우는 다른 애가 메시지 보내줘야지만 실행될 수 있음~
    - 리시브 primitive에서 두 가지의 가능성
        - 메시지가 없을 때
            - 메시지가 올 때까지 blocking해서 기다리거나 수신되는 것을 포기하고 프로세스를 꼐속 실행
        - 메시지가 있을 때
            - 수신하고 계속 실행
    - send primitive
        - blocking
            - 수신자가 수신할 때까지 block
        - non blocking
            - 수신자 안 받아? 응~ 포기하고 다른거 하면 그만이야~
    
    ### Blocking send, Blocking receive
    
    - sender와 receiver 둘 다 메시지 전달될 대까지 block 유지
    - 프로세스 사이에서 엄격하게 동기화가 제공되고 있음
    - 랑데뷰라고 부름
    
    ### Nonblocking Send
    
    - Nonblocking Send, blocking receive
        - 가장 유용하게 사용되는 조합이당
        - sender는 계속 보내고 receiver는 block된당
        - nonblocking send는 concurrent programming에서 많이 사용됨
            - 프린팅 요청했을 때 기다릴 필요가 없다. 끝나면 신호 받으면 됨
        - receive는 대기하고 (block)있다가 듣고 수행하기 대문에 많이 사용되지요
        - 단점
            - message가 lost되면 계속 block됨
        - 한 프로세스가 하나 이상의 메시지를 다른 대상에게 뿌릴 수 있음
    - Nonblocking Send, nonblocking receive
        - 이때는 둘 다 기다리지 않고 자기 할 거 해버린다~
        - 언제 좋아?
            - 다수의 녀석이 한 녀석한테 주는데 (N:1)상황일 때 다른 애의 message 받아서 실행하면 됨~
        - 문제점
            - sender가 보낸 것을 잃어버림 ㅜㅜㅜ
            - receive가 받을 때까지 대기하지 않기 때문에 많이 보낼 수 있음
                - 감당 안돼~

## Addressing

- 주소 지정
- 두가지 방식
    - 직접 방식
        - 목적지에게 다이렉트로
    - 간접 방식
        - 우편함에다가 보내놓고 알아서 가져가라...

### Direct Addressing(직접 방식)

- send는 destination process의 identifier(주소)를 포함해야함
- receive는 두가지 방시으로 가능
    - 어떤 녀석으로 받을지 명시해두고 그 녀석으로 온 것만 수행하겠다
    - Implicit addressing
        - ex) 프린터 서버. 누구든지 보낼 수 있고, return 하는 동작이 포함되어 있다.

### Indirect Addressing(간접 주소 방식)

- 메시지가 큐로 구성된 공유 데이터 스트럭쳐로 보내지게 되는데 이걸 메일박스라고 부름
- 그럼 다른 프로세스가 메일 박스에 와서 메시지 가져감
- sender와 receiver가 서로 모른다.(decoupling)
- 유연하게 사용할 수 있다

![Screen Shot 2022-04-14 at 6.25.33 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_6.25.33_PM.png)

- (a): 특정 목적으로 사용되는 1:1
- (b): 클라이언트 서버같은 곳에서 사용되는 N:1(port)
- (c): broadcast에서 사용되는 1:N
- (d): N:N

### 일반적인 메시지 포맷

![Screen Shot 2022-04-14 at 6.28.35 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_6.28.35_PM.png)

- Header
    - 메시지 타입
    - 목적지
    - 출발지
    - 길이 어느정도?
    - control Information
        - sequence number, priority와 같은 이 메시지에 필요한 컨트롤 정보
- Body
    - 메시지의 내용
    

### Queueing Descipline(큐잉 정책)

- 선입선출~
    - 긴급(urgent)한 문제를 잘 처리하지 못함
- 다른 방법
    - 메시지에 우선순위 주기
    - 메시지 열어보고 선별

- 메시지를 이용해서 상호배제를 구현
    
    ![Screen Shot 2022-04-14 at 6.31.39 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_6.31.39_PM.png)
    
    - 가정
        - nonblocking send & blocking receive
        - mailbox 초기화하면서 1개 메시지만 가지고 내용없음
    - 메시지가 하나 있고,
    - receive로 메시지 하나 받음
    - critical section 실행
    - send에 메시지 하나 보냄
    - 다른 애들이 메시지 넣어줄 때까지 대기
    
- 메시지를 이용해서 생성자소비자문제 해결(이거 살짝 모르겠는데)
    
    ![Screen Shot 2022-04-14 at 6.34.40 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_6.34.40_PM.png)
    
    - 두개의 메일박스 사용
        - mayproduce
        - mayconsume
    - producer
        - 처음에 초기화할 때 buffer 크기만큼 nill값을 mayproduce로 보내버림
        - 메시지 생성해서 사용하는 mail box(mayconsume)에 넣어줌
    - consumer
        - mayconsume으로 뭔가가 오면 받음
        - 받은 메시지 소비
        - mayproduce에 send해서 하나 끝났다고 알려줌

## Readers/Writers Problems

- 많은 프로세스들이 데이터 영역을 공유
    - 일부 프로세스들(reader)은 읽기만 하고 일부 프로세스들(writer)은 쓰기만 함
- 반드시 만족되어야하는 조건
    - reader들은 파일을 동시에 읽을 수 있어야함
    - 한 시점에는 한 개의 writer만 쓸 수 있어야함
    - writer가 공유자원(file)을 쓰고 있을 때, reader들은 그 자원을 읽을 수 없다

### 생성자 소비자문제: 세마포어를 이용하고 reader가 priority 우선

![Screen Shot 2022-04-14 at 8.10.11 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_8.10.11_PM.png)

- x: mutual exclution 구현하기 위해 사용
- wsem: 상호간에 lock을 걸 때 사용
- semwait(x) → reader, writer 모두에게 기다리라고
- semwait(wsem) → writer에게 기다리라고
- reader
    - semwait(x)로 모두에게 사용하고 있다는 것을 알림
    - readcount ++
    - readcount == 1 확인해서 맞으면 semwait(wsem)으로 write 세마포어들 다 wait하도록
    - semSignal(x)로 reader들 더 들어올 수 있도록
    - 작업 진행
    - semwait(x)으로 모두 기다리게
    - readcount - - (사용 끝났당)
    - readcount == 0 이면 reader 없다는 거니까 semsignal(wsem)으로 writer들 들어올 수 있게
    - semSignal(x)로 모두에게 사용 끝났다~-
- 많은 reader들이 들어오면?
    - readercount 계속 올라감
    - 그럼 writer들이 readercount 가 0이 될 때까지 계속 일을 못하고 기다려야함
    - writer semaphore들이 배고파서 기아(starvation)상태됨

### 생성자 소비자문제: 세마포어를 이용하고 writer가 priority 우선

![Screen Shot 2022-04-14 at 8.22.42 PM.png](Concurrenc%20a69fb/Screen_Shot_2022-04-14_at_8.22.42_PM.png)

- 세마포어와 전역변수 추가됨
    - writecount:
    - rsem :  block이 됐을 때 첫번째 reader만 만 rsem 큐에서 block
    - z: 나머지들 넣어버리기 위해
    - y : writecount의 mutual exclusion 보장하기 위해 생김
- reader
    - semwait(z)를 해서 z: 1→ 0
    - semwait(rsem)을 통해
    - semwait(x)를 통해
    - readcount ++
    - readcount ==1이면 semwait(wsem)을 통해 읽고 있다고 wsem들이 못 들어오도록
    - semSignal(x)를 통해 풀어줌
    - semsignal(rsem)을 통해 풀어줌
    - semsignal(z)를 통해 풀어줌
    - 9:00