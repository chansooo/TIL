# 27. Code Coverages for White Box Testing

White box test에서는 코드 구조를 알고 있기 때문에 내부적인 정보를 이용한 테스트가 얼마나 커버되었는지 확인할 수 있다.

- 참고자료
    
    [구조적 커버리지(Coverage)의 정의와 종류](https://blog.naver.com/PostView.nhn?blogId=suresofttech&logNo=221833396343&parentCategoryNo=&categoryNo=155&viewDate=&isShowPopularPosts=false&from=postView)
    
    ![Screen Shot 2022-06-12 at 11.17.21 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.17.21_AM.png)
    

### Code Coverage

- **Code Coverage**
    
    소스코드가 테스트에서 어느정도 커버 되었는가?
    
    여러 종류의 coverage가 정의된다 (아래에 기술된 것들)
    
- **Function coverage**
    
    각 function들이 수행되는가?
    
    → test case가 얼마나 성공했는지
    
- **Statement coverage (= node coverage)**
    
    각 라인이 수행되었는가?
    
    각 라인이 다 잘 수행되는가
    
- **Branch coverage (= decision coverage or edge coverage)**
    
    각 분기문이 true/false가 최소 한 번씩 실행되었는가?
    
    → 분기되어 나눠지는 것들이 edge
    
- **Condition coverage (= predicate converage)**
    
    각 분기는 여러 condition의 조합이 될 수 있는데
    
    이 condition의 true/false를 따지는 것
    
- **Condition/Decision coverage**
    
    branch와 condition은 서로를 완전히 포함하지 않는다
    
    → 둘을 같이
    
- **Modified Condition/Decision (MCDC) coverage**
    
    decision에 영향을 미치는 condition들에 대해서 독립적인 테스트 케이스를 만든다
    
    → 독립적인 condition을 하나만 변경했을 때 outcome에 영향을 주는 테스트 케이스들을 모은 것
    
    - 이러한 테스트 케이스들을 모두 모으면 MCDC를 100% 달성했다고 한다.
    - 항공기/자율주행 등 높은 신뢰성을 요구하는 분야에서는 MCDC를 중요하게 여긴다.
- **Multiple Condition converage**
    
    decision을 일으키는 input들의 가능한 모든 조합을 수행했는가?
    

---

## Statement Coverage

![Screen Shot 2022-06-12 at 11.19.24 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.19.24_AM.png)

tc1에 대한 statement coverage : 3/4

tc2에 대한 statement coverage : 4/4

전체 statement coverage : 4/4 (tc를 단순히 합하는 것이 아니라, cover 안된 line이 있는지로 파악해야 한다)

### Statement Coverage exercise

![Screen Shot 2022-06-12 at 11.21.52 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.21.52_AM.png)

---

## Branch Coverage

![Screen Shot 2022-06-12 at 11.23.40 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.23.40_AM.png)

- 단순히 더하기 아님. 분기 중에 어떤 거 만족을 시켰는가 (합집합)

### Branch coverage exercise(Decision Coverage)

![Screen Shot 2022-06-12 at 11.24.40 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.24.40_AM.png)

![스크린샷 2022-06-14 02.09.54.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_02.09.54.png)

### Comparison : Statement coverage & Branch coverage

Branch는 Statement를 커버한다

- 만약 모든 분기에 대해 cover (branch coverage = 100%)
    
    → 포함관계에 의해 statement coverage = 100%가 된다.
    
    ![Screen Shot 2022-06-12 at 11.27.14 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.27.14_AM.png)
    

---

## Condition Coverage

branch와는 달리 각각 독립적인 condition들을 고려한다.

- 모든 condition에 대해 T/F를 모두 달성하면 coverage는 100%가 된다.

![Screen Shot 2022-06-12 at 11.27.48 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.27.48_AM.png)

- A>1 과 B==0 등 독립적인 condition들을 분리
- 모든 condition에 대해 T/F를 모두 cover하는 것을 확인할 수 있다.

### condition coverage exercise

![Screen Shot 2022-06-12 at 11.29.15 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.29.15_AM.png)

### Comparison : Condition coverage vs. Branch coverage

![Screen Shot 2022-06-12 at 11.30.49 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.30.49_AM.png)

![Screen Shot 2022-06-12 at 11.32.12 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.32.12_AM.png)

branch와 condition이 동일하지 않아서 둘 다 확인하는 coverage 등장

---

## Condition/Decision Coverage

- condition과 decision(= branch)을 둘다 고려한다.
- coverage는 둘을 합한다.

![Screen Shot 2022-06-12 at 11.33.13 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.33.13_AM.png)

- 분모는 분모끼리, 분자는 분자끼리 더해서 계산한다.

### Comparisoin : Condition/Decision coverage

![Screen Shot 2022-06-12 at 11.34.00 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.34.00_AM.png)

---

## Multiple-Condition Coverage (Compound-Condition Coverage)

- 각 condition들의 모든 조합을 고려한다.
- 가장 복잡하다
- multiple condition coverage를 (100%) 달성하기 위해서는 tc가 가장 많아진다.
    
    ⇒ coverage를 달성하면 포함하는 모든 coverage를 모두 달성한다 (condition/decision → branch, condition → statement)
    

![스크린샷 2022-06-14 02.26.03.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_02.26.03.png)

![Screen Shot 2022-06-12 at 11.36.25 AM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_11.36.25_AM.png)

![스크린샷 2022-06-19 20.10.22.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-19_20.10.22.png)

---

## Modified Condition Desision Coverage (MCDC)

- multiple condition coverage는 n개의 condition에 대해서 2^n 개의 tc가 필요하다
    
    ⇒ 현실적으로 힘들다
    
- 따라서 현실적인 절충안을 제시하는 것이 MCDC이다.
- 높은 신뢰성을 요구하는 프로그램(안전, 생명 등에 직결)에서는 MCDC가 자주 활용된다.
- 각각의 Condition들이 독립적으로 Decision에 영향을 미친다.
- n+1 ≤ x ≤ 2*n 개의 Test Case만 필요.

- 참고
    
    ![Screen Shot 2022-06-12 at 12.29.40 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.29.40_PM.png)
    

### 예시) N = 2

![Screen Shot 2022-06-12 at 12.29.04 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.29.04_PM.png)

- 2^2개를 원래 테스트를 해야한다
- 근데 결과를 중심으로 줄여보자
    - 왼쪽 표
        - (파란박스)B 값에 따라서 결과값이 바꼈다.
        - (빨간박스)A 값에 따라서 결과값이 바꼈다.
        
        → A=F, B=F의 tc는 만들지 않아도 된다.
        
    - 오른쪽도 동일한 방식으로 이루어진다.

⇒ MCDC는 n+1~2n 범위의 tc를 요구한다. (n = conditions)

### 예시) N = 3

multiple condition에서는 2^3 = 8개의 tc를 모두 요구한다.

MCDC에서는 4(lower) ~ 6(upper) 범위에서 tc를 요구하게 된다.

![Screen Shot 2022-06-12 at 12.37.14 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.37.14_PM.png)

- 결과값에 영향을 주는 pair를 짝지어서 모으고(박스)
- 변인들 중에서 해당하는 거 제외하고는 동일한 값을 가지고 해당하는 하나에 따라서 결과가 바뀌는 것

- A && B && C :  TCs = {1, 2, 3, 5}
    - (A, B는 고정일 때) C의 변화는 row 1, 2에 영향을 준다.
    - (A, C는 고정일 떄) B의 변화는 row 1, 3에 영향을 준다.
    - (B, C는 고정일 때) A의 변화는 row 1, 5에 영향을 준다.
- A || B || C : TCs = {4, 6, 7, 8}
    - C만의 변화 → row 7, 8에 영향
    - B만의 변화 → row 6, 8에 영향
    - A만의 변화 → row 4, 8에 영향

![Screen Shot 2022-06-12 at 12.39.52 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.39.52_PM.png)

- 얘는 5,6,7은 공통이지만 A에 대한 테스트케이스를 뭘로 할지에 따라 달라질 수 있다

### Comparison

![Screen Shot 2022-06-12 at 12.40.47 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.40.47_PM.png)

- MCC: 2^n
- MCDC: 현실적으로 많이 쓰이는 친구
    - n+1~2n

---

### Summary

![Screen Shot 2022-06-12 at 12.42.16 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.42.16_PM.png)

![Screen Shot 2022-06-12 at 12.56.07 PM.png](27%20Code%20Coverages%20for%20White%20Box%20Testing%205c09868e5604445599e229b1cb8b8569/Screen_Shot_2022-06-12_at_12.56.07_PM.png)