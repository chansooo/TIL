# Domain model

# Domain model이란?

- 내가 만드는 소프트웨어가 다룰 도메인의 중요한 컨셉들을 class diagram으로 시각화하는 것
- software object와 class들을 디자인 하기 위한 inspiration을 제공
- 객체지향 분석에서 가장 중요한 작업
- 주어진 domain model에서 우리가 알고 있어야하는 key concept, 그들의 관계를 class diagram 모양으로 만드는 것



- business domain에서  중요한 것들과 제품을 설명하는 데 중점
- conceptual model, domain object model, analysis object model이라고도 불림
- software object 포함 x
- 소프트웨어 아키텍쳐의 domain layer와 같지 않음
- class diagram의 세 요소에서 두 요소만 사용 (class 이름, class attribute)
    - domain object
    - attributes
    - associations

## Business object 그리기


- 실제 상황에서 보이는 것들만 시각화한다.
    - software 관련된 것들 만들어넣지마
    - 근데 애초에 product의 domain이 software와 연관 있으면 당연히 가능

## 왜 도메인 모델을 만들까?

- elaboration의 여러 iteration을 거칠 때 domain을 이해하는데 도움을 줌
- 그 경험으로 domain layer의 software class를 만드는데 큰 영감을 줌
    - 실제 도메인과 프로덕트가 멀어지는 것을 방지
    - lower representation gap
        - domain과 domain layer사이의 간극을 줄인다~
    - 나중에 만들어내게 될 domain layer의 class name을 여기서 가지고 오는게 많다!

### Lower Representation Gap

![Screen Shot 2022-05-10 at 12.32.26 AM.png](Domain%20model%204c802d9719ee4a2591f9501083e7f8f7/Screen_Shot_2022-05-10_at_12.32.26_AM.png)

- 현실의 실제 domain을 기반으로 UP Domain model을 만들며 그 이름을 그대로 쓰거나 해서 UP Design Model을 만들 때 큰 도움을 줌

## Domain Model을 만드는 순서

1. Conceptual class 찾기
2. UML class diagram을 통해 클래스의 형태로 그린다
3. 연결(association)해주고, attribute 추가해준다

Domain model도 한번에 되는게 아니라 iteration 반복하면서 완성되는 것이다!

### Conceptual class 어떻게 찾지?

1. 기존에 있는 모델들 재사용
    - 이미 출판되거나 잘 만들어진 것들을 가져다 쓰는 것이 가장 좋다
2. category list 이용
    
    ![Screen Shot 2022-05-10 at 12.38.59 AM.png](Domain%20model%204c802d9719ee4a2591f9501083e7f8f7/Screen_Shot_2022-05-10_at_12.38.59_AM.png)
    
    - 자주 등장하는 카테고리의 category list를 봐라
3. 주어진 use case의 text를 보고 우리가 추론
    - use-case의 명사들을 보고 추론해서 만들자

1→3으로 불가능하면 점점 내려가기

### Conceptual Class 그려보기

먼저 이름만 박스떼기에 나열한다

### Report Object

예시로 영수증은 과정에서 나온 것들을 그대로 담아두는 것이기 때문에 domain model에 넣지 않아야한다

vs

반품하기 위해서는 영수증으로 입증을 해야하기 때문에 영수증이 단순히 정보의 나열이 아닌 증거가 될 수 있기 때문에 넣어야한다

하지만 우리는 domain model에는 넣지 않는다!

### Conceptual Class에 대한 고민

- 어떠한 정보를 어떠한 클래스의 attribute로 적을 것인가, 연결해서 표현할 것인가
    

    
    → 실제에서 숫자나 텍스트로만 표현되는 것이 아니라면 conceptual class로 따로 빼는 것이 낫다
    

### Description class를 만들어야할 때

- Description class

    - 어떤 다른 클래스를 설명하는 정보를 담은 클래스
- 따로 빼는게 좋은데 왜?
    - item을 없앴을 때 그 item에 대한 정보는 남아있어야하므로
        - 인스턴스가 사라졌을 때 인스턴스에 대한 정보는 남아있어야한다
        

## Association

- 클래스들의 연결

    
    - 의미있는 relationship이어야함
    

### 언제 association을 연결해야할까

- 관계가 있고, 일정 시간 이상 보존된다면
    

    
- 모든 association이 software 파트에서 보면 없을 수도 있다.
    - domain model은 data model이 아니니까!
- 클래스 간의 association은 1개 이상이 될 수도 있다

## Attribute

- object의 데이터 값
- 형태
    
    `visibility name: type multiplicity = default { property-string}`
    
    예시
    
    `+pi : Real = 3.14 {readOnly}`

- 다른 attribute로부터 계산이 가능할 때는 `/ symbol` 을 붙여줌(산술 프로퍼티 느낌)

### UP Domain Model은 `보통은(대부분)` elaboration 단계에서 시작되고 끝남
