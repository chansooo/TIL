# Syntax Analyzer part 2

### How to validate input token sets Automatically???

- Syntax Analyzer 컴퓨터가 하니까 특정 Algorithm이 필요하다.
1. Top-down Parsing(=LL parsing)
    - 첫번째 L은 left-to-right scan of input
    - 두번째 L은 leftmost derivation
    - top(root) to bottom(leaf) 순으로 만들어진다.
2. Bottom-up Parsing

---

### Top-down Parsing( LL parsing )

- 각 derivation에서 어떤 production이 선택되어야 할까?

### #1 Recursive descent (Method, Algorithm)

1. Start symbol부터 시작.
2. leftmost non-terminal부터 순서대로 rule을 적용시켜나간다.
3. backtracking(백트래킹)
    - 만약 derivation으로 새로운 terminal이 나오면 input과 비교한다
        1. 맞으면 → Input Pointer를 진행시킨다.(오른쪽에 있는 input으로 pointer를 옮긴다.)
        2. 틀리면 → 백트래킹 실행.
    - terminal이 나왔는데 input과 일치하지 않을 때
    - input과 일치해도 뒤에 유도되지 않은 식이 남아있을 때
4. Accept Reject
    - Accept : 모든 input character가 match되면 accept
    - Rejcet : 모든 production이 틀리면 reject
- 유도과정
    
    ![2022._4._13._0340_Microsoft_Lens.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/2022._4._13._0340_Microsoft_Lens.jpeg)
    

### Recursive descent 장단점

- 장점
    - 이해하기 쉽다
    - 구현하기 쉽다
- 단점
    - 백트래킹 해야 한다.
    - 매우 비효율적이다.

→ CFG에서 이미 [non-ambiguous, no left recursive, left factoring not needed]가 이미 보장되어 있으니까 굳이 경우의 수를 다 넣어서 Recursive descent를 비효율적으로 할 필요가 없긴함!

- LL(0) parser라고도 불림. ⇒ 다음 symbol을 보지 않아서!

---

### Predictive parsing (= LL(k) parsing)

- 첫번째 L : input을 left → right 방향으로 스캔한다.
- 두번째 L : leftmost derivation 적용!
- 괄호 안 k : 앞의 Input을 이용해서 예측한다. ⇒ “Prediction”
- 한단계 이후의 결과를 미리 예상하고 parsing path를 정하기 때문에 backtracking을 할 필요가 없어진다. → lookahead 라는 개념.

- LL(1) 이 보통 많이 쓰인다
    - Leftmost nonterminal A를 보고 LL(1) parser는 다음 input symbol a를 기반으로 production을 결정한다!
- 앞의 Recursive descent 방식 → LL(0) parsing이라고 부른다.
왜냐면 우리는 Recursive descent방식을 사용할 때 앞의 input symbol을 고려하지 않고 prodcution을 결정하기 때문에 LL(0)라고 부르는 것이다.

### Predictive parsing에서 No Backtracking인 이유

- CFG는 이미 non-ambiguous, left recursive, left fatored 다.
- Ex) S → aaA | abA | abB    input : ab
이때 LL(1)이면 이 CFG는 backtracking이 필요하다. 왜냐면 3개의 production 모두 a로 시작하니까!
LL(2)를 사용해서 ab를 읽어서 prediction을 진행해도 abA, abB때문에 또 backtracking을 해야한다.
그러면 LL(3)를 쓰면 되잖아!!!!!!!!!!!! 그치만.... k의 값이 올라갈수록 프로그램 복잡도가 존나 올라가서 
k를 늘리는 것 보단 CFG자체를 left factoring을 통해 저런 현상을 없애버리면 모든게 해결된다...
그래서 left factoring이 제대로 되어있으면 LL(1) Parsing으로 구현할 수 있다!!!!!!!!!!!!!!!!!!

 

### LL(1) parsing table with Example

![IMG_59C436DC3AA0-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_59C436DC3AA0-1.jpeg)

![IMG_1ADEA0975B2D-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_1ADEA0975B2D-1.jpeg)

- input : (id)$
- Derivation : 
1. input ( 를보고 derivation 시작
E ⇒ TE’ ⇒FT’E’ ⇒(E)T’E’

2. input ( 만났으니까 id를 보고 derivation시작
(E)T’E’ ⇒ (TE’)T’E’ ⇒ (FT’E’)T’E’ ⇒ (idT’E’)T’E’

3. input id 만났으니까 )를 보고 derivation 시작
 (idT’E’)T’E’ ⇒ (idE’)T’E’ ⇒ (id)T’E’

4. input ) 만났으니까 $보고 derivaiton 시작
(id)T’E’ ⇒ (id)E’ ⇒ (id)    → ACCEPT!!!!

### LL(1) Parsing Table 만들기

- First set( `First()`)
    - 해당 token의 set 중에서 비교대상의 이전 terminal을 찾아주는 함수.
    - First(A)라고 하면 A라고 하는 set 중에서 첫번째 Terminal을 찾아주는 것.
    - Ex) A→ aB | ac 가 있으면 First(A) = a 이다.
    - 규칙
        1. x가 terminal이면 First(x) = {x} 이다. (x밖에 포함못함!)
        2. X → $\epsilon$ 라는 production이 있으면 First(X)에 $\epsilon$을 추가해준다.
        3. $\alpha$ = $Y_1Y_2Y_3....Y_kx\beta$ and $\epsilon \in$ First($Y_i$) for all $i$ 이면 → First($\alpha$)에는 First set of A1 ~ An이 다 들어간다.
        근데 만약 Y중 하나라도 empty를 포함하지 않는다면 이건 성립 안한다.
        4. First($\alpha$) = First(x) if $\alpha = x\beta$
        5. non-terminal A의 first set : First(A) = {x|A⇒* $x\alpha$} 이다.
- example
    
    ![IMG_59C436DC3AA0-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_59C436DC3AA0-1.jpeg)
    
     
    
    ![IMG_A184D5BEB421-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_A184D5BEB421-1.jpeg)
    
- Follow set(`Follow()`)
    - 비교대상 바로 다음에 나올 수 있는 terminal 들의 집합을 구해주는 함수.
    - Follow(A)라고 하면 A라는 Symbol뒤에 나올 수 있는 terminal들을 말한다.
    - First() 와 Follow()의 관계 → 어떤 Symbol의 First는 어떤 Symbol의 Follow가 될 수 있다.
    - 규칙
        1. Non-terminal A의 follow set은 
        2. Start Symbol의 Follow에는 무조건!! $ 를 포함해줘야한다.
        3. 구하고자 하는 non-terminal이 속한 production을 찾는다. → 찾으면 바로 뒤에 뭐든 있으면 First(뭐든) - { $\epsilon$ } 을 follow에 넣어준다.
        4. 구하고자 하는 non-terminal이 속한 production을 찾는다 → 근데 우리가 구하고자 하는 non-terminal이 그 production의 맨 끝에 있다면!!! non-terminal을 derivation한 앞의 놈의 모든 follow를 우리가 구하고자 하는 non-terminal에 넣어준다. 
        
        일단 씨발 무슨 소린지 모르겠다. 하지만 존나 중요하다. 원래 이해가 안되면 뭐다? 예시를 통해 조져보는거야!!!!!!
        
    
    Ex) 
    
    ![IMG_59C436DC3AA0-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_59C436DC3AA0-1.jpeg)
    
    ![IMG_53D3F7ED3439-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_53D3F7ED3439-1.jpeg)
    
    ![IMG_E190D451F214-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_E190D451F214-1.jpeg)
    
    자 설명 해볼게 Follow(E)를 구해보자고.
    1. 먼저 E는 start symbol이니까 2번 룰에 따라 $를 넣어줘.
    2. 다음 E가 속한 production들을 쭉 보다가 저 맨뒤에 F → (E)를 찾았엉 E가 맨뒤가 아니지? 그러니까 E뒤에 있는 )의 First를 추가! → First( ( )
    3. 다음 또 찾아봐 오 저기 E’에 E가 있구만 근데 맨뒤지? 그러면 먼저 Follow(E’)을 추가시켜! 근데 Follow(E’)은 어케구하는거냐 씨발 일단 넘어가... 보니까 아마 Follow(E’)은 Follow(E)와 같아서 그냥 저렇게 쓴듯.
    4. Follow(E’)을 Follow(E)가 포함하니까 Follow(E’)도 찾아야겠지? E’어딨나 봤더니 E에 있다 그리고 E’은 맨뒤에 있어서 Follow(E)를 추가! → 근데 이건 있으나 없으나... 무시하자
    5. 그러면 {$,)}가 나온당~
    
    자 다음 Follow(T) 해보자
    1. 먼저 찾아! E에 보이네 근데 T는 뒤에 E’이 있으니까 First(E’) - {$\epsilon$}을 추가해준당
    2. 그리고 그 뒤에 있는 E’을 봤더니 아니 왠걸 E’은 $\epsilon$을 포함해서 T가 맨뒤가 될수 있네? → Follow(E)추가
    3. 다음 T찾아보면 T’에서 찾을 수 있음. 맨뒤니까 Follow(T’)추가.
    4. Follow(T’)을 포함하니까 저것도 찾아줘야겠찌? Follow(T’)은 T에 속하고 맨뒤에 있으니까 
    Follow(T’) = Follow(T) 고로 Follow(T)를 추가 해주지만 응 똑같아~ 그래서 Follow(T), Follow(T’) 무시때려도됨.
    5. 결과 First(E’) , Follow(E) → {+, $, ) }
    

### LL(1) Parsing Table 만들기. 진짜로 뭔소리지..?

1. 먼저 모든 production에 대한 first를 찾아서 하나씩 대입해주기.

![IMG_59C436DC3AA0-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_59C436DC3AA0-1.jpeg)

![IMG_663D33C7FA40-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_663D33C7FA40-1.jpeg)

![IMG_8D578E75260B-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_8D578E75260B-1.jpeg)

1. $\epsilon$채우기. (Follow를 통해서한다.)
    - production중 $\epsilon$ 을 포함하는 nonterminal이 있다면 Follow(nonterminal)에 $\epsilon$을 다 넣어준다.
    
    ![IMG_59C436DC3AA0-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_59C436DC3AA0-1.jpeg)
    
    ![IMG_DBEAFDBD0E75-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_DBEAFDBD0E75-1.jpeg)
    
    - E’에  $\epsilon$ 있고 T’에 $\epsilon$ 있으니까 이놈들의 Follow를 구해준다. 그러면 terminal 목록이 나오는데..
    E’ 에 해당하는 터미널 칸에 $\epsilon$을 다 넣어준다.
    
     
    
    ![IMG_6907DA377AFB-1.jpeg](Syntax%20Analyzer%20part%202%20c4058557381a42e081b32aff485a002e/IMG_6907DA377AFB-1.jpeg)
    
    그러면 만들기 끝!!!
    

### LL(1) Parser Implementation해보기! → 그림은 그냥 피피티봐!!!!

1. Stack에 Start Symbol, $(endmarker) 넣어주기
2. First component를 pop해준다! 
3. 만약 pop한 놈이 non-terminal이면 input pointer를 보고 가르키는 input과 빼낸 nonterminal을 비교해서 Parsing Table에서 찾아서 Push 시켜준다.(넣을 때 뒤에서부터 넣어준다 b/c leftmost니까!)
4.  만약 pop한 놈이 terminal이면 input pointer가 가리키고 있는 current input symbol과 비교해준다.
같다면!!! input pointer를 뒤로 옮겨준당!
5. 이렇게 3 4 를 계속 반복하다가 마지막 $ pop 했는데 input pointer까지 맞으면 stack에 아무것도 없고 input pointer도 끝났따!!! 그러면 Accept해준다