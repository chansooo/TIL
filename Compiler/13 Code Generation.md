# 13. Code Generation

## Runtime Environment

정의 : 런타임에서 High-Level Structure를 지원하기위해 사용되는 Data Structure

1. What do **Objects** look like in Memory??
2. What do **Functions** look like in Memory?
3. Where in Memory should **Objects and Functions be Placed**??

---

## 1. Object Representation

- Object들은 Memory상에서 N-bytes Alligned한다.
    
    ![IMG_A954E00F2024-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_A954E00F2024-1.jpeg)
    
    ---
    
- **Allignment : Start Address가 Allignment의 배수가 돼야한다!!!**
    
    ![IMG_9A661B9B7538-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_9A661B9B7538-1.jpeg)
    

---

- Array를 Mem에 넣어보자~
    - 각 언어마다 Array를 Mem에 저장하는 방식이 다르다.
    - C언어 → 그냥 저장시켜
        
        ![IMG_852C0E0A7FDE-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_852C0E0A7FDE-1.jpeg)
        
    - JAVA → 제일 처음에 Size Information도 같이 저장시켜
        
        ![IMG_85BA4CDE9E0C-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_85BA4CDE9E0C-1.jpeg)
        

---

- Multi-Dimensional Array
    - C언어 → 순차적으로 쭉~
        
        ![IMG_8387342738AB-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_8387342738AB-1.jpeg)
        
    - JAVA →Linked List형식!
        
        ![IMG_AA305FC9F783-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_AA305FC9F783-1.jpeg)
        

---

## 2. Function Representation

1. 함수간 관계(Function Call같은것)

2. 함수의 정보들 어떻게 다룰것인가(parmeter, return val, return address..)

### 1. 함수간 관계 → Stack을 이용!

### Activation(Lifetime)

**정의** : 함수 F가 invoke(호출..느낌) 되면 Activation of F라고한다.

- Activation of F : 함수에 있는 모든 Statement가 실행되는 LifeTime
- 만약 F가 Q를 호출한다면 Q가 다 실행될때(Q의 Activation)까지가 F의 LifeTime에 들어간다.
    - 다시말해 Activation of F는 Activation of Q를 포함한다!!

**Activation Tree → Last In First Out… → Stack으로 표현가능!**

![IMG_5232923729BC-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_5232923729BC-1.jpeg)

---

**Stack**

1. Pending : 기다리는..
    
    ![IMG_ED8DB466DAEF-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_ED8DB466DAEF-1.jpeg)
    
2. 더이상 내려갈데 없으면 Pop
    
    ![IMG_BB1BC16E42C8-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_BB1BC16E42C8-1.jpeg)
    
3. 또 가지쳐~
    
    ![IMG_4E0064D5A16D-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_4E0064D5A16D-1.jpeg)
    
4. 끝난 놈들은 Pop
    
    ![IMG_4BDE51D3CBE9-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_4BDE51D3CBE9-1.jpeg)
    
    1. main까지 Pop
        
        ![IMG_81A4AA75D5EE-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_81A4AA75D5EE-1.jpeg)
        

---

### 2. 함수의 Information들 관리

### Activation Records

정의 :  Function을 돌리기위해 필요한 정보들

1. Input Parameter
2. Space for F’s Return Value
3. Control Link
4. Machine Status Prior to Calling F
5. F의 Local, Temporary Var

---

![IMG_0A38B86E2AE0-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_0A38B86E2AE0-1.jpeg)

- fib(1)의 Return Val은 아직 안정해져서 (result)라고 표시.
- Control Link : main에서 호출해서 main의 Start Address가 들어감
- Saved Machine Status : fib(1)이 수행된 다음 a = t0 에 들어간다.

---

**< main에서 fib(1)을 Call 했을 때 상태 >** 

![IMG_8707F4F82CDC-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_8707F4F82CDC-1.jpeg)

---

**< fib(1)이 실행될 때 >**

![IMG_C60262C0AEBD-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_C60262C0AEBD-1.jpeg)

---

**< fib(1)이 실행된 다음! >**

![IMG_594AF27C6139-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_594AF27C6139-1.jpeg)

---

## 3. Object와 Function을 Mem에 저장하기!

### Memory Layout

1. Data
2. Code 

부분으로 나뉜다!

![IMG_60FC88D95E87-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_60FC88D95E87-1.jpeg)

---

### Global Variable

- Global Variable에 대한 참조는 Same Object를 가리킨다.
- Global Variable은 Activation Record에 저장될 수 없다
- 프로그램이 돌아갈때 계속 이 정보를 갖고있어야한다.
- **그래서 Fixed Address에 저장된다!! → Statically Allocated된다.**
    
    ![IMG_C07845C5F5F3-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_C07845C5F5F3-1.jpeg)
    

---

### Activation Record

- Stack을 통해 관리된다.
- Stack은 High Address → Low Address로 쌓인다!!
    
    ![IMG_8849B391819B-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_8849B391819B-1.jpeg)
    

---

### Dynamically-Allocated Data

- malloc같은 동적할당 하면 Data부분에 Dynamic하게 할당되고 Free된다.
- Heap영역에 동적할당된다.
- Low Address → High Address방향으로 채워진다.
- **Activation Record와 겹치지 않게 반대방향으로 채우는거임!!!**
    
    ![IMG_F01C4645BC21-1.jpeg](13%20Code%20Generation%20d3c2533222c14d7eb2a36928689a8d69/IMG_F01C4645BC21-1.jpeg)
    

---

### 결론

- 위에서 설명한것들은 다 **Runtime**에 High-Level Structure를 지원하기 위해 사용된다.
- 컴파일러는 **컴파일타임**에 Code or Data의 Memory Layout을 결정하고  Target Data가 있는 Location에 올바르게 접근하는 Code를 Generate한다.
- **컴파일러는 컴파일 시간에 코드와 데이터의 메모리 레이아웃을 결정하고 대상 데이터의 위치에 올바르게 액세스하는 코드를 생성합니다.**