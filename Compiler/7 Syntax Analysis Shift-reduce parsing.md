# 7. Syntax Analysis: Shift-reduce parsing

## Shift-reduce parsing

---

1. 기존 string을 두 개의 substring으로 나눈다.
    
    ![스크린샷 2022-05-09 14.09.12.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_14.09.12.png)
    
- $\alpha$ : non-terminal & terminal sequence
- $\beta$ : terminal sequence
- | : Splitter
1. 각 step에서 Shift 또는 Reduce를 진행한다.
    - **`Shift`**
        
        : splitter를 오른쪽으로 움직인다.
        
        ![스크린샷 2022-05-09 14.13.05.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_14.13.05.png)
        
    - **`Reduce`**
        
        : 왼쪽의 substring에서 suffix(제일 오른쪽)부분과 일치하는 production의 RHS(right-hand side)이 있다면 이전 Non terminal로 교체한다.
        
        위에서 Suffix는 b, Yb, aYb, XaYb이다.
        
        ![스크린샷 2022-05-09 14.13.30.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_14.13.30.png)
        
        위의 예제에서 왼쪽 substring의 suffix는 XaYb, aYb, Yb, b가 될 수 있다.
        
    
- 예제
    
    ![스크린샷 2022-05-09 14.16.01.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_14.16.01.png)
    

> splitter는 production의 or와 다른 의미이니 주의할 것
> 

### Stack에 넣기

- Stack에는 Splitter의 왼쪽 놈들이 들어간다.
- Shift : Stack에 Push 한다.
- Reduce : Reduce될 String들을 Pop한뒤 →  Result를 Push한다.
- Stack 마무리 : Input Pointer가 End of Input에 있고, Stack에 Start Symbol만 있으면 Accept.

### 2개의 Conflict(Problem) - 어떻게 다뤄야할까??

1. **Shift-reduce conflict**
    
    : 각 step에서 shift와 reduce를 동시에 할 수 있을 때 무엇으로 결정할 것인지
    
2. **Reduce-reduce conflict**
    
    : 각 step에서 두 개 이상의 reduction이 가능할 때 무엇으로 결정할 것인지
    

### 4-1 Handle & Viable Prefix.

### **Handle : Production의 결과물과 동일한 Substring**

- Production의 RHS(Right Handle Side)와 완벽하게 일치하는 Substring
- Handle은 항상 left substring의 suffix이다.
- Handle은 항상 Left Substring의 Suffix이다.  의 예시.
    
    ![스크린샷 2022-05-09 15.14.00.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_15.14.00.png)
    
    $\alpha Y$는 $X$로 Reduce될 수 있기 때문에 다음번의 Handle이다.
    
- Handle은 Reduce되는 놈이다.
- Handle 예제
    
    ![IMG_67ACC1E00A81-1.jpeg](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/IMG_67ACC1E00A81-1.jpeg)
    

### Viable prefix(선행자)

: Shift-Reduce Parsing 진행 과정에서 나타나는 왼쪽 부분의 첫글자부터 마지막글자까지(Left Substring)

- 예제
    
    ![스크린샷 2022-05-09 15.18.00.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_15.18.00.png)
    

### Viable prefix를 이용한 Parser의 대략적인 과정

- 예제
    
    ![스크린샷 2022-05-09 15.30.51.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_15.30.51.png)
    
- viable prefix는 production의 RHS(Right hand side)의 prefix들이 이어진 형태이다.
    
    ex) F * id | 상태에서 viable prefix는
    
    T → F * T의 RHS의 prefix인 ‘F *’와
    
    F → id의 RHS의 prefix인 id의 concatanation이다.
    
- if ( 가장 오른쪽 prefix이 handle일 때 ) → `reduce`
    
    else if ( handle은 아닌데, RHS의 prefix일 때 ) → `shift`
    
    else `reject`
    

### Finite Automata로 Viable prefix 인식

위에서는 viable prefix를 이용해 reduce, shift, reject 중에서 결정할 수 있다는 것을 보았다.

그렇다면 이를 오토마타로 표현할 수 있겠네?

left substring

:  $prefix_1prefix_2...prefix_n$

1.  $prefix_n$ 은 Handle이거나 → 만약 $prefix_n$이 Handle이면  
$prefix_1prefix_2...prefix_{n-1}$까지는 Reduce될 Production이 존재 X
만약 중간에 Reduce가 될 수 있다면 Splitter가 거기 있으면 안된다. 
2. 만약 Handle이 아니면 $prefix_{n+1}$, $prefix_{n+2}$ 이런식으로 제일 오른쪽 prefix를 포함한 앞의 놈들 중 Handle이 되는 놈이 있을때까지 Shift한다.
- Example
    
    ![IMG_51F6F6720954-1.jpeg](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/IMG_51F6F6720954-1.jpeg)
    

예제를 이용해 prefix 경우의 수를 생각해보자

![스크린샷 2022-05-10 15.34.12.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-10_15.34.12.png)

가정 : prefix1 = T +

가능한 prefix2 = T +, F *, (

→ prefix1 뒤에 나올 수 있는 녀석들 중에서 handle은 아닌 녀석들이 prefix2가 될 수 있다.

T + 뒤에는 E가 나와야 되므로 E에서 진행 가능한 production의 RHS는

T + E, T, F * T, F, (E), id 가 가능

handle이 아닌 prefix는 T +, F *, (가 될 것이다!!

> 이 때 주의 깊게 볼 것은 F의 production
> 
> 
> : (E), id는 handle이고, (E는 왜 안되냐면 (E까지 나오면 무조건 (E)이기 때문에 (E는 안됨!
> 
> : (E T ... 이런식으로 되면 아예 Accept될 수가 없기 때문에 ( E 는 prefix가 될 수 없다.
> 

prefix은 handle을 포함할 수 있음! (가장 오른쪽의 prefix)

따라서 prefix1 = T + 일 때 prefix2로의 이동에 대한 오토마타는 아래와 같음

<$prefix_1$에서 $prefix_2$로의 Transition Graph>

![스크린샷 2022-05-10 15.59.55.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-10_15.59.55.png)

만약 handle을 포함한다면 어떻게 될까?

$prefix_{n-1}=T+$ 일 때

$prefix_n$ 은 앞서 { T+, F*, ( }와 함께 handle들도 포함할 것이다.

따라서 { T+, F*, (, T, F, (E), id, T+E, F*T } 들이 $prefix_n$으로 가능하며 이를 오토마타로 표현하면 다음과 같다.

< $prefix_{n-1}$에서 $prefix_n$으로의 Transition Graph >

![스크린샷 2022-05-10 16.02.29.png](7%20Syntax%20Analysis%20Shift-reduce%20parsing%20ed9d4c23d8d94a6f827cd57b419863cf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-10_16.02.29.png)

→  Number of State : 유한개

→ Number of Transitions : 유한개   b,c input symbol of type 은 유한하니까! 

### 4-2

어떻게 Handle이 있는지 없는지 알 수 있을까?

### Viable prefix를 이용한 Handle 판별 정리

viable prefix를 판별하는 오토마타가 있으니까, 이 녀석을 이용해서 parser의 진행을 설명해보자

---

지금 $\alpha\beta|b\omega$ 상태일 때 left substring은 $\alpha\beta$ 겠죠?

그렇다면 이제 무엇을 할지 결정해보자 (집중해야하는 구간)

1.  X→$\beta$ production이 존재 → $\beta$는 string $\alpha\beta b\omega$의 handle
    
     && $\alpha$X가 viable prefix → reduce하면 그 다음 진행 가능하겠네?
    
    ⇒ `reduce` 진행시켜
    
2. handle은 없는데, 그 다음 녀석인 b를 포함한 $\alpha\beta b$가 viable prefix가 되네?
    
    ⇒ `shift` 진행시켜
    
3. 둘다 아니다 → 해당 문법에서 더 이상 진행불가
    
    ⇒ `reject` 시켜
    

---