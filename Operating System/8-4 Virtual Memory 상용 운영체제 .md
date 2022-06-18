# 8-4. Virtual Memory: 상용 운영체제 (1)

## Unix and Solaris Memory Management

---

UNIX는 기계 독립적으로 설계되었기 때문에 시스템에 따라 다른 메모리 관리 방법이 사용된다.

- 초기 UNIX : 가상 메모리 없는 가변 partitioning
- 현재 UNIX : Paged virtual memory

SVR4 및 Solaris는 두 가지 방법을 사용한다.

- Paging system
    - use level에서 사용
    - 메모리에 있는 page frame을 프로세스에 할당하는 VM 기능을 제공
    - page frame을 disk block buffer로 이용하기 위한 할당 제공 (파일 입력 버퍼)
- Kernel memory allocator
    - 커널에서는 VM이 적합하지 않다.
        
        페이지보다 작은 단위의 table과 buffer를 많이 사용하기 때문!
        
        → buddy system을 이용한다.
        

### UNIX의 paged virtual memory에 사용되는 자료구조

![스크린샷 2022-06-05 05.10.07.png](8-4%20Virtual%20Memory%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6%20(1)%201ad14088e03346c2a01982a51371badc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-05_05.10.07.png)

- **Page table**
    
    : 프로세스마다 페이지 테이블을 가진다.
    
    - Age : 페이지가 참조되지 않은 경과시간
    - Copy on write : 여러 프로세스가 해당 페이지를 공유하고 있을 때, write 작업을 위해서 복사본을 만들어서 그 장소에 쓸 수 있도록 지원한다.
    - Modify : 페이지가 수정되었는지
    - Reference : 페이지 참조되고 있는지 여부
    - Valid : 주기억장치에 있는지 여부
    - Protect : 쓰기 작업이 허용되는지 여부
- **Disk block descriptor table**
    
    : 페이지 번호로 인덱싱되고 virtual page가 disk의 어디에 위치했는지 기록한다.
    
    - Swap device number : 논리적인 디바이스 번호
    - Device block number : swap device에서의 페이지의 블록 위치
    - Type of storage : 저장공간의 타입
- **Page frame data table**
    
    : 해당 프레임에 대한 참조의 정보
    
    → 페이지 교체 알고리즘에 사용된다.
    
    - Page state : 페이지 상태
    - Reference count : 페이지가 참조의 수
    - Logical device : 페이지가 위치한 logical device
    - Block number : logical device에서 페이지가 위치한 블록
    - Pfdata : free page 리스트의 포인터
- **Swap-use table**
    
    : swap되는 장치마다 swap된 페이지에 대한 정보를 기록한다.
    

### Page Replacement

- page frame data table을 페이지 교체에 사용한다.
- Pfdata 포인터를 사용해 free frame 리스트와 연결된다.
- 가용 프레임 수가 임계값(threshold) 밑으로 떨어지면 커널이 사용자 프로세스와 커널 캐시로부터 프레임을 회수한다.

- **Two-handed clock algorithm**
    
    : SVR4에서 사용하는 페이지 교체 알고리즘
    
    ![스크린샷 2022-06-05 15.13.26.png](8-4%20Virtual%20Memory%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6%20(1)%201ad14088e03346c2a01982a51371badc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-05_15.13.26.png)
    
    - `reference bit`
        - 처음 페이지 반입 시 0으로 초기화
        - 읽기/쓰기 작업 시 1로 설정
    - `fronthand`
        - 허가 받은 (not locked) 페이지를 조사한다.
        - reference bit을 0으로 설정한다.
    - `backhand`
        - 일정 시간 뒤에 fronthand가 갔던 동일한 리스트를 보면서 refenece bit을 확인한다.
            - 만약 1이라면 참조가 된 것
            - 만약 0이라면 참조가 안된 것 → 페이지 교체 대상
    - 알고리즘 매개변수
        - `Scanrate`
            - 초(second)당 확인할 페이지의 개수 = clockhand의 속도
            - 메모리가 작다면 속도를 높여서 페이지를 빨리 반환하도록 한다.
        - `Handspread`
            - fronthand - backhand 사이 간격
            - scan-rate처럼 페이지 반환 속도를 결정한다.

### Kernel Memory Allocator

- 커널은 페이지보다 훨씬 작은 크기의 테이블과 버퍼를 동적할당해서 생성하고 사용한다.
- 커널은 빠른 생성 및 종료가 필요하므로 페이징이 비효율적이다. (가상 테이블을 사용하는 과정 등에서)

- **Laze Buddy**
    
    기존 버디 시스템에서 자원이 반환되면 즉시 버디를 합쳤다
    
    - 특정 크기의 블록에 대한 요구는 급격하게 변화하지 않는다. (비슷한 시간대에는 비슷한 크기의 블록이 필요)
        
        → 즉시 버디를 합치면 다시 원래 크기의 블록이 필요할 수 있다!
        
    - 불필요한 작업(합병/분할)를 하지 않도록 필요할 때까지 합병을 지연시키자
    - 매개변수
        - $N_i$ : 2^i 크기의 블록 개수
        - $A_i$ : 2^i 크기의 할당된 블록 개수
        - $G_i$ : 2^i 크기의 globally free한 블록 개수
        - $L_i$ : 2^i 크기의 locally free한 블록의 개수
            - 가까운 미래에 사용될 것으로 예상 → free되어도 합병하지 않는다.
        - $N_i=A_i+G_i+L_i$
        - $D_i$ : Delay variable
            
            = $A_i-L_i=N_i-2L_i-G_i$
            
    - 알고리즘
        
        Di의 초기값은 0
        
        1. 다음 operation이 블록 할당을 요청할 때
            1. 할당할 free 블록을 하나 선택
                
                if  locally free 라면?
                
                Di = Di + 2 (Li에 -1을 대입)
                
                else
                
                Di = Di + 1 (Gi에 -1을 대입)
                
            2. free 블록이 없다면
                
                큰 블록을 원하는 크기까지 재귀적으로 나눈다.
                
                해당 크기의 블록이 두 개 생겼을 것인데, 하나는 할당해주고 & 하나는 locally free로 설정
                
                Ai, Li가 둘다 1씩 증가 → Di는 변화가 없다.
                
        2. 다음 operation이 블록 해제를 요청할 때
            1. Di ≥ 2
                
                (할당 요청 때 locally free인 블록은 Di+2를 했지? 그니까 이건 지역적으로 할당 해제를 해야하는 블록이다!)
                
                → locally free로 표시 & 지역적인 할당 해제
                
                → Di = Di-2
                
            2. Di == 1
                
                (할당 요청 때 globally free인 블록은 Di+1을 했지? 그니까 이건 전역적인 할당 해제를 해야 한다)
                
                → globally free로 표시 & 전역적인 할당 해제 + 가능하면 합병
                
                → Di = Di-1 = 0
                
            3. Di == 0
                
                globally free로 표시 & 전역적인 할당 해제
                
                2^i 크기의 locally free 블록 하나를 전역적 할당 해제 + 가능하면 합병
                
                → 전역적 할당 해제 * 2 & locally free block의 할당 * 1
                
                → globally free block은 2증가 & locall free block은 1 감소
                
                ⇒ -2Li-Gi에 대입하면 Di는 변화가 없다.
                
                ⇒ Di = 0
                

### Linux Memory Management

두 가지 메인 측면

1. Process virtual memory
2. Kernel memory allocation

물리적 페이지는 4KB~2MB(hugepage)까지의 크기를 지원

### Linux Virtual Memory

3계층 메모리 테이블 구조를 사용한다.

![스크린샷 2022-06-05 18.06.50.png](8-4%20Virtual%20Memory%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6%20(1)%201ad14088e03346c2a01982a51371badc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-05_18.06.50.png)

- Page directory
    - 활성 프로세스는 페이지 1개 크기의 단일 page directory를 가짐
    - middle directory의 한 페이지를 가리킨다.
    - 메모리에 상주하며 운영된다.
- Page middle directory
    - 여러 개의 페이지에 걸쳐 있을 수 있다.
    - 각 entry들은 페이지 테이블의 페이지를 가리킨다.
- Page table
    - 여러 개의 페이지에 걸쳐 있을 수 있다.
    - 각 entry들은 프로세스의 virtual page를 가리킨다.

### Page Replacement Algorithm

- **clock 알고리즘**
    - 이전 Linux (2.6.28)에서는 clock 알고리즘에 기반한 교체 정책을 사용
    - 8bit `age`  를 이용해서 `use bit` 을 나타낸다. (접근할 때마다 증가)
    - 주기적으로 age bit를 감소시킨다
        - age가 0이 되면 교체 후보가 된다.
        - age가 큰 페이지는 자주 참조되는 페이지로 교체하지 않는다.
    - 문제점 (메모리 용량이 커짐에 따라)
        - 8 bit 사용 → 메모리 차지 많음
        - age 연산에 프로세서 시간이 많이 소요됨
- **split LRU 알고리즘**
    
    ![스크린샷 2022-06-05 18.20.48.png](8-4%20Virtual%20Memory%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6%20(1)%201ad14088e03346c2a01982a51371badc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-05_18.20.48.png)
    
    - 따라서 이후 Linux (2.6.28)에서는 이를 사용한다.
    - 각 page table entry 마다 두 개의 flag 비트를 사용
        - `PG_active`
        - `PG_referenced`
    - 물리적 메모리를 특정한 구역(zone) 단위로 나눠서 zone마다 `active` , `inactive` 두 개의 연결 리스트를 사용한다.
    - 페이지 관리 절차
        1. 처음에는 `inactive` 리스트의 페이지에 접근 → `PG_referenced` flag를 1로 설정한다.
        2. 참조된 page가 두 번째 접근이 되었다면 → `active` 리스트로 간다.
        3. 충분한 시간동안 두 번째 접근이 없다면 → `PG_referenced` 를 0으로 설정한다.
        4. 활성 페이지는 두 번의 timeout이 발생해야 `inactive` 리스트로 간다.
        5. `inactive` 리스트에서 교체할 페이지를 선택할 때는 LRU 알고리즘을 선택한다.
    
    ⇒ 자주 사용하는 페이지는 교체 대상으로 선정되지 않는다. (두 번의 timeout이 발생해야 교체 가능)
    

### Kernel Memory Allocation

virtual memory에서 사용되는 페이지 관리 정책을 기반으로 한다.

하지만 커널에서는 페이지 크기보다 작은 chunk가 필요한 일이 많다..

- **slab allocation**
    
    chunk 단위로 할당하기 위한 기법
    
    buddy algorithm의 형태로 4KB 페이지를 2^K 크기로 나눠서 chunk를 할당한다.
    

## Windows Memory Management

---

Windows virtual memory manager가 메모리 할당 및 페이징에 대해 제어한다.

4KB ~ 64KB의 페이지 크기를 제공한다.

### Windows Virtual Address Map

- 32bit 운영체제에서 각 프로세스는 4GB의 가상 메모리를 할당 받는다.
    - 이 중 절반은 OS에게 예약된다.
        
        → 사용자는 2GB를 사용할 수 있다.
        
    - 커널 모드에서는 2GB의 system space를 공유해서 사용한다.
- 큰 메모리가 필요하면 64bit 운영체제를 쓰면 된다.

![Windows Default 32bit Virtual Address Space](8-4%20Virtual%20Memory%20%E1%84%89%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%AD%E1%86%BC%20%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6%20(1)%201ad14088e03346c2a01982a51371badc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-05_18.31.23.png)

Windows Default 32bit Virtual Address Space

- 첫 번째 빈공간은 NULL pointer를 위한 것
- 두 번째 빈공간은 OS가 out of bound 포인터 참조를 확인하기 위한 공간

### Windows Paging

- 페이지 3개의 상태
    - Available : (=free) 누구나 사용할 수 있는 상태
    - Reserved : 프로세스가 사용을 위해서 예약한 상태
    - Committed : 메모리에 실제 물리주소로 할당되어 있는 상태

### Resident Set Management System

variable allocation + local scope 방법을 사용한다.

- 활성 프로세스의 Working set은 일반적인 규칙을 사용해서 조정한다.
    - 만약 메모리가 풍족할 때
        - 적재 집합(resident set)을 계속 증가시키는 것을 허가한다.
            - 새로운 페이지가 추가되어도 기존 페이지를 swap out하지 않는다.
            - 메모리 사용량이 급격하게 증가하는 프로세스는 예의주시하면서 최근 사용되지 않은 페이지를 제거한다 → 새로운 프로그램이 들어왔을 때 바로 할당할 수 있도록 준비한다.
                
                (메모리가 풍부할 때도 관리를 한다)
                
    - 만약 메모리가 부족할 때
        - working set에서 최근 사용되지 않은 페이지를 찾아서 해제한다.

## Android Memory Management

---

Linux 기반이지만 하드웨어의 자원이 풍부하지 않기 때문에 추가적인 관리 방법이 필요하다.

- ASHMem (Anonymout SHared Memory) : file descriptor를 이용해서 다른 프로세스와 메모리 공유를 지원
- ION
    - 그래픽 또는 멀티미디어 장치에서 사용하는 메모리 관리자
    - 프로세스와 드라이버 사이 간의 버퍼 공유를 지원
- Low Memory Killer
    - 대부분의 모바일 기기는 swap을 지원하지 않는다.
        - why? 모바일에서는 NAND 플래시 메모리를 사용하는데 이는 쓰기 횟수 제한이 있다.
        - 따라서 메모리의 수명을 단축시키지 않기 위해서 swap을 하지 않는다.
    - 메모리가 고갈되어 갈 때는 거의 모든 메모리를 사용하던 앱을 종료시켜 버린다.
        - 이를 위해서 앱에 종료 요청을 하는 기능이 있으며, 이후에 강제로 종료시킨다.