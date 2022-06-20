# 11. Intermediate Code Generation

## Intermediate Code Generator

- A **high-level** intermediate representation(parse trees or AST) → A **low-level** intermediate representation.

### 왜 Intermediate Representation 이거 씀?

1. 이해/최적화 하기 쉽다.(Easy-to-understand/optimize)
    - 머신레벨 Code보다 Optimaizaiton(최적화)하기 쉽고 Clear하다.
    - 머신 Code는 최적화하기에 너무 많은 Constraints가 있다.
2. Translate하기 쉽다.(Easy-to-be-translated)
    - High-Level Code와 비교해서 머신레벨 Code와 더 비슷하게 생겼다.
    - 그래서 Intermediate Representation → Machine-Level Code로 바꿀때 **Low Cost**가 든다.

High Level Code → Machine Level Code로 바로 바꾸면 상당히 어렵다.

고로 중간에 Intermediate Representation을 거쳐서!! 바꾸는거임

### Intermediate Representation이란?

- 보통 **하나의 Compiler**는 **여러개의 Intermediate Representation**을 갖고 있다!
- 각각의 Intermediate Representation은 다른 Information/Characteristic(정보, 특징)을 가지고 있다.
    
    ![IMG_5C1984195464-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_5C1984195464-1.jpeg)
    

### Intermediate Representaion 종류 2가지

1. AST(Abstract Syntax Tree)
    - High-Level Intermediate Representation
    
    ![IMG_24731F77FF2E-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_24731F77FF2E-1.jpeg)
    
2. TAC(Three Address Code)
    - Low-Level Intermediate Representation
        
        ![IMG_6DDC3DA0527E-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_6DDC3DA0527E-1.jpeg)
        

### AST(Abstract Syntax Tree)

- Parse Tree와 비슷해보이지만 Parsing Detail을 빼고 그린 Tree임.
    
    ![IMG_78EEE1C4F094-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_78EEE1C4F094-1.jpeg)
    

![IMG_E7098BA9F51E-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_E7098BA9F51E-1.jpeg)

### TAC(Three Address Code)

- Temporary Variable을 이용해서 나타낸다!
    
    ![IMG_CA6991DD3C13-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_CA6991DD3C13-1.jpeg)
    
- **TAC의 구성요소 (2가지)**
    1. Operands(= address)
        1. Explicit Variable(ID) : A, B, C
        2. Constant : 1
        3. Temporary Variable : t0, t1, t2
    2. Operators(= instruction)
        - +, -, *, = …

### TAC: Variable Assignment

1. **Copy Operation**
    - Explicit or Temporary Var = 모든 Operand (이런형태)
    - Ex) A = 1, t0 = 2 ..
    - Operand가 무조건 2개…

1. **Binary Operation**
    - Explicit or Temporary Var = Operand      **Binary Operator**      Operand
    - Binary Operator
        1. Arithmetic Operator + - * / %
        2. Boolean Operator && ||
        3. Comparison Operator == != ..
    - Ex) A = B + C, t0 = A || 1, A = t0 ≤ 0 …
    
2. **Unary Operation**
    - Explicit or Temporary Var = Unary Operator  Operand
    - Unary Operator : -, !
    - Ex) t0 = -2,   t0 = ! true ;

### TAC : Control Flow Statements

1. Unconditional Jump With Label(ex, $L_n$)
    
    ```c
    goto Ln
    ...
    Ln :
    ...
    ```
    
2. Conditional Branch(Conditional Jump)
    - `if x GOTO Ln`
    - `if not x GOTO Ln`

### TAC : More..

1. Call Procedure
    
    ![IMG_3D526474A49E-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_3D526474A49E-1.jpeg)
    
2. Array Operation
    
    A[i] =  B
    
    A = B[i]
    
3. Return Statement
    
    return x
    

### TAC Examples

![IMG_D5262507C08B-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_D5262507C08B-1.jpeg)

---

![IMG_658FF84F5C6B-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_658FF84F5C6B-1.jpeg)

---

![IMG_832761C42144-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_832761C42144-1.jpeg)

### TAC 표현하기 : Quardruple

- Quadruple → (op, arg1, arg2 , result)
- Linked List로 표현!
    
    ![IMG_655668FD7F1A-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_655668FD7F1A-1.jpeg)
    

---

![IMG_5168E11E250F-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_5168E11E250F-1.jpeg)

---

### AST → TAC

![IMG_AF5AA15A1D9B-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_AF5AA15A1D9B-1.jpeg)

---

![IMG_1D5285266DC0-1.jpeg](11%20Intermediate%20Code%20Generation%20c718656c8bf24ad8ae7c5a1bbb91a6e7/IMG_1D5285266DC0-1.jpeg)