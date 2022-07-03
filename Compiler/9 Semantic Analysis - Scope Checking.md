# 9. Semantic Analysis - Scope Checking

## Common Semantic Error

1. Variable 쓰기 전 무조건 선언 되어야함.
(globally or locally)
    
    → Scope Checking
    

```c
void foo(){
	a = 3;
	int a;
}
```

1. Variable은 선언 한번만 되어야함(locally)
→ Scope Checking

```c
void foo(){
	int a = 3;
	int a = 2;
}
```

1.  모든 function은 선언 한번만 되어야함(globally)
→ Scope Checking

```c
void foo() {,,,}
void foo() {,,,}
```

1. 모든 vairable은 올바른 type으로 써야함.
→ Type Checking

```c
int a = 3.5; // 정수로 써야함
char b = 3.5; // char로 써야함
c[3.5] = 0; //index는 정수값!
```

1. 모든 function은 argument 맞게 써야함.
→ Type Checking

```c
void foo(int a){ ... }
foo("compiler") //정수값을 argument로 받아야함
void foo2(int a){ ... }
foo2(1,2); // argument는 하나만 받아야함.
```

### 그러면 어떻게 Check할까?

- Scope Check & Type Check
- Scope : Locally, Globally

### Scope Checking

- Scope : 어떤 부분에서 쓸 수 있는지
    
    ![IMG_9EE43D37FBD5-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_9EE43D37FBD5-1.jpeg)
    
- 두번째 code에서 `int a = 2;` 는 저 빨간 Scope에서 사용됨.
- `int a = 3;` 은 파란 Scope안에서 사용됨.

Two types of scope

1. **Static Scope** 
    - 대부분의 Language에서 사용. (ex. () {} …)
    - Scope는 Program Text의 Pyhsical Structure에 영향을 받는다.
2. **Dynamic Scope(Special)**
    - 특정 언어에서만 사용(Lisp, SNOBO)
    - Scioe는 Execution(실행) of the program에 영향을 받는다.
    - Ex. 가장 최근에 Declare된것이 사용된다.

예제.

```c
void foo() { int a = 3; }
...
int a = 2;
foo();
print(a);
```

**Static Scope** 

결과 : 2 (왜냐하면 int a = 3은 foo안에서만 사용됨)

**Dynamic Scope**

결과 : 3 (왜냐하면 Execution 에 따라서 결정돼서 int a = 3 이 가장 최근에 선언돼서 a = 3이된다.)

---

### Static Scope에 집중해서 볼거임!

1. Function Declaration
    - int funcName(…)
2. Class Declaration
    - class className {…}
3. Variable Declaration
    - int varName
4. Formal Parameters
    - int func(int formal1, int formal2) { }

### The most-closely nested Scope Rule

- identifier는 가장 가까운 nested scope에서 선언된 identifier와 match된다.
    1. identifier는 사용되기 전에 선언 되어야 한다.
        
        **Example**
        
        ![IMG_6620DDD28838-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_6620DDD28838-1.jpeg)
        
        ---
        
        **Exception 1**
        
        ![IMG_D920967A22B1-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_D920967A22B1-1.jpeg)
        
        Function이 선언되기 전에 쓰고 다음에 function을 선언해줘도 된다.
        
        ---
        
        **Exception 2 객체지향언어에서..**
        
        ![IMG_0DD069BF959A-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_0DD069BF959A-1.jpeg)
        
        foo()는 Class Test Scope에서도 선언 안됐고 Global Scope에서도 선안 안됐지만 상속된 놈한테 있기 때문에 정상 작동한다. 
        
        ---
        
    
    ### Implementation of Scope Checking → AST & Symbol Table
    
    1. 코드
    
    ```c
    int foo() {
    	int a;
    	int b;
    	{ 
    			int a;
    			{
    					int b;
    			}
    	}
    }
    ```
    
    1. **AST**
        
        1.  시작: 새로운 Declaration 저장.
        
        ![IMG_21E80659BD02-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_21E80659BD02-1.jpeg)
        
        ---
        
        1. New Scope(Block)가 Detect 될때마다 Current Scope를 증가시켜주면서 내려간다!
            
            ![IMG_B1DFBC87E881-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_B1DFBC87E881-1.jpeg)
            
        
        ---
        
        1. 현재 Block에서 만약 새로운 선언(Declaration)이 있으면 Current Scope와 같이 Table에 정보를 저장한다!
            
            ![IMG_076D9337A6C2-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_076D9337A6C2-1.jpeg)
            
        
        이런식으로 내려가면서 Current Scope 값을 증가시키고 새로운 Declaration이 있으면 Table에 저장하면서 쭉 내려가다가
        
        ---
        
        1. Child Node가 더 이상 생성되지 않으면 Exit the Scope를 진행!
            
            ![IMG_A0547D70D9E1-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_A0547D70D9E1-1.jpeg)
            
        
        Current Scope값이 0 이되면 Exit!
        
    
    ---
    
    Example 1.
    
    ![IMG_95FE3AA5B901-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_95FE3AA5B901-1.jpeg)
    
    - 만약 Current Scope가 3인 곳에서 b를 쓴다면?
        
        → Table에서 Current Scope가 3인 곳에 선언이 되어 있는지 확인한다! 
        
        → 있다! → Good.
        
    
    ---
    
    Example 2.
    
    ![IMG_5CFEC7445B69-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_5CFEC7445B69-1.jpeg)
    
    - 똑같이 Table에서 같은 Current Scope에 해당 변수가 선언되어 있는지 없는지 확인한다!
        
        → 없다!
        
        → Parent Scope에 있나 없나 확인한다!
        
        → 오 있다!! → 그러면 저걸 쓰면된다! (Table entry에 가장 가까운 Scope에 선언되어 있는 놈 갖다 쓰면됨)
        
    
    ---
    
    Example 3.
    
    ![IMG_835575DE9EB6-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_835575DE9EB6-1.jpeg)
    
    - Table에서 같은 Current Scope에 있나 없나 확인! → 없다.
    - Table에서 Parent Scope에 Declare 됐는지 안됐는지 확인 → 없다
    - Error!!!

### While Traveling AST의 문제점.

1. **Ambiguity**
    
    ![IMG_884C90A743CA-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_884C90A743CA-1.jpeg)
    
    - 위에서 a 와 c 는 다른 Scope에 있지만 Same Level of Scope로 인식된다.
2. **Inefficiency**
    - 만약 int c; 밑에 c = 0 같은게 들어가 있다면 c = 0 때문에 전체 Table Entry를 매번 Search 해야한다.

---

## Scope Checking (more)

![IMG_F954425A74F4-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_F954425A74F4-1.jpeg)

### Ambiguity 해결하기

- Symbol Table을 각 Scope마다 하나씩 만든다!
    - 그리고 nesting structure를 표현해준다.

![IMG_F954425A74F4-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_F954425A74F4-1.jpeg)

![IMG_3EBE8FFA8F9C-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_3EBE8FFA8F9C-1.jpeg)

< AST Node 방문 순서 & Table 만드는 순서> → 슬라이드 보면서 봐보기!

![IMG_178C5139F351-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_178C5139F351-1.jpeg)

### Inefficient 해결하기

- Table Entry 탐색할 때 제일 먼저 Current Symbol Table을 확인한 뒤 연결되어 있는 Parent Symbol Table을 순차적으로 탐색하면 Scope가 속한 곳만 쭉 탐색할 수 있다.

### Example 살펴보기.

Example 1.

![IMG_078A0FEBCD6F-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_078A0FEBCD6F-1.jpeg)

![IMG_EBF0AD190704-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_EBF0AD190704-1.jpeg)

---

Example 2.

![IMG_F99E8A1F05A8-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_F99E8A1F05A8-1.jpeg)

![IMG_4AC6505E3347-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_4AC6505E3347-1.jpeg)

---

Example 3.

![IMG_61BC4701A0D3-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_61BC4701A0D3-1.jpeg)

![IMG_9E575E9B907B-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_9E575E9B907B-1.jpeg)

---

 

### 그러면 위의 Exception들.. Function Call / Inherited Variable은 어떻게 해결할까?

### Multi-pass Scope Checking

1. Function Call
    - Function / Class name들의 정보들을 한번에 모아놓자!
        
        ![IMG_3E82E4F843B6-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_3E82E4F843B6-1.jpeg)
        
2. Inherited Variable → 이건 자기가 알아서 생각해보래..
    
    ![IMG_18CA8F8E12D5-1.jpeg](9%20Semantic%20Analysis%20-%20Scope%20Checking%2038dc24d7572745ebadadbbe4b4feedae/IMG_18CA8F8E12D5-1.jpeg)