# 7. Use Cases

## Use Case

---

Use Case는 UP와 밀접한 관계를 가지며, FR을 기술하는데 활용되는 유용한 도구이다.

- UP Artifacts Relationships (UP의 산출물들의 관계)
    
    ![스크린샷 2022-04-05 오후 4.33.41.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.33.41.png)
    
    - Use Case Model
        - Use Case Diagram + Use case Text
        - Requirement
            - Vision
            - Glossary
            - Supplementary Specification (NFR이 담겨있음)
        - 이로부터 actor와 system 간의 주요 인터랙션을 통해서 API를 추출할 수 있고, 그것의 Operation Contracts를 만들 수 있다.
    - Requirement 관련된 것들은 Design 단계에서 사용된다.
    

### Use Case

우리가 만드려고 하는 **system**에서 **actor**가 **goal**을 성취하기 위한 **test story**들

- requirement를 알아내거나, 명세하는데 유용
- ex) Process Sale

### Actor, Scenario and Use Case

- Actor : 개발하는 시스템과 interact하는 유저나 다른 시스템. 이들의 **role을 명세**
- Scenario (= use case instance) : 일련의 action이나 상호작용
- Use case : 성공/실패 시나리오들 모인 것

### Use Case Model

![스크린샷 2022-04-05 오후 5.16.17.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.16.17.png)

- UP는 requirement discipline에 Use Case Model을 정의
    - 명세된 use case들
    - 시스템의 기능과 환경을 모델링
- use case diagram을 선택적으로 포함
    - use case와 actor의 이름
    - 시스템의 바운더리 (내외부 구분), 환경

### Use case 사용 동기

- user의 목표를 알아낼 수 있는 방법
    - domain expert or requirement 제공자가 직접 요구사항을 쓸 수 있다.
- user의 목표를 강조
- Use case는 요구사항 (특히 functional or behavioral에 중점을 둔다.

### Actor의 종류 (3가지)

- Primary Actor : SuD 에서 goal을 성취하고자 하는 user
    - 일반적으로 인터랙션을 시작하는 역할 (모두 그런 것은 아님)
    - eg) cashier
- Supporting Actor : SuD에 서비스를 제공하는 외부의 시스템(또는 사람)
    - eg) payment authorization service
- Offstage Actor : use case와 관련 있는 actor
    - 직접적으로 인터랙션을 하지는 않지만, 관련 있고 참고할 것들
    - eg) government tax agency

### Use case 포맷 (3가지)

- Brief
    - 간단하게 하나의 paragraph로 표현.
    - 하나의 성공 시나리오 ( = happy path)로 구성
- Casual
    - 여러 paragraph로 구성
    - cover 시나리오를 포함한다.
- **Fully Dressed** : 모든 step, variation, supporting section들을 포함
    
    ![스크린샷 2022-04-05 오후 5.29.48.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.29.48.png)
    
    - **Use Case Name** : 동사로 시작
    - **Scope** : 기술(또는 디자인)하는 시스템의 대상
        - System use case : 일반적으로 소프트웨어 시스템을 개발할 경우
        - Business use case : 기업 수준의 업무 프로세스를 명세
    - **Level**
        - User-Goal level (default) : primary actor의 goal을 성취하기 위한 시나리오를 기술할 때 사용
        - Subfunction level : use case에 자주 등장하는 substep들 → 재사용
    - **Primary Actor**
    - **Stakeholders and Interests** : 이해관계자들과 그들의 관심사
    - **Precondition** : 어떤 것들이 전제되어 있는지?
    - **Success Guarantee** : 시나리오가 성공적으로 수행된 이후 어떤 것들이 보장되는지 (=Postcondition)
    - **Main Success Scenario**
        - = Basic Flow, happy path
        - 각각의 스탭에 대해 자세한 설명과 인터랙션들을 명세해야 함
        - 브랜치되는 상태들은 기술하지 않는다 (이들은 대체 흐름에서 다룸)
        - 스탭의 종류
            1. actor간의 상호작용
            2. 시스템 내에서의 검증
            3. 시스템 내부의 상태변화
        - 첫 스탭은 위에 해당안될 수 있음 (시나리오가 시작되는 트리거가 일반적)
    - **Extensions** ( = Alternate Flows)
        - 메인 시나리오 + 대체 흐름 → 이해관계자의 관심사를 거의 만족해야함
            - NFR은 supplementary specification에 포함되므로
        - *a : 언제든지 해당 플로우로 들어올 수 있음
            - eg) 언제든지 Manager 모드로 접속할 수 있음
        - 1a : 메인 성공 시나리오 스탭 1 진행 후에 진행될 수 있는 대체 플로우들 (a, b ...)
    - **Special Requirement** : NFR들을 기술
    - **Technology and Data Variations List** : 기술이나 데이터에 관련된 변경 사항들
    - **Frequency of Occurrence** : 위의 것들이 발생하는 빈도
    - **Miscellaneous** : 아직 해결하지 않은 오픈 이슈들
    

## Use Case 가이드라인

---

### Use Case 작성 시 가이드라인

- 사용자의 의도, 시스템의 책임 면에서 작성할 것. concrete한 행동은 x
    - GUI, 입력하는 방식에 영향을 받는 구체적인 행동들은 요구사항 분석 때는 x
    - eg) dialog에서 ID와 PW 입력
- 간결하고, 요점에 맞게 작성
- Black-Box 스타일로 작성
    - 내부의 작동 방식 x
    - eg) SQL INSERT  x
- Actor와 Goal을 연결해서 작성
    - 요구사항을 정확하게 분석하기 위해서 고객의 goal을 중심으로 알아낼 것

### Use Case를 찾는 방법에 대한 가이드라인

- primary actor의 goal을 성취하는 것이 목표
- 아래의 순서로 식별한다
    1. 시스템의 범위 (boundary)
    2. primary actor를 식별
    3. primary actor마다 goal을 식별
    4. 위의 과정을 토대로 user goal을 만족하는 use case가 만든다.
- 두 가지 접근 방식
    1. 발견해서 바로 다이어그램을 그리고, goal의 이름을 붙임
    2. actor-goal의 목록을 먼저 만들고, 각각에 대해 분석하고 다이어그램을 만듬
- 고객에게는 goal을 중심으로 질문한다.
    - 유저가 어떤 일을 수행하는가요? → x
    - 측정 가능한 결과가 있는 goal이 무엇인가요? → o
- primary actor는 누구인가?
    
    ![스크린샷 2022-04-05 오후 6.40.52.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.40.52.png)
    
    - 직접 인터랙션을 일으키는 사람
        - eg) POS 시스템에서는 캐셔이다.
    - 시스템의 바운더리에 따라서 달라진다
        - eg) 범위가 POS → Checkout 서비스로 넓어지면 Sales activity system으로 바뀜

### 유용한 Use Case를 찾기 위한 방법들

- use case 레벨
    - 너무 디테일해도 오버헤드가 많아서 힘들고, 너무 hight level이어도 분석된 것이 별로 없어서 힘들다
    - 적절한 수준의 use case를 만들어야 함
- **The Boss Test**
    - boss의 관점에서 봤을 때 만족할만한가?
    - eg) Logging → boss not happy
- **The EBP Test**
    - Elementary Business Process (EBP)이어야 함
        - 한 사람이 한 장소에서 수행할 수 있는 작업
        - 비즈니스 이벤트에 대응
        - 비즈니스 가치로 측정할 수 있는 작업
        - 여러 개의 스탭으로 구성
- **The Size Test**
    - 단일 스탭으로 구성된 use case는 x
    - 여러 스탭으로 구성되야 함
- 테스트 적용 예제
    - 공급 계약 협상 : EBP보다 큼 (한 사람이 한 장소에서 하기에는..)
    - 반품 핸들링 : boss, EBP, size 모두 통과 → good!
    - 로그인 : boss x
    - 보드 게임에서 말 움직이기 : size x
- 예외적인 상황
    - test에 실패하더라도 subfunction으로 분리하는게 효율적인 경우들이 존재
    - eg) 신용카드로 결제
        - EBP, size 실패
        - 여러 다른 use case에서 반복되기 때문에 분리하는 것이 효율적
    - eg) 유저 인증
        - boss test 실패
        - 복잡한 내부가 있을 경우 분리해서 use case로 할만한 크기가 된다.
    

## Use Case Diagram

![스크린샷 2022-04-05 오후 7.38.42.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.38.42.png)

- use case와 같이 쓰이지만, 주가 되는 것은 text 형태의 use case
- 한 눈에 파악할 수 있는 시각적인 context 제공
- 따라서 짧고 간단하게 그리는 것이 좋음

## UP에서 Use case model의 역할

![스크린샷 2022-04-05 오후 7.39.54.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.39.54.png)

- Incep (1주)
    - 2일짜리 워크샵 진행 → use case의 이름 & 하나의 parapgraph 정도
    - 10%정도의 중요한 요구사항 선별 (아키텍처인 중요도, 리스크, 비즈니스 가치 고려)
- Elab 1 (3주)
    - 인셉션 단계에서 분석된 요구사항을 디자인 및 구현
    - 후반부에서는 남은 요구사항들에 대해서 상세하게 분석하고, use case로 만듬
- Elab 2 (3주)
    - 앞서 elab과 같이 반복 → 유즈 케이스 50% & 구현 5%
- Elab 4 (3주)
    - 반복 → 유즈 케이스 80~90% & 구현 15%
    - 남은 요구사항들은 쉬움 (덜 중요, 리스크 낮음, 비즈니스적 낮음) → 프로젝트 성공확률 높다

### UP에서 산출물이 나오는 시기

![스크린샷 2022-04-05 오후 7.48.13.png](7%20Use%20Cases%20099c9eabb5184a0db4a7e98536f77cfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.48.13.png)