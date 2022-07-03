# 20. GRASP Pattern

General Responsibility Assignment Software Patterns

- Graig Larman’s 9가지 principle
    - Information Expert
    - Creater
    - Controller
    - Low Coupling
    - High Cohesion
    - Polymorphism
    - Pure Fabrication
    - Indirection
    - Protected variations

OOAD 작업을 할 때 수행해야할 task

- requirement 알아내기
- domain model 수립
- design model 만들기(domain model을 기반으로)
    - 클래스의 attribute, method를 추가
    - 클래스로 만들어진 object들의 interaction 정립

우리가 파악한 software class들의 responsibility를 어떻게 배정을 하고, 어떻게 협업을 할지

→ GRASP을 이용해서…!

# Responsibilities

- 클래스의 책임 or 의무

### 1. Knowing Responsibility

- 객체가 encapsulate하고 있는 데이터를 가지고 있는지(attribute가 어떤 class에 들어가야 하는지)
- 관련된 객체들을 알고 있어야하는 것(다른 객체에 대한 정보)
- 어떤 것을 만들어낼 수 있고, 계산해낼 수 있는지
    
    ex) Domain model (분석)단계에서 만든 Sale이라는 (conceptual)클래스가 디자인단계에서 큰 도움을준다(inspire)를 준다. → 총 합을 알아야하는 책임을 Sale이 진다.
    

### 2. Doing Responsibility

- 뭔가를 수행하는 측면의 responsibility
- 다른 객체의 initialization 수행
- 다른 객체를 control, coordinating
    
    ex) “a Sale is responsible for creating SalesLineItems”
    
    SalesLineItems 클래스를 만드는 것을 누가 책임을 져야할까…!?!??!?
    

→ GRASP은 객체지향 설계 중에서 responsibility(책임)을 어떤 class or object에 부여할지에 대한 논리적인 판단을 도와주는 pattern

주의) responsibility와 method는 다르다!

responsibility는 일반적으로 여러 method로 구현됨.

# Modularity

### 설계의 목표

- system을 여러개의 module로 나누고, 그것들에게 책임(responsibility)를 부여

### Modularity를 키우는 것이 좋아요

- total complexity를 줄여준다

**< Modularity를 증가시키기 >**

- 관심사의 분리(Sepatation of Concerns)
    - function(기능)들은 유사한 것들이 있다면 공통 모듈로 묶어버린다
- 정보 은닉(Information Hiding)
    - 모듈들 간의 interface를 작고, 간단하고, 잘 만들어야한다…

### Modularity 측정 방법 2가지.

- Coupling(결합도)
    - 두개 이상의 모듈 사이의 inter-dependency를 측정(얼마나 서로를 의지하는가)
    - 모듈 사이에 얼마나 서로 연관성이 있는가.
    - 낮으면 낮을수록 좋다..!
- Cohesion(응집도)
    - 특정한 모듈 하나를 측정
    - 하나의 모듈 내의 attribute들이 얼마나 관련성이 있는가!
    - 높으면 높을수록 좋다…!

---

## Coupling

### Concept of Coupling

- 두개 이상의 클래스(or 모듈) 사이의 inter-dependency의 정도
- 높은 coupling → 다른 것들이랑 상관관계가 많다
- 어떤 element가 변하면 dependency가 있는 다른 module한테도 영향을 끼친다
- Highly coupled ↔  Loosely coupled

- 특정한 모듈을 분리해서 이해하기 편하다
- Low Coupling이 더 좋다
    - reuse할 때 편하다(어떤 특정한 module을 떼어서 다른 프로젝트에서 사용하고싶다)
        - Coupling이 높으면  couple된 모든 클래스들을 다 가져가야함
    - 한 애가 바뀔 때 다른 모듈(클래스)를 바꿀 필요가 없다
    - 

### 다양한 형태의 coupling

![Screen Shot 2022-05-26 at 5.12.04 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_5.12.04_PM.png)

- type X로 표시된 class들은 type Y에 dependency(coupling)이 있는 것
- Y가 변하면 X에게 영향을 미친다

---

## Cohesion

- 한 모듈(클래스) 내의 element들 사이의 관련성을 측정하는 지표
- 관련성이 있는 책임이 한 모듈에 들어가 있는가? 가 중요한 포인트
- 단일   책임 원칙(SRP: Single Responsibility Principle)과 유사하다

![Screen Shot 2022-05-26 at 5.16.25 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_5.16.25_PM.png)

- 그림에서 같은 빗금을 가진 애들이 많을수록 cohesion이 높은 것
    - low cohesion을 보면 한 module에 다양한 빗금이 있음
    - high cohesion을 보면 같은 빗금끼리만 모아두고 나머지는 모듈 분리

High Cohesion을 추구하자!

- 이해하기 쉽고, 유지하기 쉽다
- reuse 좋아요
- Low coupling 유도 가능

# 예시 - POS 씨스템

- **Domain Model**
    
    ![Screen Shot 2022-05-26 at 5.20.08 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_5.20.08_PM.png)
    

## Creator Pattern

| Name | Creater |
| --- | --- |
| Problem | 누가 객체 A를 만들래? |
| Solution | class B가 만들어라! 어떨 때?
- B가 A를 기록하고 있을 때
- B가 A를 포함하거나 aggregate하는 관계에 있을 때
- B가 A를 밀접하게 사용하고 있을 때
- B가 이미 A에 대한 init data를 가지고 있을 때 |
- creation에 대한 responsibility를 부여할 때 많이 쓰임
- 쓰지 않는 경우
    - creation의 complexity가 매우 복잡할 경우
    - 성능 상의 이유로 기존 instance를 recycle해야하는 경우
    - 간단한 instance가 아닌 큰 instance(Family of Similar Classes)를 만들 경우
    - dependency injection과 같이 외부에서 우리가 어떤 객체를 쉽게 wiring(갈아 끼울 수 있도록)하는 장치를 사용할 경우
- 장점
    - low coupling
- 연관된 패턴
    - Abstract Factory
    - singleton

### Creator Example

![Screen Shot 2022-05-26 at 5.25.12 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_5.25.12_PM.png)

- Domain Model에서 class 간의 관계 (contains, desribed-by 등)을 도출
    - Sales Line Item의 생성을 누구한테 맡길까?
        - Sale 클래스가 SalesLineItem객체의 instance를 만드는 것이 좋겠다!
        - Sale이 Contain하고 있으니까!!
- 그래서 Design Model을 뒤에 만들 때 create해주는 관계를 결정해줄 수 있다

---

## Information Expert Pattern

responsibility를 충족하는데 필요한 정보를 가지고 있는 class가 responsiblity를 가져야한다.

| Name | Information Expert |
| --- | --- |
| Problem | responsibility를 누구한테 부여할까? |
| Solution | 정보를 가지고 있는 class가 담당해야지.. |
- Responsibility를 구성하는데 가장 많이 쓰이는 패턴
- information의 위치를 생각해보면 여러 객체에 흩어져있게 됨
    
    →Interaction이 필요하다
    
- 주의해야할 점
    - Separation of Concern(관심사의 분리) 측면에서 생각해보면 바람직한 다른 대안이 있을 수 있음
        - sale에 필요한 것은 모두 sale 안에 있어야한다.
        - 하지만 그렇게 되면 low cohesion이 되기 때문에 안 좋아요
- 장점(Information Expert는)
    - Information encapsulation을 도와준다. → low coupling 유도 가능

## Information Expert Example

![Screen Shot 2022-05-26 at 6.07.06 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_6.07.06_PM.png)

- Sale 클래스가 sale LineItem의 정보를 가지고 있으므로 Sale 클래스가 Sales LineItem을 만든다
- Design Model을 보면 Sale이 total을 뱉어줄려면 SalesLineItem이 subTotal(각각의 Item의 Price합)을 뱉어줘야함
- 하나의 product price는 productDescription이 뱉어줘야 SalesLineItem이 subTotal 뱉어줄 수 있음
- 해당하는 정보가 있는 곳에 Responsibility를 부여해라.

---

## Controller Pattern

| Name | Controller |
| --- | --- |
| Problem | UI layer의 이벤트를 받았을 때 처리하는 애는 누구? |
| Solution | 1. 전체 시스템을 대표할 수 있는 객체를 만들어서 root object로 지정하고 활용하자
ui layer에서 domain layer로 들어오는 최초의 객체
(facade controller)
2. use case나 session별로 controller를 나눠서 해당하는 것마다 배정해줌
(use case controller or session controller) |
- 종류
    1. Facade Controller
        - 하나에 다 때려박기
        - Responsibility 계속 올라감… Cohesion 증가
    2. Use-case controller
        - use case별로 담당 controller를 배정해서 나눠버림
- Controller 뚱뚱쓰
    - 나눠버리자
    - facade를 사용하면 delegate를 사용해서 자기가 일 하는 걸 떠넘겨버림
- 장점
    - GUI에서 application logic이 안 담기게 됨
        
        → 뒷 단을 reuse하기 좋아짐
        
    - 현재 usecase의 상태에서 어떤 상태인가를 파악하기 쉬움
        
        ex) endSale이 나타나기 전까지 makePayment 안 불러도 됨
        

### Controller Example

![Screen Shot 2022-05-26 at 6.25.44 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_6.25.44_PM.png)

- UI layer에서 Domain Layer로 넘어왔을 때 어떤 처리를 해줘야하는데 그것을 하는 친구가 Controller
- 그러면 Design Model에서 register가 item 들어오는 것 처리하면 되니까 배정해버림
- 아니면 특정한 use case 전용 controller를 만들어줘버림

---

## Low Coupling Pattern

가능하면 coupling이 낮아지는 방향으로 진행한다

| Name | Low Coupling |
| --- | --- |
| Problem | coupling을 어떻게 더 줄일까? |
| Solution | 두가지 선택지가 있을 때 요 놈을 사용하면 Low Coupling이 보장되는 쪽으로 선택을 할 수 있게 된다.
예를들어 여러 GRASP패턴들이 충돌할때 
A한테 Responsibility를 부여해!
아니야 B한테 부여해! 그러면 이때 
Low Coupling을 쓴다!! |
- 독립적이게 할 수 있고 변화에 대한 impact를 줄일 수 있다.
- 많은 디자인 결정 사항에서 여러 다른 패턴들의 Recommendation이 다를 수 있는데 이때 Low Coupling사용해서 선택을 할 수 있다.
- High Coupling이 생기는 경우가 있는데 충분히 용인 가능한 상황이 있음
    - 안정적인(Stable한)(변화가 거의 없는) Global Object에 대해서는 괜찮다~
        
        ex) java.util의 친구를 쓰더라도 coupling이 높더라도 안정적이고 바뀔 일이 없으니까 괜찮음
        

### Low Coupling Example

![Screen Shot 2022-05-26 at 6.39.43 PM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-26_at_6.39.43_PM.png)

두개 중에 어떤 걸로 고를까요~

Option 1

- 모든걸 register가 다 진행함
- register가 payment를 통해 값 받아서 sale한테 주면 되겠구나~

Option 2

- register가 sale한테 payment에 대한 처리를 맡겨버림!!!!!(delegate)

→ Low Coupling 관점에서 Option 2를 고르게 된다.

- Coupling이 더 적게일어나는 옵션을 고르자
- 우리의 주 목적은 payment와 sale을 연결시켜 주는 것
- register가 payment에 대한 정보를 알고 있게 되면 coupling 높아짐
- 그래서 coupling을 낮출 수 있는 option2 를 고르게 된다!

---

## High Cohesion Pattern

| Name | High Cohesion |
| --- | --- |
| Problem | 어떻게 집약적이고, 이해하기 쉽고, 관리하기 쉽고, 부작용이 적고, Low coupling을 유도할 수 있는 아이를 만들 수 있을까? |
| Solution | 얘도 다른 애들 비교할 때 기준으로 사용이 가능 |

### High Cohesion Example

![Screen Shot 2022-05-27 at 12.25.31 AM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-27_at_12.25.31_AM.png)

- Option 1보다 Option 2를 고르게 됨.
- Option 1에서 계속 이런식으로 Register가 뚱뚱해지면 상당히 Cohesion(응집도)가 떨어지게 된다.
- 다른 것들이 추가되면 Register가 방대해지게 됨

---

## Pure Fabrication Pattern

순수하게 새로운 것을 만들어내는 

| Name | Pure Fabrication |
| --- | --- |
| Problem | high cohesion, low coupling 버리기 싫어… |
| Solution | responsibility가 추가되는 것이 예상될 때 아예 새로운 친구를 만들어내자!  |
- 만약 새로운 친구를 그냥 만들어버리면 높은 Cohesion, 적은 Coupling, Reuse도 굳이다~

### Pure Fabrication Example

![Screen Shot 2022-05-27 at 12.29.00 AM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-27_at_12.29.00_AM.png)

- domain model에는 등장하지 않은 새로운 class를 만들어버림

---

## Polymorphism Pattern

디자인에서 polymorphic한 operation을 제공

연관된 대안이나 Behavior가 Type에 따라서 달라진다면 그런것들을 위해 Polymorphic한 Operation들을 만들어 놓고 Type이 다른 Class마다 서로 다른 Behavior를 보인다.

![Screen Shot 2022-05-27 at 12.44.35 AM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-27_at_12.44.35_AM.png)

- polymorphic하게 interface를 만들어두고 서로 다른 behavior마다 class를 만들어서 override를 해주면 좋다~

**장점**

- 더 쉽고 더 reliable하다
- interface를 따르면서 additional behavior를 책임지면서 이전의 것들에 관여 x

**단점**

- indirection에 기반하기 때문에 class 개수가 너무 많아진다
- indirection은 프로그래머가 읽기 힘들어짐

**의의**

- 근본적인 design principle중 하나
- 유사하지만 그 안에 변종들을 만들어 사용한다…!
- 과도한 polymorphism을 사용하면 어질어질해져요
    - 확장되는 상황이 왔을 때 이렇게 하고 잘 모르겠을 때는 이렇게 하지 말어라..
    - 확장되는 확실한 근거가 있을 때만…

---

## Indirection Pattern

객체를 설계할 떄 주어진 객체들 간의 direct coupling을 만들기 싫을 떄 어떻게?

→ intermidiate object를 둬서 intermediate object를 중심으로 direct coupling 없애버림/ intermediate object에게만 Coupling을 준다.

![Screen Shot 2022-05-27 at 12.57.01 AM.png](20%20GRASP%20Pattern%20b4e7f92f9d574a71b076596b0f4d577e/Screen_Shot_2022-05-27_at_12.57.01_AM.png)

- TaxMasterAdapter를 만들어서 Sale과 외부의 TaxMasterSystem을 이어주는 역할을 하게 함

정의

- direct coupling을 회피하고 싶을 때가 생김
- intermediate unit을 만들어서 회피

장점

- Low Coupling

예시

- Adapter, Facade, Proxy, Mediator

---

## Protected Variations Pattern

system이 stable하게 있지 않을 때 그 부분을 찾아서 interface로 감싸서 구현을 해버림

변동 지점(안정하지 않은부분)을 파악해서 stable한 interface를 만들어 준 다음에 interface를 중심으로 polymorphism 같은것을 활용해서 variation들을 여러 형태로 둬서 

- OCP(open close principle) == PV
    - 기능을 확장시킬 가능성을 염두해두고, 확장될 때 원래의 코드를 건드리는 것을 최소화하고 할 수 있도록

장점

- flexibility, protection
- 구조화된 디자인 제공

PV에 영감을 받은 친구

- C# , Ada, Eiffel (Uniform Access)
    - method call과 field access가 같은 형태로 표현됨
- Service lookup
    - JNDI
- Reflective or meta-model design