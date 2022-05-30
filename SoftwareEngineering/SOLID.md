# SOLID

## Hierarchy of Pattern Knowledge

- Design Pattern
    - OO principle을 지키려고 하다보니 생긴 아이
    - 23가지 패턴 중 하나는 전략패턴
        - 알고리즘의 군집을 정의
        - 알고리즘들을 encapsulate → interchangeable하게 만듬
        - client가 사용하고 있는 알고리즘들이 client와 독립적이게 되도록
- OO principle : design pattern의 근간이 되는 객체지향 설계 원칙 (ex: GRASP)
    - 변하는 것을 encapsulate 시켜라
    - inheritance보다 composition을 선호해라
    - implementation에 초점x, interface에 초점
- OO Basics
    - Abstraction
    - Encapsulation
    - Polymorphism
    - Inheritance

# Design Smells

설계 악취 = 이상한 디자인이라고 느낀 수 있는 신호, 증상

- 경직성(Rigidity)
    - 시스템을 변경하기 어려움
    - 다른 부분까지 변경해야함
- 취약성(Fragility)
    - 코드가 잘못되기 쉬움
    - 하나를 변경했는데 다른 애가 잘못되는 것
- 부동성(Immobility)
    - mobility의 반대
    - 움직이기 여러움. ⇒ 재사용하기 어려움
- 점착성(Viscosity)
    - 설계를 유지하면서 변경하는 것이 어렵다.
    - 꼼수를 자꾸 써야한다
- 불필요한 복잡성(Needless Complexity)
    - 과도한 설계, 설계가 불필요한 것을 포함할 때 생김
- 불필요한 반복(Needless Repetition)
    - 복붙을 많이 해서 하나가 잘못돼있으면 다 고쳐야하는 위험
- 불투명성(Opacity)
    - 이해하기 어려워 ㅠㅠ

# Dependency Management

- 설계 악취는 dependency 관리가 안될 때 많이 발생된다.
- dependency가 관리가 안되면
    - coupling 증가
    - spaghetti code
- OO language는 dependency를 잘 관리하라고 다양한 tool들을 제공
    - Interface: dependency의 방향을 바꾸거나 끊을 수 있도록
    - Polymorphism: dynamic하게 method를 부를 수 있음
    - 어떤 식으로 사용하는게 올바를까?
        
        → OO 설계 원칙
        

# SOLID

- Single-Responsibility Principle (SRP): 단일 책임 원칙
- Open-Closed Principle (OCP): 개방 폐쇄의 원칙
- Liskov Substitution Principle (LSP): 리스코프 치환 원칙
- Interface Segregation Principle (ISP): 인터페이스 분리 원칙
- Dependency Inversion Principle (DIP): 의존성 역전 원칙

## Single-Responsibility Principle

- 이게 좋아보이니?
![](https://velog.velcdn.com/images/kcs05008/post/dc165a03-c783-4053-8e88-0ab29b8bc49a/image.png)

    
   안좋아~
    
- 클래스는 단 하나의 변경될 이유가 있어야한다. 즉 하나의 responsibility만 가져야한다.

### Responsibility

- 클래스의 계약, 의무
- 클래스를 변경할 요인
    - 많은 responsibility를 가진다 → 많이 변경될 가능성
    - 많이 클래스가 변경되면 bug 많아짐
    - 다른 class에게 impact

### SRP: responsibility를 분리해서 class를 만들자

- Related measure - Cohesion
    - 연관성이 있는 애는 묶어~

주의: 너무 과도하게 다 지키려고 하지 말어라…

## 예시

### #1
![](https://velog.velcdn.com/images/kcs05008/post/09d63b2a-2cd8-434b-9ffe-7c3af763b34d/image.png)


- 학생 클래스를 정의하고 sorting할 수 있는 method를 어디에 둘까?
    - data가 있는 곳에 두자 (information expert)
- 학생 클래스는 business entity임.
- 단점
    - student class가 sorting하는 responsibility를 가지게 되면
    - 학생 클래스에 dependency가 있는 모든 client들이 같이 변경되어야한다
    - 그러면 테스트도 다 해야함
    - 이런 문제 왜?
        - responsibility를 두개 이상 가져와버렸기 때문에 → SRP 위반
- compareTo를 추가하면서 student 클래스가 변경되면서 client인 Register, AClient가 모두 변경되어야함
    
![](https://velog.velcdn.com/images/kcs05008/post/af17abf7-9a65-4684-8e59-4963dff70dde/image.png)

    
- 그러면 SRP 유지해보자
    ![](https://velog.velcdn.com/images/kcs05008/post/d1dde4cb-9c87-4321-b570-04187ccd2463/image.png)

    
    - compare하는 class를 새로 만들어버리자
    - student에게 dependency를 가지는 다른 애들을 안 바꿔도 된다!

### #2

![](https://velog.velcdn.com/images/kcs05008/post/80159ab1-3a3b-449b-9c21-443944a420e5/image.png)


- Rectange이라는 class를 두가지 class가 사용.
- 일단 지금도 responsibility가 중첩됨(area, draw)
- computational에서 area에 대한 변경을 요청. 그러면 Graphical은 억울하다… 자기는 바꿔달라고 한 것도 없는데 재컴파일 해줘야하거든…
- 고쳐보자!
    
    ![](https://velog.velcdn.com/images/kcs05008/post/c588bb96-aed7-467d-837e-6f79d01d02df/image.png)

    
    - 좋다!
    - rectangle을 responsibility에 따라 두개로 나눔
    - 하지만 Graphic Rectangle도 Geometric Rectangle의 length, width를 사용해야함 ㅠㅜ
        - GA의 요청에 의해 Graphic Rectangle의 draw를 바꿔야한다고 가정
        - 그러면 Graphic Rectangle을 보고 있는 GA도 바꿔야하지만 Geometric Rectangle은 안 바뀌어도 되네!
        - 하지만 Computational Gero~가 변경되면 오른쪽도 바꿔줘야함 ㅠㅠㅠ
- 그럼 또 바꾸자
    
    ![](https://velog.velcdn.com/images/kcs05008/post/1afa7652-4c71-46a0-9f6e-ae352f964827/image.png)

    
    - 이렇게 하면 왼쪽과 오른쪽이 분리되버렸당
    - 추상성을 사용해서 SRP를 잘~ 지킬 수 있당
    

## Identifying Responsibilities Can be Tricky

- 과거에 하나의 responsibility만 가지는 class를 만들었는데 다른 종류의 responsibility가 섞여있을 수도 있음.
    
    ![](https://velog.velcdn.com/images/kcs05008/post/ca3e38e0-bfc9-436f-8e4a-dfc0b0ad512e/image.png)

    
    - 모뎀에 대해서 만들었는데…
        
        ![](https://velog.velcdn.com/images/kcs05008/post/32bf09e1-9871-4149-848c-255f37eaa282/image.png)

        
    - 알고보니 이렇게 나뉘어질 수 있었던 거였음!
    - responsibility 두개
        - Connection management
        - Data communication
    - 알고보니 두개였던 거임~
    
    → 앞으로 문제가 어떻게 변할 것인가를 생각하고 같은 것으로 묶을 것과 뗄 것을 생각.
    
    불필요한 복잡성은 피해야한다.
    
    - 떼야할 것 같은데 미리 떼놓을까?
    
    → 아니야…. 그 상황이 올 때만 하자…
    

# Open Closed Principle

![](https://velog.velcdn.com/images/kcs05008/post/e2316057-612f-4339-be95-ee20457c7590/image.png)


OCP: 개방 폐쇄의 원칙

- soft entity는 확장에 대해서 열려있어야하고, 수정에 대해서 닫혀있어야한다.
    - 기존의 코드를 수정하는 일을 최소화하자는 뜻~

## Conforming to OCP

- Open for extension
- Closed for modification
- 어긴다면?
    - Design Smell 중 Rigidity의 냄새가 날 것이다…
    - 하나의 바꿈이 끝없이 다른 것들을 바꿔야 하는 일이 있을 것이다..

## 나쁜 예시

![](https://velog.velcdn.com/images/kcs05008/post/545b26da-00bf-4b10-b113-9b04fbc05ad9/image.png)


- incEngineerSalary를 만들기 위해서 다른 class에서 else if를 추가해줘버려야한다..
- 문제점(무슨 smell이 날까요?)
    - Rigid
        - 이런게 여러개 있으면 하나하나 다 바꿔줘야함
    - Fragile
        - 버그가 나타났을 때 찾아내기가 쉽지가 않어
    - Immobile
        - 다른 데서 쓸 때 조건문 모두 있어야 사용할 수 있당.
        - 아니면 빼줘야해 ㅜㅜ

## 좋은 예시

![](https://velog.velcdn.com/images/kcs05008/post/862cbebd-6360-4e0a-99c9-b96d73fb4093/image.png)


- incSalary는 각각의 클래스에서 만들어버려야함
- 그러면 다른 클래스를 추가하더라도 그것만 딱 만들어버리면 된다.
- 결과적으로 incAll은 closed(닫혀) 된다.

## Abstraction is the Key!

- Abstraction
    - 잘 변하지 않는 것, 고정된 것
- Program the class
    - interface 기반으로 하자(or abstract class)
    - implementation기반으로 하면 안돼요(concrete class)

![](https://velog.velcdn.com/images/kcs05008/post/ee103bba-88e4-4920-9ca0-10fa53902b49/image.png)


- 여기서 위의 그림대로 하면 server가 변경되면 client에 바로 영향을 주게 된다.
- 반대로 밑 처럼 interface를 통해서 연결되어 있다면 interface가 변하지 않는다면  client에 영향을 주지 않는다.

## 미래의 변화 예측해버리기

- strategy is needed
    - 어떤 종류의 변화가 일어날 것인지 고려
    - 변화가 많을 것이라고 생각되는 부분은 abstraction을 만들자
- Beware: Consider the cost
    - OCP를 항상 지키면 너무 힘들어진다.
    - 시간과 노력이 많이 듬
    - Abstraction은 복잡도가 증가한다.
- Do not put hooks in for changes that might happen
    - 한번 당하고 또 당하면 넌 바보야
    - 처음은 그냥 만들어두고, 변할 것 같을 때 abstraction 적용하자
    - 한번 바뀌는게 빨리 일어날 수록 좋다,,?
    - TDD를 사용하자

# Liskov Substitution Principle

![](https://velog.velcdn.com/images/kcs05008/post/796258f9-016e-41e2-a724-3b7d432022bf/image.png)


LSP: subtype은 base type을 대신해서 대체재로 동작해야한다

형식이 아닌 내용이라는 것에 집중

- inheritance를 쓸지 말지를 결정해주는 rule
- P라는 type의 subtype으로 C가 있다고 하면, P type의 객체가 C type의 객체로 쓰였을 때 변경되지 않는다.
    - 자식 클래스로 대신 던져줘도 돌아가는데 문제가 없다

## Subtyping VS Implementation Inheritance

- Subtyping
    - is_a 관계를 성립시킴
    - interface inheritance라고 불림
- Implementation inheritance
    - implementation을 재사용하기 위한 목적
    - 의미적인 관계를 만들어내는 것이 아님
    - code inheritance라고 불림
- 일반적인 OOP language는 두개를 동일하게 봄.
    - extend를 사용
    - 사실 subtyping이 맞음

## Reuse

![](https://velog.velcdn.com/images/kcs05008/post/66d8e4db-eba6-441c-a33f-f9988fa5bb02/image.png)


- List라는 것을 만들어두고, queue를 만들려고 하는데 어? list를 조금 바꿔서 쓰면 되지 않을까?
- 근데 이러면 안돼 왜냐면 LSP 위반이거든
    - 이렇게 의도치 않게 subtype을 설정해버리면 list의 자리에 queue를 갖다놔도 잘 돼야하는데 그게 안돼잖아.
    - queue가 list의 모든 일을 하는 게 아니므로,,
- 그러면 어떻게?
    
    → 상속이 아닌 list가 queue를 알도록 해버리자
    
    insert, delete를 할 때 list한테 맡겨버려
    
    두 개는 자식, 부모 뭐 이런 관계가 아니야. 상속을 쓴 게 아니야
    
- 더 좋게?
    
    → queue가 list의 interface를 알도록 해버리는겨
    
    상속으로만 생각하지 말고 다른 것들을 사용해서 진행해보자..!
    

## LSP가 어겨지는 경우

![](https://velog.velcdn.com/images/kcs05008/post/b00e3c17-e56d-411b-9635-8587c2681738/image.png)


sql.date와 sql.time은 util.date의 모든 것들이 있는 게 아니라 deprecate된 애들도 존재.

이렇게 되면 [util.date](http://util.date) 대신에 sql.date를 사용하면 문제가 생기겠죠?

그래서 `dateValue = date.getDate();`를 하면 오류가 나겠죠

왜냐면 sql.time에는 getdate가 deprecate되어있거든요.

### LSP를 어겨서 다른 걸 또 어기게 된다

P라는 type이 있고 그것의 자식인 C type이 있다.

잘 돌아가고 있었는데….

`f(PType x){}`라는 method에서 C type이 사용되지 않도록 하면 fragile해짐

그래서 CType이 들어오면 exception해버리도록 처리를 하면…?

이제는 f라는 함수는 PType의 하위 타입이 생길 때마다 신경을 써야한다

→ f가 closed하지 않다 → OCP를 위반해버렸다 ㅠㅜㅠ

# Dependency Inversion Principle

![](https://velog.velcdn.com/images/kcs05008/post/68b3870d-1958-4e23-9fd7-7b2b0efafa5f/image.png)


의존 역전 규칙 (DIP)

- High level module은 더 낮은 low level module에게 dependency를 가지면 안된다
- high, low 둘 다 abstraction에 대해 dependency를 가져야한다

- Abstraction이 detail에 대해서 dependency를 가지면 안되고,
- Detail이 abstraction에 대해서 dependency를 가져야한다

왜 Inversion?

- 객체지향 설계를 따르지 않고 그 전의 패러다임인 구조적 분석 설계를 따른다면 dependency를 invert(역전)시켜줘야했기 때문에

## 구조적 분석 설계를 아라보자

![](https://velog.velcdn.com/images/kcs05008/post/5f37a9c8-f813-4952-bf41-66ebfb8d848e/image.png)


- program이 하위의 모듈을 불러야하므로 module에 대해서 dependency를 가지게 됨
- 즉, 상위 레벨이 하위 레벨을 알고 있어야 한다.
- 가장 변화가 많이 일어나는 곳은 function(말단쪽)인데 그럼 function → module → program 다 바꿔줘야할 수도 있다

## Dependency Inversion

![](https://velog.velcdn.com/images/kcs05008/post/82064dc9-0fab-480e-9e8b-3a90ced444f3/image.png)


- program이 class1,2,3보다 상위임에도 불구하고, program이 class들을 직접 알도록 하는 것이 아니라 interface를 통해서 연결시켜버린다.
- 그러면 program은 자기보다 추상성이 높은 interface를 알게 되고, c
- class들은 이 interface들을 implement하는 관계에 있으므로 의존관계가 역전이 된다!(아까는 화살표가 내려가는 방향이었죠?)
- 그러면 class가 변경되더라도 interface는 영향을 받지 않으므로 program도 바뀔 일이 없다

## Inversion of Ownership

- DIP는 controll flow만 역전시키는 것이 아닌 owership도 역전시킨다
- service, interface가 있고, client가 interface를 통해 전달된다면, ownership이 service에 있다고 생각할 수 있는데 client와 interface에 있다고 생각.
- 그러면 client는 뭐가 바뀌는 지도 모른다

→ ownership을 공급하는 쪽이 가지는 것이 아니라 사용하는 쪽이 가져야 한다!!!!

### Layer architecture에 적용해보자

![](https://velog.velcdn.com/images/kcs05008/post/e73ba050-a052-4049-9672-126aba545f3f/image.png)


- policy layer를 구현하기 위해서 mechanism을 사용. 그 밑도 동일
- 하지만 이것은 하위 layer에 상위 layer가 dependency를 가지는 것.

![](https://velog.velcdn.com/images/kcs05008/post/1a292096-4748-44b8-b00d-e9f440e7365f/image.png)


- 이렇게 돼야한다!
- interface는 공급자가 아닌 사용자의 이름을 따라서 만들어버린다.
    - mechanism service가 아닌 policy service인 이휴

# Interface Segregation Principle

![](https://velog.velcdn.com/images/kcs05008/post/5e3e203a-5818-4423-b310-e6abbc650382/image.png)


ISP: 인터페이스 분리의 원칙

- client는 자기가 사용하지 않는 method에 대해서 dependency를 갖도록 강제되어선 안된다
- interface를 잘게 쪼개서 client specific하게 만들어주자

## Fat interface

- 서로 다른 client들한테 하나의 큰 interface를 만들어서 불필요하게 coupling을 만들어주는 것은 좋지가 않어
    
    → cohesive한 group으로 나누고 그것들을 각각으로 interface로 만들어야함
    

## 예시

![](https://velog.velcdn.com/images/kcs05008/post/cc5f0011-109a-4881-9ec2-ce9aeb6d970a/image.png)


필요한 method들이 다른데 하나의 interface에 집어넣어버렸다.

이러면 쓸데 없는 dependency가 생기죠?

![](https://velog.velcdn.com/images/kcs05008/post/cfee3169-9545-42a4-9bcd-54b1465faf10/image.png)


그래서 이렇게 interface를 분리해서 사용합시다.

그러면 의미 없는 dependency가 줄어들겠죠?

이러면 EnrollmentAPI가 바뀐다고 가정하면, 그것을 알고 있는 RoasterApplication이 영향을 받고, EnrollmentAPI를 구현하는 StudentEnrollment도 바뀌게 된다.

하지만 AccountAPI는 안 바뀌지롱~
