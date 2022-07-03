# Iteration1

## 모든 requirement를 한번에 진행하지 않는다

- iteration1은 완성된 usecase와 requirement의 일부분이다.
- 하나의 usecase와 requirement를 전부 다 하는게 아님!
    
    ![Screen Shot 2022-05-09 at 10.57.41 PM.png](Iteration1%20b2fc050cc0234e498d845449b0a93f06/Screen_Shot_2022-05-09_at_10.57.41_PM.png)
    

## Inception은?

- 반복되지 않고 한번만 진행된다
- 중요한 것들을 정함
    - most actors, goals, usecases named

## Elabotation

첫번째 iteration에서 하는 일

- 중요하고 risky한 architecture 프로그래밍, 테스트
- 주요 요구사항 찾아내고 안정화
- core architecture를 만들고, hight risk element 풀어내고, requirement들 정의, 전반적인 schedule, resource 정의
- Do short timeboxed risk-driven iterations
- Start programming early
- Adaptively design, implement, and test the core and risky parts of the architecture
- Test early, often, realistically
- Adapt based on feedback from tests, users, developers
- Write most of the use cases and other requirements in detail, through a series of workshops, once per elaboration iteration

## Elaboration 단계에 시작되는 것들(UP에서)

- domain model: 분석 작업
- design model: 설계 작업
    - 어떻게 논리적으로 실현할 수 있을까~
    - software class diagrams,
    - object interaction diagrams
    - package diagrams
- software architecture
    - 중요한 architecture 이슈들 파악하고, 어떻게 해소할지
- data model
    - database schema
- usecase, ui prototype

## elaboration에서 쉽게 하는 오해

- 프로젝트에서 몇개월 이상 걸린다
    - 1주일에 끝나야함
- iteration이 하나만 있다
    - 많은 iteration에서 반복된다
- 대부분 요구사항은 elaboration 전에 정의되어있다
    - iteration 하면서 진행되는 것임
- 위험한 요소와 핵심 아키텍처를 다루지 않는다
    - 그걸 다루는 거임
- 실행 가능한 아키텍처를 만들지 않는다. 실제 코드 작성 안한다
    - 한다!\
- implementation 단계 이전에 요구사항, 설계 단계로 간주된다
    - 아니다~ 지속해서 하는거다~
- 프로그램이 하기 전에 완전하고 세심한 설계 해야한다
    - 지속해서~
- 최소한의 피드백과 adaption만 한다
    - 많이 해야한다.
- 초기에는 현실적인 테스트가 없다
    - 있다!
- 아키텍쳐는 프로그래밍 전에 추측으로 완성한다
    - 프로그래밍 중에도 바뀐다~
- production core executable architecture 프로그래밍 << proof-of-concept programming
    - 실전 코드 쓴다!
    

## Use Case Ranking

- 반복적 개발에서 use case 랭킹을 매겨서 진행하게 되는데 높은 순서부터 개발함.
- 랭킹 매길 때 고려 요소
    - risk
        - 기술적인 어려움, 노력과 사용성
    - coverage(영향도)
        - logging같은 건 전체적으로 필요하니까 빨리 해야겠지?
    - criticallity
        - 클라이언트가 중요하게 생각하거나 BM에서 중요하다
    
    ![Screen Shot 2022-05-09 at 11.18.18 PM.png](Iteration1%20b2fc050cc0234e498d845449b0a93f06/Screen_Shot_2022-05-09_at_11.18.18_PM.png)
    
- early iteration에서 랭킹이 높은 애들을 먼저 해야한다
    - 그렇게 해야 중요한 것들 빨리 할 수 있고, 많이 테스팅 할 수 있음