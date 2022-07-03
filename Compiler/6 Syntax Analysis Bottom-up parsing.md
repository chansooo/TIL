# 6. Syntax Analysis: Bottom-up parsing

## Bottom-up parsing

---

parse tree를 leave → root의 방향으로 생성한다.

right derivation을 모두 진행한 뒤에, 역방향으로 읽으면서 트리를 생성한다.

- 예제
    
    ![스크린샷 2022-05-09 13.47.19.png](6%20Syntax%20Analysis%20Bottom-up%20parsing%2001eadac1a3cc43e9ad671f4ae8643b25/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-09_13.47.19.png)
    

### LR parsing

- L: **L**eft-to-right scan of input
- R : **R**ightmost derivation

### Reverse Order : Reduction 방식이라고 부른다.

### Bottom-up parsing의 특징

- Left factoring과 Left-recursive 제거가 필요 없음 (unambiguous는 만족해야함)
    
    ⇒ top-down parsing보다 더 선호된다.
    

## 3-2

### Q : At each reduction, which substring should be reduced?

어떤 심볼이 reduce되어야 할까?

### Bottom-up Parsing with Backtracking Code.

![IMG_1BE66CAD7F2B-1.jpeg](6%20Syntax%20Analysis%20Bottom-up%20parsing%2001eadac1a3cc43e9ad671f4ae8643b25/IMG_1BE66CAD7F2B-1.jpeg)

 - SStr : $\alpha$의 substring 중 non-terminal로 바뀔 수 있는 놈.

### Backtracking Example 1. → INPUT : cad 일때.

S → dAc | cAe | cAd , A → a

![IMG_D3F5A57CD85F-1.jpeg](6%20Syntax%20Analysis%20Bottom-up%20parsing%2001eadac1a3cc43e9ad671f4ae8643b25/IMG_D3F5A57CD85F-1.jpeg)

 

### Bottom-up Parsing with Backtracking 의 장 단점 및 조건

- 조건
    1. CFG가 Non-Ambiguous는 만족해야함
    2. CFG가 Left Recursive는 만족 안해도됨
    3. CFG가 Left Factoring은 만족 안해도됨.
- 장점
    - 이해하기 쉬움
    - 실행하기(implement) 쉬움(손으로)
- 단점
    - 존나 Inefficient하다. 왜냐면!! Backtracking을 존나하니까!!

---

### Bottom-up Parsing #1 : Shift-Reduce

- Bottom-up Parsing의 특징
    - $\alpha \beta \omega$가 현재 bottom-up parsing을 진행중인 String이라고 하자.
    - 만약 다음 Reduction이 X → $\beta$ 이면, $\omega$는 terminal의 sequence(연속, 집합)이다.
- 이유
    - Bottom-up Parsing은 Rightmost derviation을 사용하기 때문에 항상 Rightmost non-terminal이 Replace된다.
    - 따라서 위의 특징 이 만족하는거임.