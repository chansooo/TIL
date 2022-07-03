# 25. Relating Use Cases

### Use Cases and the Use-Case Model

- Use-Case Model
    - (주로 텍스트 형태의) use case들의 집합
    - 시스템의 기능들과 관련된 시스템과 상호작용하는 환경에 대한 모델
- Use Case Diagram
    - 주요 정보는 텍스트 형태로
    - 선택적으로 다이어그램을 통해 전체적인 이해를 도울 수 있다.

### <<include>> Relationship

- 여러 use case에서 동일하게 사용되는 공통된 것을 subfunction 형태로 만든다
    
    → 재사용이 핵심
    
- Subfunction형태로 만들면서 이제 새로운 Use Case로 만들어준다.

![스크린샷 2022-06-14 01.01.34.png](25%20Relating%20Use%20Cases%20fbca14a9f1824a2bbc48c1de9fbbb831/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_01.01.34.png)

- 방향은 Base → Sub Use-Case 방향이다

### <<extend>> Relationship

- **변경하지 않고 싶은 use case**가 있을 때
- 해당 use case를 고치지 않고 시나리오를 확장하고 싶을 때

![스크린샷 2022-06-14 01.06.22.png](25%20Relating%20Use%20Cases%20fbca14a9f1824a2bbc48c1de9fbbb831/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_01.06.22.png)

base use-case에서는 Point만 명시한다.

→ base에서는 아무런 정보가 없다.

⇒ <<include>>의 방향과 반대이다.

![스크린샷 2022-06-14 01.07.57.png](25%20Relating%20Use%20Cases%20fbca14a9f1824a2bbc48c1de9fbbb831/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_01.07.57.png)

extension point만 명시한 모습

> 동일한 상황에 대해서 <<include>>를 사용할 수도, <<extend>>를 사용할 수도 있는데 적합한 것을 쓸 수 있다. 실무에서는 그냥 extension에서 분기처리를 통해 표현하는 것을 선호할 수 있다. (use case를 더 만드는 cost가 발생하기 때문에
> 

⇒ 무조건 이것저것 많이 쓴다고 좋은 것이 아니라 다양한 것을 고려하고 과하게 복잡한 산출물을 만들 필요는 없다.