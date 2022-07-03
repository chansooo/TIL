# 8. Syntax Analysis: SLR parsing Implementation

## SLR parsing table (GOTO & ACTION table)

---

CFG와 viable prefix DFA가 있을 때 SLR parsing table을 만드려고 한다.

![스크린샷 2022-05-17 10.50.02.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_10.50.02.png)

### SLR parsing table 만들기

1. GOTO table을 viable prefix DFA에서 각 state와 non-terminal A에 대해 만든다.
    
    state qi에서 non-terminal A를 input으로 하는 transition하면 state qj로 이동 → GOTO(qi, A)=qj 
    

![스크린샷 2022-05-17 10.56.03.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_10.56.03.png)

ex) state 1에서 E로 transition → state 2, state 1에서 T로 transition → state 3

...

1. ACTION table을 viable prefix DFA에서 각 state와 terminal a에 대해 만든다.
    1. state qi에서 terminal a를 input으로 하는 transition하면 state qj로 이동 
        
        → ACTION(qi, a) = shift&goto qj
        
        ![스크린샷 2022-05-17 11.03.16.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.03.16.png)
        
    2. qi가 X→$\alpha.$ 을 포함하고 a가 Follow(X)에 포함될 때 
        
        ACTION(qi, a) = reduce by X → $\alpha$ (production)
        
        ![스크린샷 2022-05-17 11.03.42.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.03.42.png)
        
    3. 둘다 아닐 경우 error이다.
        
        ![스크린샷 2022-05-17 11.10.11.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.10.11.png)
        

### SLR parsing table을 이용한 parsing 과정

- 매번 viable prefix DFA를 사용하는 방식
    
    ![스크린샷 2022-05-17 11.18.53.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.18.53.png)
    
    - state 1에서 다음 input이 id일 때 S5 (shift & goto 5)를 실행한다.
    
    ![스크린샷 2022-05-17 11.19.57.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.19.57.png)
    
    - left substring인 `id`가 viable prefix인 것을 확인한다.
    - 현재 state 5에서 next input symbol `*` 가 들어오면 Reduce by 5 (T→id)가 결정된다.
    
    ![스크린샷 2022-05-17 11.30.57.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.30.57.png)
    
    - left substring인 `T` 가 viable prefix인 것을 확인한다.
    - 현재 state 3에서 next input symbol `*` 가 들어오면 shift and goto 6가 결정된다.
    
    ![스크린샷 2022-05-17 11.32.27.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.32.27.png)
    
    - left substring인 `T*` 가 viable prefix인 것을 확인한다.
    - 현재 state 6에서 next input symbol `id` 가 들어오면 shift and goto 5가 결정된다.
    
    ![스크린샷 2022-05-17 11.33.30.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.33.30.png)
    
    - left substring인 `T*id` 가 viable prefix인 것을 확인한다.
    - 현재 state 5에서 next input symbol `$` 가 들어오면 Reduce by 5 (T→id)가 결정된다.
    
    ![스크린샷 2022-05-17 11.34.26.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.34.26.png)
    
    - left substring인 `T*T` 가 viable prefix인 것을 확인한다.
    - 현재 state 3에서 next input symbol `$` 가 들어오면 Reduce by 3 (E → T)가 결정된다.
    
    ![스크린샷 2022-05-17 11.35.19.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_11.35.19.png)
    
    - left substring인 `T*E` 가 viable prefix인 것을 확인한다.
    - 현재 state 8에서 next input symbol `$` 가 들어오면 Reduce by 2 (E → T * E)가 결정된다.
    
    ...
    

그런데 이런 방식은 비효율적이다.

따라서 **스택**을 이용하고자 한다.

### 스택을 이용한 SLR parsing 과정

매번 input symbol마다 viable prefix DFA를 확인하지 않고, 스택과 GOTO 테이블을 이용한다.

- `push` : 다음 state로 이동할 때
- `pop` : 현재 state가 reduce될 때
    - pop이 되면 스택의 다음 원소로 현재 state가 변경된다.
    - A→$\alpha$ 로 reduce되었을 때, GOTO(현재 state, A)를 이용해 새로운 state를 `push` 한다.
    - 예시
        
        ![스크린샷 2022-05-17 12.20.51.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_12.20.51.png)
        
    

⇒ Reduce와 Shift를 반복해서 dummy start symbol까지 도달하면 accept된다!

![스크린샷 2022-05-17 12.25.03.png](8%20Syntax%20Analysis%20SLR%20parsing%20Implementation%20752a5724533242efb7bc13724584c9a7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-17_12.25.03.png)