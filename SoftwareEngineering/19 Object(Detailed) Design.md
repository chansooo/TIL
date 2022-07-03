# 19. Object(Detailed) Design

이제부터 상세 설계 스타트!!

- Dynamic models(동적인 모델):
    - logic 디자인할 때, 코드의 동작, 메소드 body
    - UML interaction diagrams
        - sequence diagram
        - communication diagram
- Static Models(정적인 모델):
    - 단순하고 파악하기 쉬운 정보들
    - package 정의, class 이름, attributes, method의 signature
        - UML class diagrams
- Agile modeling 추천 예시(순서!!)
    1. interaction diagram 그려보기(dynamic model) → behavior들을 파악을 해보기!
        1. 상관관계 파악할 수 있음
        2. Class Diagram을 먼저 그리는 것 보다 Class, Object간의 Responsiblity를 먼저 파악한 뒤 그 다음 파악된 Responsibility를 Static으로 표현하는게 순서가 맞다!
    2. class diagram 그려보기(static diagram)
        1. 상세하게!
    
    ![Screen Shot 2022-05-18 at 4.17.23 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_4.17.23_PM.png)
    

## UML보다 현업에서 중요한 것은 Object Design Skill

- 근본적인 지식은 디자인에 관한 지식이다....
- reponsibility의 배정
- design pattern

## UML Interaction Diagram

 message를 이용해서 어떤 식으로 서로 상호작용을 일으키는가를 보여주는 그림

→ dynamic object modeling

![Screen Shot 2022-05-18 at 4.27.08 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_4.27.08_PM.png)

### Sequence Diagram ↔ Communication Diagram

- Sequence Diagram에서 표현할 수 있는 모든 정보들은  Communication Diagram으로 표현할 수 있음
- 정보는 동일하지만 focus가 다르다
    - sequence diagram: interaction의 시간적인 순서에 집중
    - communication diagram: 객체들 간의 상호작용(누가 누구랑 연결?)
        - 공간을 덜 사용하고 표현 가능(화이트보드에서 충분히 표현 가능해요~)
        - message 순서(10진법)
    - 예시

![Untitled](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Untitled.png)

### Communication Diagram: Link

- Link
    - 객체들 사이를 연결해주는 선
    - navigation이 가능하거나 보일 때 연결
    - 두 객체 간의 연결이기때문에 inheritance까지 표현해주지는 못한다
        - dependency와 aggregation같은 것들 표시

![무제.jpg](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/%E1%84%86%E1%85%AE%E1%84%8C%E1%85%A6.jpg)

### Communication Diagram: Message

- 객체 간의 message call은 message expression으로 나타냄
    - 메시지 이름
    - 순서(타이밍과 관련된 정보들)
    - Message의 방향(화살표로)
- 하나의 링크로 여러 message expression 표시 가능(밑에서 1,2,3)

![스크린샷 2022-05-24 00.12.32.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_00.12.32.png)

### Communication Diagram: Creation

- 다른 객체를 만들어 내는 법.
- message 이름을 create로 만들면 a가 b를 만든다는 의미(new라고도 쓰기도 함)
- 표현 방식에는 세가지
    1. message name에 **create**붙이기
    2. 객체 뒤에 **{new}** 붙이기
    3. UML의 stereotype인 **<<create>>** 사용하기
    
    ![Screen Shot 2022-05-18 at 10.54.04 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_10.54.04_PM.png)
    

### Communication Diagram: Numbering

- 첫번째 message에는 번호를 붙이지 않음
    
    ![Screen Shot 2022-05-18 at 10.58.18 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_10.58.18_PM.png)
    
    첫번째 메시지 : msg1
    
    두번째 메시지 : 1. msg2
    
    세번째 메시지 : 1.1: msg3
    
    네번째 메시지 : 2.msg4
    
    이 상황에서 만약 1.1.1이라는 message가 추가되면, 네번째 메시지는 1.1.1이 된다.
    

### Communication Diagram: Conditional/Mutually Exclusive message

- 조건이 붙는 message
    
    ![Screen Shot 2022-05-18 at 11.00.30 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.00.30_PM.png)
    
    - 만약 test1이 true면 msg2, msg3가 실행
    - 만약 false면 msg4, msg5가 실행된다.

### Communication Diagram: Iterative messages

- 반복되는 메시지
    
    ![Screen Shot 2022-05-18 at 11.02.26 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.02.26_PM.png)
    
    - 반복문을 저런식으로 표현 가능. 1~n까지 getSubtotal라는 msg를 SalesLineItem이라는 객체 여러개에다가 계속 보낸다.
    - 밑은 1* 이라고 써서 반복 횟수가 명확한 경우에 생략 할 수 있다.

### Communication Diagram: Static Method call

- 객체한테 보내는 것이 아닌 클래스에 보내는 message
- **<<metaclass>>**라는 표기법을 사용해서 클래스 자체라는 것을 알려줌

![Screen Shot 2022-05-18 at 11.04.08 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.04.08_PM.png)

### Communication Diagram: Polymorphic Method Call

- polymorphism처럼 subClass 들에 똑같은 method가 override돼서 다른 역할을 할 때 표현하는 것

![Screen Shot 2022-05-18 at 11.07.56 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.07.56_PM.png)

![IMG_D8B44A00623B-1.jpeg](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/IMG_D8B44A00623B-1.jpeg)

1. 첫번째 박스 : Register라는 Class가 Abstract Class인 Payment에게 authorize()라는 Polymorphic한 Call을 한다.(하지만 Payment는 Abstract Class라 Instance를 못만들지만 저렇게 쓰면 Payment를 만족하는 객체한테 authorize()라는 메세지 전달.)
2. 두번째 박스 : 첫번째 시나리오로 DebitPayment 에서는 Foo라는 Class에 doA, doB를 전달하는 것으로 authorize를 구현했다.
3. 세번째 박스 : 두번째 시나리오로 CreditPayment에서는 doX 를 Bar에 전달함으로 authorize()를 구현했다. 이런 뜻임. 

### Communication Diagram: Asynchronous and Synchronous Calls

- 화살표 모양이 좀 달라요~
- 두줄로 표현된 Object : Active Object임.
- Asynchronous Call : Method Call하고 완료되기 전이지만 Control이 Caller에게 다시 돌아와서 응답을 기다리지 않고도 진행할 수 있는 Call
- Synchronous Call : 일반적인 Method Call. Method Call하고 완료되고 return 받을때까지 Caller가 멈춰서 기  Call
    
    ![Screen Shot 2022-05-18 at 11.13.44 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.13.44_PM.png)
    
    - 2번(run)이 종료가 안 되더라도 3번(runFinalization)을 보내버릴 수 있다!
    

### Sequence Diagram: Iteration over a Collection

- 몽말인지 알지? 존ㄴ ㅏ 반복

![Screen Shot 2022-05-18 at 11.21.50 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.21.50_PM.png)

### Sequence Diagram: Messages to Classes to Invoke Static Methods

- 박스(lifeline box)에 <<metaclass>>라고 표시를 해주고 클래스 자체라는 것을 알려주고 static Method를 호출
- Static Method Call : <<metaclass>>
    
    ![Screen Shot 2022-05-18 at 11.29.50 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.29.50_PM.png)
    

### Sequence Diagram: Polymorphic Method Call

- abstract나 interface object가 구현되어서 진행될 때

![Screen Shot 2022-05-18 at 11.31.15 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.31.15_PM.png)

- 공통된 부분 그려주고, 그 밑에 다형적인 애들 따로 그려줘버린다
- 3개 다 그려줘야한다!!

### Sequence Diagram: Asynchronous and Synchronous Calls

- 박스떼기 줄 두개, 화살표 날카롭게~
    
    ![Screen Shot 2022-05-18 at 11.34.33 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.34.33_PM.png)
    

---

# UML Class Diagram

- 같은 UML Class Diagram이 다양한 역할로 쓰일 수 있음
- 목적에 따라(개념, 디자인) Class Diagram이 다르게 쓰임.
    
    ![Screen Shot 2022-05-18 at 11.50.36 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.50.36_PM.png)
    
    - Domain model만들 때: 분석 작업을 할 때 Class Diagram사용 가능.
        - 개념적인 측면
    - Design class diagram(DCD)를 만들 때 에도 Class Diagram사용 가능.
        - 디자인적인 측면
        - 세세한 방향까지 .. Detail하다 더!!!

## Associations in Different Perspectives

![Screen Shot 2022-05-18 at 11.57.42 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.57.42_PM.png)

- Domain model
    - software적인 관점이 아니므로 navigation arrow를 많이 쓰지 않는다
- DCD
    - UML 표기법을 따름
    - 바로 다음 단계가 개발이기 때문에 충분히 자세한 표기가 필요하다

## Keywords

- 부가적으로 정보를 더 주는 것
    
    ex) interface를 표시해주기 위해서 <<interface>>
    
    ![Screen Shot 2022-05-18 at 11.59.09 PM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-18_at_11.59.09_PM.png)
    

- 주어진 model element의 정보를 추가로 주는 것

## Streotypes, Profiles, Tags

- **stereotype - 유저들이 마음대로 정의해서 사용 할 수 있음.**
    - 주어진 어떤 모델링 컨셉을 사용자(User)가 define하는 것

profile을 정의해서 더 자세한 것을 확장해서 표시

![Screen Shot 2022-05-19 at 12.02.40 AM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-19_at_12.02.40_AM.png)

- Standard를 확장해서 사용
    - 확장하기 위해서 stereotype, tag, constraint를 설정
- 예시 <GUI Profile>
    
    ![Screen Shot 2022-05-19 at 12.04.36 AM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-19_at_12.04.36_AM.png)
    

우리가 정의한 stereotype으로 싹 조질 수 있다.

![스크린샷 2022-05-24 02.12.22.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_02.12.22.png)

결과를 보자

- Design Class Diagram
    
    ![Screen Shot 2022-05-19 at 12.07.44 AM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-19_at_12.07.44_AM.png)
    

### Interaction과 Class Diagram과의 상관관계

- interaction diagram을 먼저 그려라
    - operation, 사이의 logic 파악하고, 그것을 이용해서 class diagram 그려라~
- 그리고 보통 dynamic modeling과 static modeling을 함께(동시에) 진행한다

---

![Screen Shot 2022-05-19 at 12.22.32 AM.png](19%20Object(Detailed)%20Design%2012cfb8bc38fa4ae5b0f68cc2725700b9/Screen_Shot_2022-05-19_at_12.22.32_AM.png)

1. doA() → X라는 Class에 있어야한다. 그래서 외부에서 doA()를 부른거임.
2. doA()를 통해 Y라는 Class가 만들어짐. by create()
3. doB() → Y에 정의되어 있음.
4. doD() → X에 정의되어 있어야함. , 호출하는 코드는 Y에 있어야함!
5. y1 ≠ y2
6. deE() → Y에 정의되어 있음.

```cpp
public class X{
	void doA(){
	}
	void doD(Y y){
		Y y1 = new Y();
		y.doD(y1);
	}
}
```