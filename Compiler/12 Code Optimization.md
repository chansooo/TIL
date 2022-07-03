# 12. Code Optimization

```java
int x,y,z;
y = 10 + 100;
if(x==10) z = y;
else x = y;
y = 10;
```

## Basic Block

- 연속으로 실행할 수 있는 TAC의 최대 sequence

![IMG_649A4D888CA5-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_649A4D888CA5-1.jpeg)

## Control Flow Graph

- Basic Block의 Graph.
    
    ![IMG_DC9DD859D0F6-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_DC9DD859D0F6-1.jpeg)
    

### Code Optimization의 Type

1. Local : 오직 하나의 Basic block에서만 작동. 
2. Global : 전체 Program, Entire Control-flow Graph에서 작동.

### Local Optimization

- 오직 하나의 Basic block에서만 작동.
    
    ![IMG_C8A3695CD16C-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_C8A3695CD16C-1.jpeg)
    

### Global Optimization

- 전체 Program, Entire Control-flow Graph에서 작동.
    
    ![IMG_9C25FC83A7DB-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_9C25FC83A7DB-1.jpeg)
    

---

## Local Optimization Technique

### 1. Common Sub Expressions Elimination

ex) 

v0 = a op b

…

v1 = a op b  → **v1 = v0**

만약 v0, a, b가 저 두 코드 사이에 변화가 없었다면

v1 = v0라고 바꿔줄 수 있다!

![IMG_1F01E44B6E3A-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_1F01E44B6E3A-1.jpeg)

### 2. Copy Propagation → 오 근데 이렇게 바꾸는 건 의미가 없지않나? 응 밑에 바로 나와 기다려.

ex)

v0 = v1

이후.. 둘다 reassigned됐다면 

a = …. v0 ….  ⇒ a = … v1 …으로 바꿔줄 수 있음.

![IMG_24781C29787C-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_24781C29787C-1.jpeg)

### 3. Dead Code Elimination

![IMG_78D37515CFC4-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_78D37515CFC4-1.jpeg)

사용하지 않는 코드는 Dead한놈!! 지워버려~

### 4. Arithmetic Simplification

![IMG_E770B6BCA090-1 2.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_E770B6BCA090-1_2.jpeg)

산술 연산보다 비트연산이 훨씬 빨라!!

Ex) 

a * 4 ==  a<<2

a / 2 == a>>1

a * $2^n$ == a<<n

a / $2^n$  == a>>n

### 5.  Constant Folding

![IMG_F5B619173932-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_F5B619173932-1.jpeg)

- Compile하기전에 그냥 Constant value는 먼저 계산해놓으면 컴파일할때 또 계산안해도 되니까…

### Local Optimization 연습해보기~

![IMG_1D0EA0D8A8BD-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_1D0EA0D8A8BD-1.jpeg)

---

### ~Code Optimization part1(more)

## Local Optimization 어떻게 구현할까?

결론부터..

1. Common sub expressions elimination ⇒ Available Expression Analysis로!
2. Copy Propagation ⇒ Available Expression Analysis로!
3. Dead Code Elimination ⇒ Live Variable Analysis로!

---

### 1. Available Expression Analysis

- Common sub expressions elimination & Copy Propagation

**Available Expression** : 변수가 가지는 가장 최신의(up-to-date) Value

![IMG_F4DA508A25F7-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_F4DA508A25F7-1.jpeg)

![IMG_0CF7158B9148-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_0CF7158B9148-1.jpeg)

### Common subexpressions하는 과정.

1. Check Right hand Side.
    
    ![IMG_726564F29778-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_726564F29778-1.jpeg)
    
2. Replace!
    
    ![IMG_0B60B494C53A-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_0B60B494C53A-1.jpeg)
    

### Copy Propagation하는 과정

1. Available Expression에 b = a or b = constant number가 있는지 Check!
    
    ![IMG_94DFA44BE032-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_94DFA44BE032-1.jpeg)
    
2. 바꿔버려~
    
    ![IMG_34A3D6487886-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_34A3D6487886-1.jpeg)
    

### Available Expression 연습문제~

![IMG_59DCEA4E90C2-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_59DCEA4E90C2-1.jpeg)

---

### 2. Live Variable Analysis

- for Dead Elimination

**Live Variable** : 미래에 사용할 Value들만 가져간다..

![IMG_31845C9BD50C-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_31845C9BD50C-1.jpeg)

- 그러면 어떻게 Future를 예측할까..?
    
    → 맨뒤부터 쭉 이렇게 오면 된다!
    

- a = …. b …
    - 이러면 a는 …b…으로 written됐기때문에 a는 살아남지 못한다.
    - b는 쓰였기때문에 살아남는다!

#EX1)

![IMG_6C7E261A8D76-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_6C7E261A8D76-1.jpeg)

#Ex2)

![IMG_EACF32E59EA7-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_EACF32E59EA7-1.jpeg)

### Dead Code Elimination 하는 과정

1. 다음 Live Variable을 봤는데 왼쪽에 있는 변수가 없다? → 뒤에서 안쓰이는거임
    
    → Dead다! 없애버려~
    
    ![IMG_7F5747A94D98-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_7F5747A94D98-1.jpeg)
    

---

### ~Code Optimization Part2

## Global Optimization

### Global Dead Code Elimination

1. 제일 아래의 Basic Block에서 Live Variable 밑에서부터 만들어나가기.

![IMG_24C4BCF7E994-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_24C4BCF7E994-1.jpeg)

1. 제일 아래의 Block의 Live Variable올려서 다음 Block들 진행!
    
    ![IMG_F64EC9CA8615-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_F64EC9CA8615-1.jpeg)
    
2. 밑의 Block들 합쳐서 {b ,c} U {b}
    
    ![IMG_1D1C6CE4D93C-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_1D1C6CE4D93C-1.jpeg)
    
    1. Code 없는 Block들은 없애버려~
        
        ![IMG_2A6C5345B070-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_2A6C5345B070-1.jpeg)
        

---

### Loop가 있는 Code의 Dead Elimination하기.

1. 일단 맨 밑부터 똑같이하는데 Dead Code는 제거하지 않고 쭉 한다.
    
    ![IMG_9EB45939F407-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_9EB45939F407-1.jpeg)
    
2. 계속 또 진행한다.
    
    ![IMG_AB8ED48BB4C9-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_AB8ED48BB4C9-1.jpeg)
    
3. 계속 Loop를 돌다가 Live Variable 집합이 더이상 바뀌지 않을때까지 진행한다.
    
    ![IMG_3E8B4E6BE4D9-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_3E8B4E6BE4D9-1.jpeg)
    

→ 그러면 언제 바뀌지 않을까? **첫번째 Live와 마지막 Live가 같다면**!!! 그때 끝내면된다.

1. 그러면 이제 Dead를 진행시켜
    
    ![IMG_9F94A1F91E5D-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_9F94A1F91E5D-1.jpeg)
    
2. 결과물
    
    ![IMG_2569B89C3935-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_2569B89C3935-1.jpeg)
    

---

## Global Copy Propagation & Common Subexpression

### #Ex1)

1. 먼저 Local로 진행.**(b = 1+ b 왜 아닌지 이해안감)**

![IMG_C9AA23F14A7E-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_C9AA23F14A7E-1.jpeg)

1. 다음 Block으로 내려!
    
    ![IMG_321224B71C68-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_321224B71C68-1.jpeg)
    
    1. 여러개 Block 다음 하나의 Block으로 합쳐지면 **위의 Block들의 Available의 교집합**을 넣는다!
        
        ![IMG_3CB9B2903D74-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_3CB9B2903D74-1.jpeg)
        
    2. 결과
        
        ![IMG_FD331A6E53F4-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_FD331A6E53F4-1.jpeg)
        

---

### #EX2) Loop 가 있을때

1. Loop전까지는 Ex1에서 했던거마냥 쭉 내려와~
    
    ![IMG_87EBF3401069-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_87EBF3401069-1.jpeg)
    
    ---
    
2. 아무런 Optimization하지말고 일단 쭉 Available에 쳐넣어~
    
    ![IMG_49594C356B38-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_49594C356B38-1.jpeg)
    
    - 2번째 Block에서 b = a + b가 안들어가는 이유
        - 오른쪽에 있는  b는 Previous Value와 관련있기 때문에 안넣는거임.
        - 위에서 헷갈렸던 부분이랑 같지!\
    
    ---
    
3. 이제 다시 맨위로 올라가면서 {a = b+c}와 {a =1}의 교집합을 싹하면 아무것도 없으니까 {a = 1}삭제.
    
    ![IMG_6E9BA6FD2A8F-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_6E9BA6FD2A8F-1.jpeg)
    
    ---
    
4.  Change 없을때까지 계속 Loop해~ 결국 3번이랑 똑같음.
    
    ![IMG_3DC30F050038-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_3DC30F050038-1.jpeg)
    

→ 아무것도 Change하고 Copy해줄거 없음~!

---

### Some Optimizations are Global에서만 가능하고 Local에서는 안되는것들이 있음.

ex) While (솔직히 이해안감 ㅋㅋ)

![IMG_DF9A5819004F-1.jpeg](12%20Code%20Optimization%20de3feb56e9f144439791e2b1225700de/IMG_DF9A5819004F-1.jpeg)