# Syntax Analysis Part 1.

### Syntax Analyzer

- Check Validation
- Syntax Analyzer 동작 순서
    1. 주어진 Set of Tokens가 Valid 한지, 아닌지 검사! → Grammar적으로 오류 있나?
    2. Valid 하다면 Parse Tree를 만든다.
        - 이 Parse Tree는 Language의 Grammatical Sequence를 기반으로 만들어진다.
- Syntax Analyzer의 기능
    1. Check Valid or Not
    2. Create Parse Tree
    3. Detect Errors
- 의문??
    - 어떤 Rule로 Valid token set인지 판별할거임?
    - Valid vs Invalid token set 어떻게 구별할거임?

### Why We don’t Use Regular Language in Syntax Analyzer

- Regular Language는 Syntax를 묘사하기 충분하지 않음
- Ex) $(^n)^n$(0 ≤ n ≤ $\infty$) 
→ Regular expression으로 만들 수 없음. 왜냐면 무한대니까.. 아무리 Union으로 붙여봐도 무한대니까 만들 수 없음

### Context Free Language

- Context Free Grammars(CFG) : CFL을 describing 하는 표기법.
    - 구성요소
        1. Terminals - 소문자로 표현
            - 기본적인 Symbol.
            - token name = terminal
            - Terminal은 다른 걸로 바뀔 수 없다!
        2. Non-Terminals - 대문자로 표현
            - Syntactic Variable.
            - Non-Terminal은 다른 걸로 바뀔 수 있다!(Non-terminal or terminal)
        3. A Start Symbol
            - 하나의 Non-Terminal
            - 보통 Non-Terminal의 첫번째 Rule
        4. Production(→)
            - Replacement
    - Sequence of terminal, non-terminal, $\epsilon$ :    $\alpha,\beta,\omega$로 나타냄
        - ex) $\alpha$ = aAABBcddef 이런식으로 표현.
- CFG는 Recursive한 Structure를 expressing하는데 좋다.
- Example 1) $(^n)^n$(0 ≤ n ≤ $\infty$)
    
    Terminal : { ( , ) , $\epsilon$ }
    
    Non-Terminal : BALANCED
    
    Start Symbol : BALANCED
    
    CFG: BALANCED → (BALANCED) | $\epsilon$ 
    
- Example 2) Simple Arithmetic Operations
    
    Terminal: { *id , +, *, (, ) }*
    
    Non-Terminal: E
    
    Start Symbol : E
    
    CFG: E → E+E | E*E | (E) | id
    
- Example 3) Simple Function Call
    
    Terminal : { id, (, ) }
    
    Non-Terminal : S, E
    
    Start Symbol : S
    
    CFG : 
    
    S → id(E) | id()
    
    E → id,E | id 
    
- Example 4) Simple While Statement ex) while( A > B ) , while(A), while( )
    
    Terminal : { while, id, comparison, number, (, ) }
    
    Non-Terminal : S, E, T
    
    Start Symbol : S
    
    CFG:
    
    S → while(E)
    
    E → TcomparisonT | T | $\epsilon$ 
    
    T → id | number
    

- Derivation( ⇒ )
    - Replacement의 Sequence(순서)를 나타낸다.
    - ⇒* : Derivation을 0 or more times 수행한다. 라는 뜻
    - Example 2) Simple Arithmetic Operations
        
        Terminal: { *id , +, *, (, ) }*
        
        Non-Terminal: E
        
        Start Symbol : E
        
        CFG: E → E+E | E*E | (E) | id
        
    - Derivation
    E ⇒ (E) ⇒ (E + E) ⇒ (id + E) ⇒ (id + id)
    E ⇒* (id + id)
- A Rule for Derivation
    - Leftmost( ⇒$_l$$_m$) : 가장 왼쪽에 있는 non-terminal 부터 replace한다.
    - Rightmost(⇒$_r$$_m$) : 가장 오른쪽에 있는 non-terminal 부터 replace한다.

### Token Validation Test

- A sentinel form of a CFG
    - Derivation 과정에 있는 모든 sequence $\alpha$
- A Sentence of a CFG
    - Derivation의 결과인 only terminals 만 포함되어있는 sequence $\alpha$
    - non-terminal이 들어가있으면 Sentence아님!
- A Language of a CFG
    - L(G) → Language of CFG (context-free language)
    - L(G) = {$\alpha$ | $\alpha$ is a sentence of G}

## 결론! Check Valid or Not

### 주어진 Sequence(Given token sets)가 Grammar에 적합한지 어케 암?

- CFG를 통해 Derivation 해서 sentence가 될 수 있으면 주어진 Sequence는 Grammar에 Valid하다고 판정 할 수 있다.

---

## Parse Tree

- Leaf Nodes = terminals
- Non-Leaf Nodes = non-terminals
- inorder traversal method를 사용하면 Original Strings가 나온다!
- leftmost method를 쓰던 rightmost method를 쓰던 똑같은 Parse Tree가 나온다!!

### Efficient Parsing == Creating a Good CFG

1. Non-ambiguous(애매하면 안됨)
2. No Left Recursion
3. 각각의 non-terminal 

### Ambiguity 없애는 방법

- 솔직히 좀 어렵다.
- Just rewrite the ambiguous grammars based on disambiguating rules
- Ex) dangling-else → Ambiguous Problem
    - E → if E then E | if E then E else E | other인 CFG에서 Ambiguous하다.
    - 그래서 이걸 해결하려고 Disambiguating rule를 부여한다.
    - 각각의 else가 있으면 가장 가까운 then을 찾는다. 그러면 Matched를 쓸수있음.
    
    ![5.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/5.jpeg)
    
- Ex) Arithmetic expressions → Ambiguous Problem
    - Disambiguating Rule : * has a higher priority than +
    
    ![6.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/6.jpeg)
    
    E는 +만, T는 *만 Derive한다.
    

### Left Recursion 없애는 방법 → Made Right Recursion

- Parsing Techniques(top-down parsing)은 Left Derivation Policy 때문에 Left Recusive Grammar를 Handling못한다.
- Ex) A ⇒* A$\alpha$
- Ex) S → Aa|b, A→ Sb → Left Recursion
- 그러면 어떻게 Left Recursion 없앨까?? → By Right Recursion
- Answer) Rewrite Using Right-Recursion
- S→ S$\alpha$$_1$| S$\alpha$$_2$| ... | S$\alpha$$_m$|$\beta_1$|$\beta_2$|...|$\beta_n$
    
    Step1) A라는 새로운 Non-terminal 만들고 $\alpha_i$A 라는 Production Rule을 만든다.
    
    - A → $\alpha_1$A |  $\alpha_2$A | ... | $\alpha_m$A | $\epsilon$
    
    Step2) S에 $\beta_i$A 를 모든 $\beta_i$에 추가해준다.  
    (S에는 원래 S에 있던 terminal만 있던 놈에 새로운 Non-terminal을 뒤에 추가 해주기.)
    
    - S → $\beta_1$A | $\beta_2$A | ... | $\beta_n$A
    A → $\alpha_1$A |  $\alpha_2$A | ... | $\alpha_m$A | $\epsilon$

### Left Factoring 으로 공통된 Prefix 없애는 방법

- Syntax Analysis에서 같은 Symbol들을 prefix로 갖는 두개 이상의 생성 규칙이 있을 때 Syntax Analyzer가 어떤 생성규칙을 적용해야 할지 결정 할 수 없다.
- 따라서 다음 Symbol을 볼때까지 연기함으로써 해결가능.
- prefix의 공통 부분을 새로운 nonterminal을 도입함으로써 해결 가능.
- E → T + E | T

![7.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/7.jpeg)

---

### For Efficient Parsing: Create a Good CFG

1. Non-Ambiguous
2. No Left Recursion
3. Only One Choice of Production Starting from a specific input symbol. → Leftr Factoring 이용.

### Example)

- CFG G : DECL → DECL type id ; | DECL type id = id ; | $\epsilon$ (Non-Ambiguous임)
    - Step1) Right Recursion만들기
        - A → type id ; A | type id = id ; A | $\epsilon$
        DECL → $\beta$A = A
    - Step2) Left Factoring  사용
        - type id( ; A | = id ; A)  → 이런식으로 인수분해
        longest 공통 prefix = type id
        새로운 non-terminal B 도입.
        
        CFG G : 
        A → type id B | $\epsilon$
        B → ; A | = id ; A
        DECL → A

### Abstract Syntax Tree (AST)

- Parse Tree 처럼 보이지만 디테일한 요소들은 안보이게끔 만드는 TREE !!
- 우리가 지금까지 만들어 왔던 Parse Tree는 상당히 복잡하다.
- AST는 Parse Tree보다 Compact 하고 Easier to Use and Understand 하다 !
- 이제 줄여볼까???
- 용어~~(밑에 과정 보면서 설명하기)~~
    - Single-successor nodes : 하나의 child node만 가지고 있는 node.
    - Syntactic Detail 표현하는 Symbol : 괄호, 쉼표 같은거
- AST만드는 과정
1. Single-successor node 줄이기
2. Syntactic Detail인 Symbol 줄이기
    
    ** 괄호를 줄일 수 있는 이유 : 이 예에서 괄호의 뜻은 괄호안에 있는 더하기를 먼저 계산하고 그 다음 곱하기를 하라는 뜻임. 근데 이미 Tree를 보면 덧셈은 곱하기보다 Subtree에 있기 때문에 곱하기보다 먼저 수행 가능한거다. 그래서 괄호를 줄일 수 있다.
    
3. Operator(연산자) 와 Argument가지고 있는 Non-terminal 줄이기.

### Example) Make Abstract Syntax Tree(AST)

- CFG G : E → E | E * E | (E) | id (Ambiguous 없다고 가정)

![1.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/1.jpeg)

![2.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/2.jpeg)

![3.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/3.jpeg)

![4.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/4.jpeg)

### Semantic Actions

- Node 관점에서 Node 생성하는 Action.

![5.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/5%201.jpeg)

Ex) (id + id) * id

1. E ⇒ E * E                                                                   2.  ⇒ ( E ) * E

![1.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/1%201.jpeg)

![2.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/2%201.jpeg)

1. ⇒ ( E + E ) * E                                                             4. ⇒ ( id + id ) * id 

![3.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/3%201.jpeg)

![4.jpeg](Syntax%20Analysis%20Part%201%207025737300014a89a3a0b8fc6d9d9ac7/4%201.jpeg)

### Example) While문. 이건 PPT보면서 직접 해보기!!!

- 여기서 결과적으로 Single-successor node가 생기는데, 이건 나중에 배우니까 일단 넘겨!