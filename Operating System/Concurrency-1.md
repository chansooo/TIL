# Ch05-22S-1

병행성: 상호배제와 동기화

os에서 제일 중요한 부분

### 학습목표

1. mutal exclusion(상호배제)
2. semaphore
3. monitor
4. producer/consumer problem
5. readers/writers problem

### Multiple Process

- OS설계에서 중요한 주제는 Process, Thread를 관리하는 것.
    - Multiprogramming
        - Uniprocessor System에 Multiple Processes
        - 하나의 프로세서(CPU)에 여러개 프로세스 동작
        - Interleaving(인터리빙), 오버래핑 불가
    - Multiprocessing
        - Multiprocessor에 Multiple Process
        - 여러개 프로세서에 여러개 프로세스 동작
        - 오버래핑 가능
    - Distributed Processing
        - 독립적인 다수의 컴퓨터를 모아서 Multiple Process를 처리.
        - Ex) Cluster시스템.

### Concurrency와 관련있는 주요용어

- Atomic Operation(원자적 연산)
    - 하나 또는 여러개의 instruction들로 구성된 함수 또는 액션. 
    더이상 쪼개기(distributed) 불가한 단위(원자는 더이상 못쪼갬)
    - 이놈이 실행될때 다른 Process들이 이놈의 수행상태를 볼 수 없고 interrupt도 걸 수 없다.
    - Yes or Not : 이 명령어(이 단위)들은 모두 실행되거나 하나도 실행되지 않음이 보장되어있음.
    - 원자성 → concurrent process들에게 isolation(고립)을 보장한다.
- Critical section(임계영역)
    - Shared된 Resource에 접근하는 프로세스 내부의 코드 영역.
    - 다른 프로세스가 이 영역에 있을 때, Critical section은 수행 할 수 없다.
- Deadlock(교착상태) → 서로 요구하고 있는 상태.
    - 두개 이상의 프로세스들이 진행 할 수 없는 상태.
    - 다른 프로세스가 쓰고 있는 공유 자원을 대기하거나, 메세지 대기..
    - 예를 들어 P1이 R1, P2가 R2를 쓰고 있다가 P2가 R1을 쓰고 싶다. 
    그러면 P2는 P1이 R1을 다 쓸 때까지 block된다.
    근데 P1이 R2가 필요하다고 요청하면 P1도 block이 되어 서로 교착상태에 빠진다.
- Livelock(라이브락)
    - 두개 이상의 프로세스들이 열심히 다른 프로세스의 상태변화에 따라 자신의 상태들을 변화시키고 있지만 실질적으로 진행은 안되는 상태.
    - 예시
        
        ![IMG_833EA57D9118-1.jpeg](Ch05-22S-1%2058bda29b5eba40e3add0f96eed1088c4/IMG_833EA57D9118-1.jpeg)
        
        → 서로 계속 lock을 풀고 걸고 해서 Live lock 상태에 빠지는 것.
        
- Mutual Exclusion(상호 배제) - Critical section과 연관되어 있다.
    - 한 프로세스가 Shared Resouce를 접근하는 Critical Section을 수행하고 있다면 다른 프로세스들은 Shared Resource를 접근하는 Critical Section의 코드를 수행 할 수 없다는 조건.
- Race Condition(경쟁상태)
    - 두개 이상의 프로세스 or 쓰레드가 Shared Resource에 동시에 접근(Read and Write)하려는 상태.
    - 최종 수행 결과는 프로세스들의 상대적인 수행 순서에 따라 달라질 수 있다.
    - 결국 진 프로세스(마지막으로 접근한)가 수행한 결과가 최종으로 저장됨.
    - 예시
        
        ![IMG_CFCA940F3192-1.jpeg](Ch05-22S-1%2058bda29b5eba40e3add0f96eed1088c4/IMG_CFCA940F3192-1.jpeg)
        
- Starvation(기아)
    - 특정 프로세스가 수행 가능한 상태임에도 무한정 대기상태. 스케쥴링이 안되는 경우.
    - 오랫동안 수행 못하는 상태.
    - 자기 앞에 우선순위가 계속 높은 놈이 오게되면 후순위로 밀려나서 기아상태로 빠지게 됨.

### Mutual Exclusion(상호배제) : 소프트웨어적으로 제공하는 방식

- 싱글 프로세서나 멀티프로세서(main memory를 공유하는)에서 Concurrent Process를 제공하기 위해 구현하여 제공하고 있음.
- 메모리 접근(Access) 레벨에서 기본적인 Mutual Exclusion을 구현하여 제공하고 있음.
    - 같은 location에 동시에 Access(Read or Writing)를 할 때 메모리 관리자(Arbiter)가 제어해서 순서화한다. 순서화해서 제공하지만 미리 정해진 것이 아니라 그 사항에 따라 메모리 관리자가 순서를 정해준다.
    - Hardware, OS, PL에서는 software적으로 제공하는 건 없다고 가정한다.
- Dekker’s Algorithm
    - Mutual Exclusion을 위해 만들어진 최초의 알고리즘.(2개의 process)
    - 같은 시점에 2개의 프로세스가 Critical section에 동시에 진입하려고 할 경우, `turn`을 사용하여 오직 하나의 프로세스만 접근 할 수 있게한다. (`turn`은 전역변수다)
    - `turn`을 가진 프로세스가 Critical section에 들어가게 된다.
    - 만약 P1이 이미 Critical section에 접근하고 있다면 P2는 
    `busy wait` 상태가 되고 P1이 exit할때까지 기다린다. 
    여기서 busy wait는 P1이 나왔는지 안나왔는지 계속 확인해야 되기 때문에 `busy wait`라고 부른다.
    - `busy wait`
        - `flag[0]` , `flag[1]`, `turn`을 사용.
        - `flag[0]` : `flag[0]`이 `true`가 됐을 때는 프로세스 0번이 들어가고 싶다.
        - `flag[1]` : `flag[1]`이 `true`가 됐을 때는 프로세스 1번이 들어가고 싶다.
        - `turn` : 어떤 프로세스가 우선순위를 가지고 들어갈 수 있냐

### <First Attempt>

```c
/* PROCESS 0 */
프로세스 0 수행문..

while(turn != 0)
	/*do nothing/* ;
turn이 0이 아니면 여기서 대기.

/* critical section*/ 수행.

turn = 1;
/*critical section 수행 후 
turn을 1로 바꿔줌./*
```

```c
/*PROCESS 1*/
프로세스 1 수행문..

while(turn != 1)
	/*do nothing/* ;
turn이 1이 아니면 여기서 대기

/* critical section*/

turn = 0;
/*critical section 수행 후 
turn을 0로 바꿔줌./*
```

→ First Attempt 문제점 :  **Starvation**

1. 만약 P0가 한시간 한번만 critical section을 수행하고 P1이 한시간에 100번을 수행해야 한다면 P1이 critical section을 수행하고 `turn`을 0으로 넘겨주면 P0는 한시간 뒤에 `turn`을 1로 넘겨준다. 그래서 P1은 100번을 수행해야 하지만 P0때문에 1시간에 한번밖에 수행을 하지 못한다.
2. 만약 P0가 어떠한 오류로 종료되게 되면 `turn`은 0인채로 종료돼서 영영 P1은 `turn`을 1로 받지 못한다.
P1은 Starvation(기아)상태에 빠지게 된다.

### <Second attempt>

```c
/* PROCESS 0 */
*
*
while (flag[1])
	/*do nothing*/
flag[0] = true;
/* flag[1]이 false면 P0가 critical
section에 들어가겠다. */

/*critical section*/;

flag[0] = false;
```

```c
/*PROCESS 1*/
*
*
while (flag[0])
	/*do nothing*/
flag[1] = true;
/* flag[0]이 false면 P0가 critical
section에 들어가겠다. */

/*critical section*/;

flag[1] = false;
```

First Attempt 문제 해결 :  먼저 Second attempt에서는 각 프로세스가 critical section을 다 쓰면 `false`로 돌려놓기 때문에 1시간에 1번, 1시간에 100번 쓰는 문제는 해결된다.(지연되는 시간)

flag[0] → p0가 들어가겠다 

Second attempt의 문제점 : 심각한 오류! → **Mutual exception(상호배제)가 되지 않는 경우** 발생. 

1. 먼저 `flag[0]`과 `flag[1]`은 `false`로 초기화 되어있다고 가정.
2. P0가 while문 통과해서 true를 만나기전에 P1으로 interleaving or 스케쥴링 돼서 넘어감.
3. 그래서 P1도 while문 통과하고 true를 만나기 전에 또 P0로 넘어가면
4. P0는 while문 다음 ~~`flag[0] = true`~~, critical section까지 수행하다가 넘어가면
5. P1도 while문 다음 ~~`flag[1] = true`~~, critical section까지 수행. 그러면 P0,P1둘다 CS를실행!!!!
6. 결국 둘다 임계영역으로 들어오게 된다. → Mutual Exclusion 수행 X 두놈이 같이 critical section으로 들어온다.

### <Third attempt>

```c
/* PROCESS 0 */
*
*
flag[0] = true;
while (flag[1])
	/*do nothing*/
/* flag[1]이 false면 P0가 critical
section에 들어가겠다. */

/*critical section*/;

flag[0] = false;
```

```c
/*PROCESS 1*/
*
*
flag[1] = true;
while (flag[0])
	/*do nothing*/
/* flag[0]이 false면 P0가 critical
section에 들어가겠다. */

/*critical section*/;

flag[1] = false;
```

Second Attempt 문제점인 mutual exception(상호배제) 안됐던 문제를 해결.

Thrid Attempt 문제 : **Deadlock**

1. P0에서 `flag[0]= true`만 수행하고 P1으로 넘어간다.
2. P1에서 `flag[1] = true`만 수행하고 P0로 넘어간다.
3. `flag[0],[1]` 모두 `true`인 상태가 돼서 while문에서 둘다 계속 기다리게 된다.
4. Deadlock 문제 발생!!!!!!

### <Fourth Attempt>

```c
/* PROCESS 0 */
*
*
flag[0] = true;
while (flag[1]){
	flag[0] = false;
	/*delay */
	flag[0] = true;
}
/*critical section*/;

flag[0] = false;
```

```c
/*PROCESS 1*/
*
*
flag[1] = true;
while (flag[0]){
	flag[1] = false;
	/*delay */
	flag[1] = true;
}
/*critical section*/;

flag[1] = false;
```

→Deadlock 문제 해결!
데드락 해결하려고 while문안에서 잠시 자신을 `false`로 바꾸고 delay시킨다. delay하다가 P1으로 넘어가면 while문 쏙 피해서 critical section을 수행한다. → Deadlock 안빠짐.

Fourth Attempt 문제점 ⇒ **Livelock**

1. 동시에 수행해서 오면 `flag[0],[1]`이 `false`로 release해서 기다리다가 
2. `flag[0],[1]` 이 `true`로 바뀌면서 서로 또 다시 while문으로 들어간다. 
3. 계속 While문 돈다. ⇒ 상태를 계속 둘다 바꿔주기 때문에 Deadlock이라고는 안함.

⇒ 그럼 뭐냐???? 바로 Livelock이다. 

### Dekker’s Algorithm ⇒ `Turn`을 도입.

- 알고리즘.
    
    ![IMG_1A11DF106F60-1.jpeg](Ch05-22S-1%2058bda29b5eba40e3add0f96eed1088c4/IMG_1A11DF106F60-1.jpeg)
    
- 위의 모든 문제들을 해결!

### Peterson’s Algorithm

- Dekker’s Algorithm을 더 축소, 알아보기 쉽게
- 알고리즘
    
    ![IMG_381ADE77DA0B-1.jpeg](Ch05-22S-1%2058bda29b5eba40e3add0f96eed1088c4/IMG_381ADE77DA0B-1.jpeg)
    

## 병행성(Concurrency)의 원리

- Interleaving & overlapping → 똑같은 Concurrency의 문제를 가지고 있다.
    - interleaving: 멀티 프로그래밍에서 프로세스가 교환을 하면서 하는거(uniprocesser)
    Logical
    - overlapping: 동시간에 수행(multiprocessor)
    physical
- 저 두개의 문제점 : 프로세스 진행 스피드를 예상을 할 수가 없음. 왜? 존나 많은게 dependency 하다.
→ 병행성의 문제 이유들
    - 다른 프로세스의 활동(Activity)에 따라 달라짐
    - OS가 인터럽트 핸들을 어떻게 하냐에 따라 다양한 방법들이 생김.
    - OS의 schedule때문에 실행 속도를 예측 불가하다.

## Difficulties of Concurrency(병행성)

- 병행성 제공하다보면 글로벌 리소스 공유할 때 문제가 생길 수 있음
    - 순서가 엇갈리면 큰일나~
    - Read & Write의 순서가 엇갈리면 값이 바뀌게 된다.
- OS가 리소스 할당하는데 있어서 어렵다
    - 잘못 할당하면 deadlock
- 잘못된 프로세싱으로 인해서 오류가 간헐적으로 발생하면 문제를 찾기 어려움

## Race Condition

- 많은 프로세스들이나 스레드들이 데이터를 Read & Write할 때 일어남
- 마지막 결과는 순서에 의해서 결정됨
    - race의 loser(늦게 들어온 녀석)이 결과값을 결정한다!
    - loser가 마지막에 수행을 함으로써 마지막에 결괏값을 넣기 때문에 그 결괏값이 적용된다!

### OS가 고려해야할 사항

- 다양한 프로세스의 Track에서 process상태를 PCB를 통해 유지 → ch4
- 리소스 할당하고 회수하는 것과 동시에 자원에 엑세스하는 걸 관리 → ch8
- 피지컬 리소스와 데이터를 의도치 않은 간섭(interference)으로부터 보호 → ch7
- 프로세스의 기능과 수행 결과는 다른 동시에 수행 중인 프로세스로부터 영향을 받지 않고 독립적으로 진행돼야함 → ch5

### Resource Competition

- 경쟁 왜 발생?
    - concurrent(병행) process들이 같은 자원 사용하기 위해 경쟁하면 충돌이 발생하는데 그때 문제가 생김
    - same resource(같은 자원) : I/O device, memory, processor time, clock
- 경쟁 할 때 3가지 제어(control) 문제 발생
    - 상호 배제가 필요해짐 → 누가 사용할때 다른 놈이 사용 못하게
    - 상호배제를 하다보니
        - deadlock 발생
        - starvation 발생

### Mutual Exclusion(by 임계 영역)

![Screen Shot 2022-04-14 at 1.27.00 AM.png](Ch05-22S-1%2058bda29b5eba40e3add0f96eed1088c4/Screen_Shot_2022-04-14_at_1.27.00_AM.png)

- 임계 영역(critical section)을 지정해서 사용하면 mutual exclusion을 수행 가능하게 한다.
- 임계영역 : 한 프로세스만 접근해서 사용할 수 있음.
- 이 그림에서는 Ra라는 resource를 요청.
- 이렇게 critical section 보호해줄 수 있따!

### 프로세스 간의 “Share”를 통한 Cooperation → 누구랑 상호작용하는지 모른다.

- Share를 통한 협력의 경우 간접적으로 상호작용한다. 누구랑 상호작용하는지 모른다!!
- 그래서 이런 협력을 하는 프로세스들은 다른 프로세스들이 포함되지 않는 독립적인 수행 환경을 갖는다.
- 프로세스가 다른 프로세스에 대해 명시적으로 인식하지 않고도 수행이 상호작용하도록 해야함
- 프로세스가 다른 애들을 참조하지 않더라도 공유 된 data를 사용할 수 있고, 업데이트 할 수 있어야한다
- 다른 녀석들도 공유된 data에 접근할 수 있다!
- 프로세스는 공유된 자원이 잘 관리되도록 협력해야함
- control mechanism을 통해 접근함으로써 Shared된 data가 일관성을 유지하도록 보장
- data가 공유되다 보니 mutual exclusion, deadlock, starvation같은 문제가 생겨버림!
- ex) read에 대해서는 mutual exclusion을 안하고 write에 대해서만 mutual exclusion을 적용하는 방법도 있다~!

### 프로세스 간의 “Communication”를 통한 Cooperation(협력)

- Share는 서로를 모르지만 Communication(통신)을 할 때는 서로가 누군지 안다!
- 특정 목적을 가지고 묶여있는 놈들임.(Processes)
- Commmunication을 통해 다양한 활동(synchronize, coordinate,등..)을 한다~
- Communication을 할 때는 특정 메시지들을 주고받으면서 사용한다
- 상호간에 메시지를 주고받을 때 아무것도(자원을) 공유하지 않는다
    - mutual exclusion 필요 없음
    - deadlock, starvation 발생
    서로 쟤가 메세지 주겠네? 하면서 대기상태. → Deadlock(서로 메세지를 받으려는 상태)
    프로세스1,2,3 → 1, 2 끼리만 계속 메세지 주고받음 → 3은 Starvation