# Ch04-22S-2

### Thread의 종류

1. User Level Thread(ULT) → 사용자
2. Kernel Level Thread(KLT) → Lightweight Process(앞에 설명쭉했던거) 여기 속한다!

### User Level Thread

- Application을 통해서 모든 Thread를 관리.
- Kernel은 User Level Thread의 존재 자체를 알 수가 없음.
- 그럼 Kernel의 역할을 누가하냐?
- Threads Library가 관리한다.

![5.jpeg](Ch04-22S-2%20cadfe/5.jpeg)

### ULT와 Process State사이 관계

---

![2.jpeg](Ch04-22S-2%20cadfe/2.jpeg)

1. Thread2 가 Running상태에서 시스템을 호출(I/O요청) 
2. Kernel에서는 Process B를 Blocked로 바꿈.
3. ULT에서는 Kernel이 Blocked 됐다는걸 알 수 없음
4. Thread 2는 계속 Running상태 유지

---

![1.jpeg](Ch04-22S-2%20cadfe/1.jpeg)

1. Process가 Running이었다가 Timeout이 돼서 Process B가 Ready상태로 감.
2. 하지만 User-Level에서는 얘가 Running인지 Ready인지 모름
3. 그래서 Thread2가 Running 상태 유지 중

---

![3.jpeg](Ch04-22S-2%20cadfe/3.jpeg)

1. Process B가 Running 중인데
2. User Level에서 자체적인 스케쥴링으로 Thread2 는 Blocked로 바뀌고 Thread1은 Running으로 바뀜
3. 예를 들어 Thread2가 Running중인데 Thread1을 통해 메세지를 받게돼서 Thread2가 Blocked돼서 기다리고 Thread1은 Running.

---

⇒ 결론. Kernel과 User은 독립적으로 움직인다!

### ULT(User Level Thread)의 장점

1. Thread Switching 할때 Mode Switch가 필요없음!! → 빠른 전환 가능
2. Scheduling을 Application에 맞도록 할 수 있음. → 응용프로그램에 따라 맞는 스케쥴링을 적용 할 수 있다.
3. 모든 운영체제에서 적용 가능하다! → Kernel의 개입이 없기 때문에 가능한 일!

### ULT(User Level Thread)의 단점

1. ULT 에서 System Call을 하면 Process가 Blocked된다. → 그렇게 되면 Thread의 상태들이 Blocked가 아니지만 Process가 Blocked 되었기 때문에 모든 Thread가 Blocked된 것 처럼 처리된다.
2.  ULT는 Multiprocessing의 장점을 사용 할 수 없다. 
→ 왜냐면 Kernel은 하나의 Processor에 하나의 Process만 할당한다. ⇒ 즉 하나의 Thread만 실행이 가능하다는 뜻! ⇒ 따라서 동시에 여러 Process를 할당을 못해서 Multiprocessing을 할 수 없다!!
~~Multiprogramming까진 가능하나 Multiprocessing을 할 수 없다.~~

### ULT의 단점 극복 방법

1. Multiple Threads 대신 Multiple Process를 제공하는 방법 [굳이 이걸 안쓴다!!]
    - Thread의 장점이 모두 사라진다.
    - Process Switch의 Overhead가 더 발생해서 오히려 더 안좋음...
2. Jacketing
    - Blocking되는 System Call이 아닌 Non-Blocking되는 System Call을 하자!
    - Ex) I/O
        
        → System을 직접 호출하는 I/O Routine이 아닌 Applicaion Level I/O Jacket Routine을 호출하자.
        
        → 만약 Applicaiton I/O Jacket을 보고 실행중이면 I/O 를 요청한 Thread(T2)를 Block시키고 다른 Thread(T1)를 Running시킨다(괜히 T2를 실행시키면 System Call이 되어 뒤에서 기다리고 있는 Thread들이 Block된것처럼 되니까)  그러다가 T1이 끝나고 T2를 실행시켜야 할 떄 다시 Jacket을 보고 I/O가 끝나있으면 그떄서야 System Call을 해서 T2를 실행시킨다. 
        

### KLT(Kernal-Level Thread)

![1.jpeg](Ch04-22S-2%20cadfe/1%201.jpeg)

- Kernel이 Thread를 직접 관리.
- Applicaion Level(User Level)에는 KLT를 관리하는 코드는 없고 Application Programming Interface(API)로 Kernel Thread를 이용하게 됨.
- Window에서 많이 사용!!

### KLT(Kernel Level Thread)의 장점

1. 같은 Process에 있는 Multiple Thread를 동시에 Scheduling 가능 → 속도 ↑
2. 1 : 1 매칭되어 있어서 한 Thread가 Block되어도 다른 Thread들은 Schedule 될 수 있다. (이때 Thread들은 한 Process에 있다고 가정.)
Ex) 한 Thread에서 I/O 요청 들어오면 그 Thread만 Blocked하고 나머지는 Schedule대로 움직이면 됨.
3. Kernel Routine은 Multithread를 제공 할 수 있다.

![1.jpeg](Ch04-22S-2%20cadfe/1%202.jpeg)

### KLT(Kernel Level Thread)의 치명적인 단점

- 같은 Proces에 있는 Thread들에서 어떤 한 Thread가 통신같은 것이 실행돼서 다른 Thread로 Control이 넘어가면 이때 Mode Switching이 필요하다 → 엄청난 시간 Delay

![2.jpeg](Ch04-22S-2%20cadfe/2%201.jpeg)

### Combined Approach

![1.jpeg](Ch04-22S-2%20cadfe/1%203.jpeg)

- User Space
    - Thread 생성
    - Thread 스케쥴링
    - Thread 동기화(synchronization)
- Kercel Space
    - ULT들을 KLT들에 mapping 시켜준다.
    - Mapping → ULT수 ≥ KLT수
    - 이러면 Kernel에서 ULT의 존재를 알게 된다. ULT에서 System Call하면 KLT중 하나만 Blocked 되고 나머지는 Schedule대로 행동하면됨.
    - 원래는 한 시점에 한 Thread만 움직였는데(KLT관점에서) Mapping이 되다 보니 동시에 여러개의 Processor에 할당을 해서 Parallel하게 스케쥴링 가능!
- ULT의 단점을 해소한다! By Mapping
→ Blocked되는 것
- Ex) Solaris

### Relationship between Threads and Processes

- 1 : 1
- M : 1
- 1 : M
- M : N

### Multicore & Multithreading

- 멀티코어에서의 성능은 Application이 Parallel 한 Resources를 어떻게 활용하는가에 따라 성능이 달라진다.
- Amdahl’s law
    - 하나의 Applicaiton을 N개의 Parallel한 Processor를 가진 Multicore System에서의 성능
    - Speedup = $프로그램을한개의Processor에서 실행하는데 걸리는 시간 \over 프로그램을 N개의 Processor에서 실행하는데 걸리는 시간$
    
                           = $\frac 1{(1-f)+\frac fn}$
    
    - f : 병렬로 처리가능한 code부분
    - 1-f : 병렬로 처리 못하는 code 부분 ~~(sequence하게 처리한다)~~
    - n을 나눠주는 이유 : n개의 Parellel한 Processor가 있기때문에 병렬로 처리가능한 code부분을 n으로 나눠주는것이다.

### Performance Effect Multiple Core

![IMG_04E2482974FA-1.jpeg](Ch04-22S-2%20cadfe/IMG_04E2482974FA-1.jpeg)

- 0% → linear하게 speed 증가
- 10% → 4.7배 만큼만 증가.
- Sequence로 처리할 코드들이 많으면 8개의 Processor가 들어갔다고해서 성능이 그만큼 좋아지지는 않는다.

![IMG_E5934AB67CF5-1.jpeg](Ch04-22S-2%20cadfe/IMG_E5934AB67CF5-1.jpeg)

- Processor가 많아지면 오히려 성능이 안좋아짐.
- Multiprocessing을 하게되면 소프트웨어로 처리할 동기화 작업들이 ↑ → 오히려 성능 저하.