# Operation Contract

- system의 operation의 결과로서 변화가 일어나는데 request를 받고 난 이후에 어떻게 변화하는가를 보여줌
- Design model의 method를 실행하면 어떠한 결과가 나오는지를 잘 보기 위해서 사용
- Domain model에서는 high level system operation에서 어떻게 변하는가를 보기 위해서.(requirement 수준)
    - design에서 쓰이는 것과는 다름
- pre-condition과 post-condition을 한 눈에 보여줌


- operation
    - operation의 이름
    - 파라미터
- Cross Reference
    - 어느 use-case에 등장하는가
- Precondition
    - 우리가 알아야할 객채의 상태, 가정 등
    - 비어있어도 O
- Postcondition
    - 가장 중요한 부분
    - domain object의 object들의 상태가 어떻게 바뀌는가
        - 인스턴스 만들어지고, association 변화 등등
        

## System Operation이란?

- 시스템을 블랙박스 형태로 취급했을 때 그 시스템이 제공하는 public한 인터페이스
    - API라고 볼 수 있음
- SSD 만들어내면서 추출할 수 있음
- SSD에서 나타나는 system event들 중에서 이벤트들을 handle하기 위해 가지고 있는 것들

### System Interface

- 모든 use-case들에서 추출할 수 있는 system operation들의 전체 집합
    
    

### Postcondition

- domain model에서 나타난 객체들의 상태 변화를 보여줌
    - instance 만들거나 지우는 것
    - association 생기거나 끊어지거나
    - attribute 변화
- 주 목적
    - system operation의 결과가 그대로는 이해가 잘 되지 않을 때
    - 더 자세히 알아야할 때
        - 충분히 이해가 간다면 따로 만들 필요는 없다
- 어떻게 쓸까?
    - 과거형의 시제로 써야함
- 예시
    - Instance 생성 or 삭제
        - 새로운 object가 생겨야함
            - `SalesLineItem` 예로 들면
            - `SalesLineItem` 의 인스턴스 `sli1`이 생성되었다
    - Attribute 수정
        - 만든 인스턴스의 attribute에 값을 넣어준다
            - `sli.quantity` became `quantity`
    - Association 생성 or 삭제
        - `SalesLineItem`과 연관된 `Sale` 그리고 `ProductDescription`
            - `sli` was associated with the current `Sale`
            - `sli` was associated with a `ProductDescription` , based on `itemID` match