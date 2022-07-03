# 10. Semantic Analyzer: Type checking

### 도입

- **Type**
    
    : 프로그래머가 데이터를 사용하려는 의도를 컴파일러에게 알려주는 데이터 속성
    
- **Type system**
    
    : 컴퓨터 프로그램의 다양한 구성에 특정 유형을 할당하는 일련의 규칙
    
    ex) a && b → 두 변수가 boolean이어야 하고, 결과도 boolean이다.
    
- **Type checking**이 필요한 이유
    - 어샘블리 언어 수준의 operation에서는 타입을 구별할 수 없다.
    - 따라서 AST같은 higher-level에서 타입 검사가 필요하다.
    
    ex) int *a = malloc(…); float b = 3.5; int c = a + b; → not allow되어야 한다.
    
    add $r1, $r2, $r3 → 여기서는 이미 구별할 수 없다.
    

### 타입 검사에 따른 언어 분류

- **Statically-typed languages(우리 수업에서 주목할 것!)**
    - 타입이 컴파일 타임에 결정된다.
    - 타입 검사도 컴파일 타임에 수행된다.(Run time 전에)
    - 장점
        - 런타임 성능
        - 이해하기 쉽고, 찾기 쉬운 타입 관련 버그(엄격한 Rule때문에)
    
     ex) C, C++, java, …
    
- **Dynamically-typed languages**
    - 타입이 런타임에 결정된다.
    - 타입 검사도 런타임에 수행된다.
    - 장점
        - 높은 유연성
        - 빠른 프로토타이핑 지원
    
    ex) python, js
    

## Rules of inference (추론의 규칙)

---

if ~ → then ~ 형식

if (Hypothesis가 맞다면) then Conclusion(결론)도 True다.

ex) A + B에서 A가 int, B가 int → A + B도 int

ex) A && B에서 if A는 bool, B는 bool → A && B는 bool

만약 B가 int라면 type error

### rules of inference의 표기법

![스크린샷 2022-05-27 05.02.33.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.02.33.png)

### 추론 규칙 표기 예시

![스크린샷 2022-05-27 05.03.20.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.03.20.png)

If we can infer that **A has type bool** and **B has type bool(Hypothesis가 True)** 

then **we can infer that A && B has type bool(Conclusion도 True)**

![스크린샷 2022-05-29 18.38.53.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-29_18.38.53.png)

---

- **Axioms (공리)**
    
    ![스크린샷 2022-05-27 05.04.47.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.04.47.png)
    
    - 위에 가정(Hypothesis가 없다면) Conclusion은 다 OK.

---

- 간단한 추론 규칙
    
    ![스크린샷 2022-05-27 05.05.10.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.05.10.png)
    
- 복잡한 규칙 표기
    
    ![스크린샷 2022-05-27 05.05.21.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.05.21.png)
    
    - 첫번째거만 설명해보겠음
        - 만약 e1이 int이고 e2가 int이면 e1+e2는 int이다.
    - 세번째거도 하겠음
        - e1이 Type T이고 e2가 Type T이면(T : short, int, long, float, double)
        e1+e2는 Type T이다!
    
    여러 타입이 필요한 경우 위와 같이 치환해서 사용할 수 있다.
    
    ![스크린샷 2022-05-27 05.06.38.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.06.38.png)
    
    ![스크린샷 2022-05-27 05.06.45.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.06.45.png)
    
    비교문에 대해서 이러한 방식으로 추론 규칙을 이용해 표기할 수 있다.
    

---

### Problem #1. free variables

: Variable이 Expression(표현)에서 Free하다는 것은 

: Type이 표현문에서 정의/선언되지 않은 변수

ex) int y = x + 3; → 여기서는 x가 Free Variable.

y = x + 3; → x, y 둘다 Free Variable.

→ Free Variable은 타입을 결정하기 위한 정보가 부족해서 추론 규칙으로 검사 불가능

⇒ scope information을 추가

### Soultion : Scope Information 추가하기!!!

![스크린샷 2022-05-27 05.12.33.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.12.33.png)

: 해당 Scope에서 Free Variable이 어떤 타입을 갖는지 선언문을 통해 정보를 가지는 것

⇒ **여기선 e가 scope S에서 Type T를 가진다.** 라는 의미임.

- 예제
    
    ![스크린샷 2022-05-27 05.13.53.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_05.13.53.png)
    
    이런 식으로 스코프 내에서 갖는 타입을 추론 규칙으로 표시한다.
    
    ![스크린샷 2022-05-27 10.54.41.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_10.54.41.png)
    
    - f는 scope S에서 non-member다.(속해있지 않음)
    - 함수 f는 Globally Declare되어있고 type(T1~Tn)에 대해서 return U를 한다.
        - f(T1~Tn) → U를 리턴.
    
    ![배열 인덱싱의 타입에 대한 스코프 추론 규칙](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_10.54.57.png)
    
    배열 인덱싱의 타입에 대한 스코프 추론 규칙
    
    - e1이 Scope S안에서 Type T[]를 가진다.(Array of Type T)
    - e2가 Scope S안에서 int이다.
    - e1[e2]는 Type T이다.
    
    int A[2];
    
    T = int, e2 = int, e1 = A
    
    A[0] → int 
    : if Type ≠ int? → error
    

---

### Problem #2. Statements

앞에서는 표현식에 대한 추론 규칙을 다뤘다. (A + B, A && B, …)

만약 statement의 semantic을 검사하기 위해서는 어떤 방법을 사용할까?

→ well-formed statement인지?

ex) int a = 3;

while(1>0) { well-formed statement1; well-formed statement2; … }

### Well-formedness rules

![스크린샷 2022-05-27 11.10.09.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.10.09.png)

추론규칙을 statement로 확장하여 사용한다.

- 예제
    
    ![타입 T로 선언된 함수에 대해 return stmt 추론 규칙](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.13.12.png)
    
    타입 T로 선언된 함수에 대해 return stmt 추론 규칙
    
    ![void로 선언된 함수에 대해 return stmt 추론 규칙](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.12.41.png)
    
    void로 선언된 함수에 대해 return stmt 추론 규칙
    
    ![statement의 집합으로 이루어진 경우](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.13.56.png)
    
    statement의 집합으로 이루어진 경우
    
    ![while의 추론 규칙](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.14.31.png)
    
    while의 추론 규칙
    
    - 여기서 S’은 while() { ,,, } while안의 Scope이다.
- Q. if-else의 statement 추론 규칙은?
    
    $\frac{S \vdash e:bool \text{ S' is the scope inside the if block }S'\vdash WF\{stmt_1, ..., stmt_n\} \text{ S''is the scope inside the else block } S''\vdash WF(stmt_{n+1}, ..., stmt_m)}{S\vdash WF(if(e)\{stmt_1,...,stmt_n \}\space else\space \{stmt_{n+1}, ..., stmt_m\})}$S’ : if 안에 들어갈 stmt들 (1~n)
    
    S’’ : else 안에 들어갈 stmt들 (n+1~m)
    

## 컴파일러가 타입 검사하는 방법

---

1. 우선 타입 검사말고 **스코프 검사를 먼저**한다.
2. 각 statement에 대해
    1. subexpression에 대해 타입 검사한다.
    2. 자식 statement에 대해 타입 검사한다.
    3. 전체적인 well-formedness를 검사한다.

### 예제

![스크린샷 2022-05-27 11.28.31.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.28.31.png)

### AST에서 타입 검사

재귀적으로 이루어진다.

1. 앞서 한 것처럼 scope별로 symbol table을 만든다. → scope checking
2. DFS로 들어가서 subexpression을 타입 검사한다.
    
    이 때 scope checking 과정에서 형성된 symbol table을 이용한다.
    

### AST 타입 검사 예시

1. scope checking의 결과 symbol table이 만들어진다. (이 예제에서는 형식 매개변수로 선언된 a, b이네?)
    
    ![스크린샷 2022-05-29 20.58.36.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-29_20.58.36.png)
    
2. DFS로 들어가서 subexpression을 타입 검사한다.
    1. 먼저 각 변수에 대해 scope information을 이용해 선언된 올바른 타입을 가지는지 확인한다.
        
        ![스크린샷 2022-05-27 11.31.44.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.31.44.png)
        
        - Current Symbol Table에서 a, b의 Type이 뭔지 가져온다.
    2. 그 부모 노드에서 비교문에 사용될 수 있는 타입인지, 그리고 그 결과가 bool인지 확인한다.
        
        ![스크린샷 2022-05-27 11.36.09.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.36.09.png)
        
    3. cond는 밑에가 bool이라 inherited되어서 bool이다
3. 자식 statement의 타입 검사를 한다.
    1. 자식 statement는 블럭 안에 있겠지? 블럭에서 안으로 쭉 들어가면 id를 먼저 만난다.
        
        ![스크린샷 2022-05-27 11.37.56.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.37.56.png)
        
        scope information을 이용해서 선언된 타입을 올바르게 사용하는지 확인!
        
    2. 그 다음 찾아 내려가보니 id, num을 만났다. 이것도 변수 올바른 타입인지 확인!
        
        ![스크린샷 2022-05-27 11.41.22.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.41.22.png)
        
    3. 더하기 연산에 대해서 올바른 타입인지 확인!
        
        ![스크린샷 2022-05-27 11.42.57.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.42.57.png)
        
    4. assign 연산에 대해서 Wellformed인지 확인! (양 쪽이 같은 타입이어야겠네?)
        
        ![스크린샷 2022-05-27 11.43.29.png](10%20Semantic%20Analyzer%20Type%20checking%20f4b3efe77cd949a3b0a7884096a1c756/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_11.43.29.png)
        
        - e1 : a → int , e2 : a + 1 → int
    5. block, while까지 올라간 이후에 WF(well-formedness)를 반환!!
    

함수 선언 및 리턴에 대한 예제는 강의 자료를 참고해서 직접 해볼 것