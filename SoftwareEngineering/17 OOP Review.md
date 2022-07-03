# 17. OOP Review

### ADT (Abstract Data Type)

: data와 그에 연관된 operation을 캡슐화해서 단일 문법적 요소 (single syntatic unit)으로 표현한 것

ex) 스택, 큐 같은 자료구조에서 세부 구현은 숨기고 함수 정의만 제공하는 것

- Single syntatic unit
    
    : 조직화, 수정, 분할 컴파일이 가능해짐
    
- Encapsulation
    
    : 내부적인 구현이 바뀌더라도 외부에 영향을 주지 않는다 → 신뢰성 증가
    
    - reliability 증가
    - 밖에서 볼 수 없음
    - 내부적인 것이 바뀌더라도 다른 코드들이 바뀌지 않음

### Object-Oriented Paradigm

- `Class` = `ADT` + `Inheritance`(for reusability) + `Polymorphism`(for flexability)
- 공통된 부분을 만들어 두고 그 클래스를 상속을 받으면 같은 성격들을 한 번에 관리할 수 있다

### OOP 예시

![스크린샷 2022-05-14 10.43.38.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_10.43.38.png)

여러 클래스에 공통되는 메소드 draw()를 재사용하고자 한다.

Shape를 Line~Polygon이 상속받아서 사용하면 된다!

- 나쁜 예시
    
    ![스크린샷 2022-05-14 10.46.19.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_10.46.19.png)
    
    타입을 직접 구분해서 프로그래밍할 경우 client의 새로운 클래스가 추가될 때마다
    
    코드가 변경되어야 한다. → risky
    

- 좋은 예시 - 상속
    
    ![스크린샷 2022-05-14 10.48.19.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_10.48.19.png)
    
    상속을 이용해서 하위 클래스에 대해 draw() 메소드를 실행하면 된다.
    
    client 프로그램의 재사용성이 증가된다.
    

### Inheritance (상속)

- X가 부모, Y가 자식 클래스
    - Y가 X의 모든 `method`를 상속 받아옴
        - X클래스가 응답하던 메시지를 Y클래스 인스턴스한테도 보낼 수 있다
    - Y가 X의 모든 `data` 를 상속 받아옴
        - X에서 가지고 있던 데이터가 전부 Y한테도 있다
    - 이제 우리는 Y는 X다 라고 할 수 있다.
        - 역은 안돼요. Y가 뭔가를 추가했을 수도 있기 때문에
    - Y 타입의 모든 인스턴스는 X 타입이라고 할 수 있다.
    - **X타입의 인스턴스가 기대되는 모든 곳에 Y 타입의 인스턴스를 쓸 수 있다**
    - 왜냐면 Y는 X가 가진 모든걸 다 갖고 있기 때문에!

![스크린샷 2022-05-14 10.49.46.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_10.49.46.png)

### Polymorphism (다형성)

하나의 operation이 상황에 따라 맞는 다양한 방법으로 수행되는 것

if-else, switch 같은 분기처리를 줄여준다.

- 객체 지향 software의 특성
    - 같은 이름을 가진 operation이 각각의 다른 클래스에 따라서 서로 다르게 반응할 수 있다.
    - 많은 if-else나 switch를 줄여줄 수 있다
    
- **Dynamic polymorphism** (**Runtime**)
    
    : 메소드 오버라이딩
    
    자식 클래스에서 부모 클래스의 메소드를 재정의 → 런타임 시간에 변수의 클래스에 바인딩된다.
    
    polymorphic variable은 객체의 클래스 또는 자식의 클래스를 참조할 수 있는 위치에 있어야 함
    
- **Static polymorphism** (**Compile time**)
    
    : 메소드 오버로딩
    
    파라미터 타입이 다른 같은 메소드
    

## Method Overloading

- 보기
    
    ![Screen Shot 2022-05-10 at 4.59.29 PM.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/Screen_Shot_2022-05-10_at_4.59.29_PM.png)
    
    - 파라미터들이 조금씩 다름
    - 하지만  `methodA` 가 `overloading` 되어 있어서 compile할 때 진행이 되는 애라서 compile time polymophism이라고도 한다

## Method Overriding

- 보기
    
    ![Screen Shot 2022-05-10 at 5.01.39 PM.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/Screen_Shot_2022-05-10_at_5.01.39_PM.png)
    
    - 양쪽에 같은 이름의 메소드가 있는데 자식 클래스인 Y가 `overriding` 조져버림
    - `obj2` 는 Y 타입의 인스턴스를 만들어서 X타입을 가리키게 해버림
        - 그럼 이 친구는 어떤 `methodA`를 부를까?
        - 당연히 Y의 것. 왜냐면 인스턴스가 Y의 것이기 때문에
    - 이것들은 runtime에서 진행됨.
        - compile time에서 이거 못해줌!

### (Run-time) Polymorphism → 가장중요한 페이지가 될것같음.

- 객체 지향에서의 variable(polymorphic variable)들은 타입을 가지고 있다.
    - 특정한 클래스의 타입으로 지정할 건데,
    - 본인의 타입으로 선언을 할 수 있고,
    - **자기 자식 클래스들의 객체들 또한 가리킬 수 있다**
        - 위의 예시에서 `x obj2 = new Y();` 이 부분을 말하는 것
            - x는 레퍼런스를 칭하는 아이. 본체는 인스턴스인 Y()
- 인스턴스가 무엇인지가 중요하다

### Abstract Class and Abstract Method

- abstract method
    - definition이 없고 protocol만 가지고 있는 method
- abstract class
    - `abstract` 라는 키워드를 정의할 때 붙여주면 해당 클래스는 abstract class가 됨
    - 인스턴스를 못 만들어낸다
    - **abstract class 또한 자식 인스턴스를 가리킬 수 있다! (******)**
        - `x obj2 = new Y();` 여기서 x가 abstract여도 가능
- 둘 사이의 관계
    - class가 abstract method 하나 이상 포함하면 무조건 abstract class
    - 반대는 성립되지 않는다
    
    : 정의는 포함하지 않고 프로토콜만 가진다.
    

- 예제
    
    ![스크린샷 2022-05-14 11.10.51.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.10.51.png)
    
    ![스크린샷 2022-05-14 11.11.04.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.11.04.png)
    
- Animal은 Abstract Class라서 인스턴스를 못만들어낸다!~!
- Cat은 Concrete라서 인스턴스 만들어낼 수 잇음.
- Cat은 Animal을 못가리키는게 Cat은 Animal에 Child라서 어차피 안됨 Abstract건 아니건.
- Sibling관계는 참조 못한다!! 마지막에서 세번째줄

- 런타임에서 메소드 결정 순서
    1. 현재 클래스에서 concrete method가 있는지 확인
    2. 상위 클래스에 해당 메소드가 있는지 확인
    3. 2단계를 반복
    4. 만약 메소드가 없다면 오류
        
        하지만, 이 경우 컴파일 단계에서 오류가 발생하기 때문에 일어나지 않을 것
        
- Z가 Y를 상속하고, Y가 X를 상속한다고 했을 때
    
    `X a = new Z();` 에서
    
    1. Z 인스턴스에서 m이라는 메소드가 concrete하게 있다면 실행. 없다면?
    2. Y에서 m이라는 메소드가 concrete하게 정의를 했다면 실행. 없다면?
    3. 큰일나는거야~

### Interface

- 모든 메소드가 abstract여야 한다., 하나라도 Concrete한 method가 있어선 안된다.
    - 모든 메소드가 body가 없어야함
- instance variable을 가질 수 없다.
- `public static final` 변수만 가질 수 있다.
    
    (상수만 가능)
    
- `interface`  키워드를 붙여서 선언
    - 인터페이스가 public이면 나중에 같은 이름으로 안을 채워서 만들어줘야함
- abstract class보다 더 abstract하다.
- interface를 구현하는 애들은 `implements` 를 붙여서 알려준다

![Screen Shot 2022-05-10 at 5.51.47 PM.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/Screen_Shot_2022-05-10_at_5.51.47_PM.png)

### Interfaces as Types (immmmmmmportttttantttt!!!!)

- abstract를 type의 이름으로 썻듯이, interface도 type의 이름으로 쓸 수 있다.
- method나, variable의 타입으로 사용할 수 있어요.
- 많관부. 많쓰부(많이 쓰기를 부탁한다는 뜻)

### Abstract Classes vs. Interfaces

대부분의 경우 인터페이스를 사용

- abstract를 사용하는 경우[특수한경우]
    1. 상위 클래스-하위 클래스가 “**`is a`**” 관계일 때
        
        단순히 상위 클래스의 메소드를 접근하기 위해서는 X → 이런 경우 인터페이스 사용
        
    2. implementation을 제공해야 하는 경우
        
        인터페이스는 어떤 구현도 불가능
        

- abstract 대신 interface를 사용해야하는 경우
    1. 클래스의 일부만을 표현하고 싶을 때
    2. 여러 개의 클래스를 상속하고자 할 때
        
        자바는 다중상속을 지원하지 않는다. 대신 인터페이스의 implementation은 제한이 없다.
        
    3. 메소드의 부분적인 구현이 필요 없는 경우

- 인터페이스와 다형성(polymorphism) 예제
    
    ![스크린샷 2022-05-14 11.35.06.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.35.06.png)
    
    ![스크린샷 2022-05-14 11.34.29.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.34.29.png)
    

### Class Relationships

시스템 설계 과정에서 클래스 관계의 방향이 중요하다.

- 화살표는 source가 target을 알고 있는 것이당
    - 부모는 자식의 정보를 가지고 있지 않아요!
    - 그래서 source이 바껴도 target는 끄떡 없습니다~!
    - target가 바뀌면 source은 영향을 받아버려요
    - Target : 부모, Source: 자식

![스크린샷 2022-05-14 11.35.39.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.35.39.png)

- 분석 단계
    
    : 위와 같이 추론할 수 없다.
    
- 설계 단계
    
    : 설계 단계에서는 방향성이 필요
    
     → 정확하게 표기하지 않아도 관례적으로 이해한다.
    

### Class Change Propagations

![Screen Shot 2022-05-10 at 6.51.35 PM.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/Screen_Shot_2022-05-10_at_6.51.35_PM.png)

ClassA1 변화 → ClassC → ClassCC

ClassG 변화 → ClassF → ClassE → Class D, Class H

- 저 다이아몬드 : F가 whole, G는 part다. whole객체는 part에 대한 정보가 다 있지만, part는 whole의 내용(reference)을 다 몰라도 된다!

이런 식으로 정보를 알고 있는 쪽으로 클래스의 변화를 알게 된다.

### Information Hiding (정보 은닉)

: 내부 설계를 숨긴다

![스크린샷 2022-05-14 11.46.40.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.46.40.png)

- ITYpe은 보이는 부분이 변하지 않았기 때문에 내부의 것들만 바뀌고 밖에서(외부사람들이 봤을때 implementation의 변화는 알 수가 없다)는 알아차리지 못한다
- 외부에는 인터페이스만 제공하고, 세부 구현은 숨긴다.
    
    → 내부의 변화가 일어나도 외부에는 영향을 끼치지 않는다.
    
    ⇒ side effect를 줄인다
    

### OOP 퀴즈

![스크린샷 2022-05-14 11.48.03.png](17%20OOP%20Review%209dbf6e11bbb4445a82d64422fb4ec06d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_11.48.03.png)

(a) Type1은 abstract class라서 new 불가.

** new 가 나오면 abstract인지 interface인지 확인//만약 concrete한 class가 아니라면 new를 붙일 수 없다!

(b) IType : interface, Type2 : concrete → Type2 → IType (o)

(c) Sibling끼리는 가리킬 수 없다.

(d) OK

(e) 부모가 자식을 가리키고있음

(f) 부모 자식관계, concrete까지 완벽

(g) Sibling관계.