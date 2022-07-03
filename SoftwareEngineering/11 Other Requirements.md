# 11. Other Requirements

![스크린샷 2022-05-13 01.29.22.png](11%20Other%20Requirements%2083c0ddb46ce4429b8bd9f834b7c4dabf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-13_01.29.22.png)

### Other Requirements Artifacts

- Supplementary Specification
    
    : NFR(non-functional requirement)들
    
    - URPS+ (usability, reliability, performance, supportability, +)
    - constraints, standards, licensing, ...
- Glossary
    
    : 용어나 정의들 (data dictionary)
    
- Vision
    
    : 프로젝트 전체 requirement의 간결한 요약 (summary)
    
- Business Rules
    
    : 비즈니스 룰, 정책, 관련 법률 등
    

### Supplementary Specification(NFR, Constraint를 작성한다)

: 주로 NFR을 포착하고, constraint(언어, 법률 등) 등이 작성된다.

- 구성요소
    - Common functionality
        
        : 특정 use case에만 나타나는 것이 아닌, 여러 use case에서의 공통된 기능을 기술
        
        > 반대로 특정한 use case에서만 나타나는 NFR은 use case에서 기술한다.
        > 
    - usability, reliability, perfeormance, supportability, reports(이런것들 이제 시스템 전반에 있어서 통용된다)
    - hardware or software constraints
    - development constraint
        
        ...
        
- 이러한 NFR은 Inception 단계에서 모두 분석해야 하는 것은 아니다.
    
    UP는 점진적이고, 반복적으로 진행되기 때문에, Inception이 아니라 Elaboration 단계에서 진행해도 괜찮다.
    
- 일반적인 research에서 알 수 있는 것
    
    “top ten” requirement가 유용하다
    
    top ten requirement → 시스템, 아키텍처적으로 중요하거나, 리스크가 크거나, 비즈니스적 가치가 큰 requirement가 있을 가능성이 높다. (러프하게라도 있다면 유용함)
    
    ⇒ 따라서 NFR을 일찍 분석하는 것은 아키텍처 등에 영향을 미치는 중요한 requirement를 미리 파악할 수 있으므로 유용하다.
    

### Supplementary Specification 예시

![스크린샷 2022-05-13 16.28.22.png](11%20Other%20Requirements%2083c0ddb46ce4429b8bd9f834b7c4dabf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-13_16.28.22.png)

![스크린샷 2022-05-13 16.29.10.png](11%20Other%20Requirements%2083c0ddb46ce4429b8bd9f834b7c4dabf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-13_16.29.10.png)

보다 상세한 예시는 교과서를 참고할 것

### Requirement 분석이 시작되는 단계

![스크린샷 2022-05-13 16.30.21.png](11%20Other%20Requirements%2083c0ddb46ce4429b8bd9f834b7c4dabf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-13_16.30.21.png)