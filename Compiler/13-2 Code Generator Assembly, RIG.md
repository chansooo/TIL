# 13-2. Code Generator : Assembly, RIG

### MIPS

- 연동 파이파라인 단계가 없는 마이크로 프로세서
- 많은 임베디드 시스템에서 사용
- 메인스트림 CPU 아키텍처도 MIPS와 유사한 방식으로 동작

### MIPS assembly

- 메모리 접근을 load / store 명령어로만 진행
- 모든 명령어는 레지스터를 피연산자로 사용
    
    ⇒ 32개의 범용 레지스터가 존재하기 때문에 효율적으로 사용해야 한다
    

- 강의에서의 가정
    - 32비트 아키텍처
        
        → 모든 객체가 4-byte aligned
        
    - 두 가지 유형의 레지스터만 사용
        - $sp : 최상위 스택의 다음 위치를 가리키는 스택 포인터를 가리키는 레지스터
        - $r0, $r1, … : 임시 값 저장
            - r 레지스터를 무한대로 가정

## MIPS 명령어

---

### Basic instructions

- lw reg1 offset(reg2) → Load
    
    : reg2+offset 위치의 값을 reg1으로 불러온다
    
- sw reg1 offset(reg2) → Store
    
    : reg2+offset 위치에 reg1을 저장한다.
    
- 산술 연산
    - add reg1 reg2 reg3 : reg1 = reg2 + reg3
    - mul, sub, div
- 비교
    - seq reg1 reg2 reg3 : reg1 = reg2 == reg3
    - sne, sgt, sge, slt, sle
        
        ![스크린샷 2022-06-20 01.56.12.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-20_01.56.12.png)
        
        맞으면 1 틀리면 0이 reg1에 저장된다.
        
- 상수 (immediate)
    - li reg1 immediate (reg1=immediate)
    - addi reg1 reg2 immediate(reg1=reg2+immediate)
    - seqi reg1 reg2 immediate(reg1=reg2==immediate)
        
        ![스크린샷 2022-06-20 01.57.12.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-20_01.57.12.png)
        
    

### Flow control instructions

- j label
    
    : 조건 없이 label이 가리키는 곳으로 바로 jump
    
- beq reg1 reg2 label
    
    : reg1 == reg2 일 경우 label로 분기
    
    - bne (≠), bgt (>), bge(≥), blt (<), ble(≤)
- beqi reg1 immediate label
    - reg1 == imm → label 분기
        
        ![스크린샷 2022-06-20 02.02.22.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-20_02.02.22.png)
        

## 방법 1. TAC를 MIPS 명령어로 직접 변경 “a = b+c”

---

![스크린샷 2022-06-19 02.09.21.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_02.09.21.png)

- Stack은 high → low address 방향으로 늘어난다.
- $sp는 스택의 top 다음 위치를 가리킨다.
- 따라서 $sp + 4의 배수를 통해 스택의 객체에 접근할 수 있다.

> 헷갈릴 수 있는데, stack에 쌓이는 방향은 high → low, $sp를 통해 접근할 때는 low → high 방향이라고 생각하면 된다.
> 

### ex#1: a = b + c

1. 변수를 메모리에서 레지스터로 불러온다
    
    lw $r0 8($sp) = b
    
    lw $r1 4($sp) = c
    
2. 레지스터끼리 add 연산을 수행한다
    
    add $r2 $r0 $r1
    
3. 결과를 메모리에 저장한다.
    
    sw $r2 12($sp)
    

### ex#2: a = b < 3

1. 변수를 메모리에서 레지스터로 불러오기
    
    lw $r0 4($sp) = b
    
2. 레지스터에 seq 연산 수행
    
     slti $r1 $r0 3
    
3. 결과를 메모리에 저장
    
    sw $r1 8($sp)
    

### ex#3: 분기 처리

bnez  !=이다.

![스크린샷 2022-06-19 02.38.56.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_02.38.56.png)

### ex#4: 루프

![스크린샷 2022-06-19 02.40.25.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_02.40.25.png)

### 함수 호출 코드 생성

2runtime environment와 함께 생각해야 한다.

1. 함수 foo를 호출 : return space 만들기(아직까진 empty공간)
- subi $sp $sp 4 이건 이제 포인터 4만큼 내리는거임.
    
    ![스크린샷 2022-06-19 03.04.09.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.04.09.png)
    
1. 함수 foo를 호출 : 매개변수 저장하기
    
    ![스크린샷 2022-06-19 03.04.38.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.04.38.png)
    
2. 함수 foo를 호출 : control link 저장
    
    현재 함수인 main()에 있는 activation record의 시작 위치를 가리킨다.
    
    ⇒ foo가 반환되고 다시 main으로 돌아가야 하니까!
    
    ![스크린샷 2022-06-19 03.05.51.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.05.51.png)
    
3. 함수 foo를 호출 : machine의 상태를 저장
    
    CPU 레지스터 $ra에는 리턴 후에 돌아갈 위치를 기록하게 된다.
    
    따라서 현재 스택의 top을 가리키는 $sp를 저장한다.
    
    ![스크린샷 2022-06-19 03.08.48.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.08.48.png)
    
4. foo() 준비 : local variable의 공간 생성
    
    ![스크린샷 2022-06-19 03.11.37.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.11.37.png)
    
5. foo() 실행
    
    ![스크린샷 2022-06-19 03.12.06.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.12.06.png)
    
    foo 함수 내부에서는 지역적인 계산 등에는 runtime environment를 고려하지 않고 진행하면된다.
    
    각 TAC 코드를 적절한 MIPS로 변환한다.
    
6. foo() 완료
    
    ![스크린샷 2022-06-19 03.15.05.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.15.05.png)
    
    1. 4에서 저장한 machine 상태를 복구한다.
    2. foo()에 할당되었던 activation record가 스택에서 제거된다.
    3. return address (여기서는 main을 가리킨다)로 돌아가고 return space를 통해 b에는 결과가 저장될 것이다.

### 위 방법의 문제점

1. 레지스터를 무한하다고 가정 → 비현실적
2. intermediate 코드를 어셈블리로 바로 변환 → 비효율적
    
    why? 메모리 접근을 불필요하게 하는 경우가 생긴다. 
    

### 더 나은 레지스터 할당 방법

- 리소스는 제한되어 있으므로 메모리 접근(읽기/쓰기)를 줄여야 한다.
- 사용 가능한 레지스터 수를 초과하지 않아야 한다.
- 메모리에 저장하는 대신 레지스터에 가능한 많은 변수를 저장해야 한다. (훨씬 빠르니까)

- 핵심 아이디어
    - 서로 다른 변수 a, b 중 최대 하나가 live variable일 때 동일한 레지스터를 공유할 수 있다.
    - 쉽게 설명하자면 두 변수가 동시에 live이면 같은 레지스터를 사용하면 안된다! (둘다 살아있는데 같은 곳에다 저장하면 안되겠지?)

![스크린샷 2022-06-19 03.26.37.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.26.37.png)

## 방법2. Live Variable Analysis를 이용한 레지스터 할당

---

### 1. Live Variable을 전체 코드에 대해서 분석한다.

![스크린샷 2022-06-19 03.33.00.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.33.00.png)

### 2.  RIG (Register Interfernece Graph)를 생성한다.

- 노드 : 각 변수를 나타낸다.
- edge(간선) : 프로그램의 특정 지점에서 두 변수가 동시에 살아있다.
- 레지스터 간 간섭을 나타내는 그래프이다.

![스크린샷 2022-06-19 03.34.54.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.34.54.png)

이 때 주의해야할 점

- b-c 연결, d-c 연결 상태
- b랑 d는 연결이 없으므로 같은 레지스터를 공유할 수 있다.
- b랑 c는 동시에 살아있기 때문에 같은 레지스터를 공유하면 안된다.

### Graph coloring

![K = 4일 때 그래프 채색 문제로 변형](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_03.42.48.png)

K = 4일 때 그래프 채색 문제로 변형

사용 가능한 레지스터가 K개일 경우, RIG에서 연결된 변수들이 같은 레지스터를 공유하지 않도록 할당하는 방법은?

⇒ Graph coloring 문제를 해결하는 것과 같다.

하지만 이는 NP-hard 문제로, O(2^n*n)의 시간복잡도를 가진다.

⇒ heuristic을 이용한다.

### Heuristics for graph coloring

핵심 아이디어

- K보다 작은 neighbor를 가지는 노드 T를 제거
- 남은 RIG가 K-colorable → 제거된 T 노드를 포함해도 K-colorable이다.

1. K보다 작은 neighbor를 가지는 노드 T를 선택
2. T를 RIG에서 제거하고 스택에 넣는다.
3. RIG에 노드가 없을 때까지 1~2를 반복한다.
4. 스택 top의 노드를 pop한다.
5. 노드에 neighbor에 이미 할당된 색상과 다른 색을 할당한다.
6. RIG가 완전히 만들어질 때까지 4~5를 반복한다.

- 예시
    
    ![스크린샷 2022-06-19 04.18.14.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.18.14.png)
    
    ![스크린샷 2022-06-19 04.18.27.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.18.27.png)
    
    ![스크린샷 2022-06-19 04.18.35.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.18.35.png)
    
    ![스크린샷 2022-06-19 04.18.42.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.18.42.png)
    
    ![스크린샷 2022-06-19 04.18.47.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.18.47.png)
    
    ![스크린샷 2022-06-19 04.18.54.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.18.54.png)
    
    ---
    
    ![스크린샷 2022-06-19 04.19.18.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.19.18.png)
    
    ---
    
    ![스크린샷 2022-06-19 04.19.27.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.19.27.png)
    
    ![스크린샷 2022-06-19 04.19.35.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.19.35.png)
    
    ![스크린샷 2022-06-19 04.19.42.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.19.42.png)
    
    ![스크린샷 2022-06-19 04.19.50.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.19.50.png)
    
    ![스크린샷 2022-06-19 04.20.00.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.20.00.png)
    
    ![스크린샷 2022-06-19 04.21.26.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.21.26.png)
    
    TAC의 모든 변수가 레지스터를 사용하도록 변경되었다.
    

### Spilling

같은 예시에서 K = 3일 때를 생각해보자

![스크린샷 2022-06-19 04.07.19.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.07.19.png)

더 이상 K보다 작은 neighbor를 가지는 노드가 없다.

1. spilling 대상으로 한 노드를 선택한다.
2. 변수에 대해 메모리를 할당한다.
    - ex) c → ca
    - spilling 변수가 겹치면 다른 이름을 사용한다.
        - ex) c0, c1, …
3. 해당 변수가 레지스터가 아닌 메모리를 사용하도록 intermediate code를 수정한다.
    - 예시
        
        ![스크린샷 2022-06-19 04.22.59.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.22.59.png)
        
        ![스크린샷 2022-06-19 04.23.27.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.23.27.png)
        
4. 변경된 코드를 기반으로 live variable analysis 수행한다.
    - 예시
        
        ![스크린샷 2022-06-19 04.24.34.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.24.34.png)
        
5. RIG를 생성한다.
6. 변수를 할당된 레지스터로 변환한다.

![스크린샷 2022-06-19 04.25.19.png](13-2%20Code%20Generator%20Assembly,%20RIG%2083efa9d6e2ff46fabf23e49fd7bb2fab/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_04.25.19.png)

### spill 대상 선택

- 정답은 없다
- 모든 spilling 결정은 RIG K-colorable을 만들 수 있다.
- 상황에 따라 적절한 노드를 선택해야 한다.
    - 예시
        - neighbor가 가장 많은 변수
        - 사용 빈도수가 가장 적은 변수
        - 루프의 밖에 위치한 변수