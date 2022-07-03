# Logical Architecture and UML Package Diagrams

![Screen Shot 2022-05-10 at 1.19.08 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_1.19.08_PM.png)

Design은 크게 세가지 부분으로 나뉘게 되는데 

- Package Diagram
    - Architecture 설계
- Interaction Diagram
    - class 간의 interation
- class diagram
    - 상세설계

# Logical Architecture

- 요구 분석 → 설계로 넘어가는 과정
- architecture 설계, 상세 설계 중에서 architecture design 부분

## UML Package Diagram

- 어떤 모듈들이 어떤 패키지에 들어가는지 그림으로 표현

## Logical Architecture와 Layer

- Logical Architecture
    - software class를 package, subsystem, layer로 대규모로 구성
    - 위의 element가 어떤 hardware와 매핑될 지는 결정되는 것이 아님
        - deployment diagram에서 하는 것임(여기선 안한다)
- Layer
    - class, package, subsystem을 그루핑하는 것
    - 구별이 될 수 있는 responsibility를 가짐
    - 상위의 layer는 하위 layer를 사용할 수 잇음
        - 반대는 안됨
    - 객체지향에서의 layer
        - User Interface layer
            - 사용자 인터페이스 부분
        - Application logic and Domain objects layer
            - 도메인 concept을 나타내는 소프트웨어 object
        - Technical Service layer

### Layer Architecture의 종류

- Strict layered architecture
    - 바로 밑의 layer까지만 사용이 가능
    - 네트워크 프로토콜과 같은 곳에서 사용
        - information system같은 곳에서는 사용을 안함
- Relaxed layered architecture
    - 밑의 어느 layer이든 사용이 가능
    - information system에서 많이 나타남
    
    ![Screen Shot 2022-05-10 at 2.15.02 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_2.15.02_PM.png)
    
    - strict는 UI에서 Domain만 사용 가능.
    - relaxed는 UI에서 모두 사용 가능

# Software Architecture란?

- 소프트웨어 시스템에서의 아주 중대하고 원천적인 구조
- 그 구조가 앞으로  어떻게 만들어낼지 알려주는 규칙이라고 할 수 있다.
- 각각의 structure는 software element와 그들간의 관계로 구성이 된다.
- software element와 관계에는 property(속성)가 있다.
- 구조를 결정하는 것.
- 앞으로 변화에서 근본적인 부분을 결정하는 사항
- software architecture design: software architecture를 디자인 하는 것. software design의 부분집합
    - design: 위를 포함해서 상세한 디자인까지

## Applying UML

- logical diagram은 UML의 package diagram으로 표현할 수 있다.
    - package diagram은 그루핑을 하는 도구
    - 클래스, 패키지, use case등을 전부 붂는다
    - nesting package도 가능
    - 자바의 패키지와 같다

## Package Dependency

- 점선으로 dependency를 표현한다
    
    ![Screen Shot 2022-05-10 at 2.39.55 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_2.39.55_PM.png)
    

### Design with Layers

- lower layer
    - 일반적인 서비스 , low-level
- higher layer
    - 만들고자 하는 system의 specific한 부분을 담는다
- higher가 lower 참조 가능. 역은 안됨
- 예시
    
    ![Screen Shot 2022-05-10 at 3.10.53 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_3.10.53_PM.png)
    
    - 밑으로 갈수록 general한, 어디에서도 쓰일 수 있는 아이들
    

## Layer 사용의 장점

- 우리가 신경써야 할 관심사들을 분리할 수 있음
    - 서로 다른 관심사를 분리를 하지 않으면 보기 힘들어요 ㅜㅜ
- low level service와 high level service를 구별할 수 있도록 해줌
- general한 service와 application-specific한 service를 구별할 수 있도록 해줌

→ coupling과 dependency를 줄일 수 있고, cohesion, reuse potential, clarity를 높여줄 수 있음

- coupling: 두 유닛 사이의 관계. 적으면 적을수록 좋음
- cohesion(응집도): 한 유닛 내의 element끼리의 관계. 내부적인 것.
- complexity가 캡슐화되고 분해가능하게 됨
- 새로운 implementation에 의해 replace될 수 있음
- Lower layer로 갈수록 reusable한 기능을 담고 있음
- 몇몇 layer들을 분리할 수 있음
- 개발 팀의 작업을 결정할 때 logical하게 나눠놓으면 팀 간의 일을 쉽게 나눌 수 있는 단위가 될 수 있음

- 예시
    
    ![Screen Shot 2022-05-10 at 3.39.10 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_3.39.10_PM.png)
    
     
    
    # Domain Layers and Domain Objects
    
    - 일반적인 software system에서는 UI logic과 application logic으로 나눠짐
    - application logic을 어떻게 객체로 구현할 수 있을까?
        - software object를 만들기 위해서 real-world domain에서 파악할 수 있다.
        - 그리고 구현한다
            - 예시: real-world POS에서 sales와 payment가 있는데 이러한 객체를 클래스로 만들어준다.
        - 이러한 software object를 `Domain object` 라고 한다.
            - Domain object는 problem domain space를 대표하게 됨
            - 그리고 그것은 application과 business logic을 포함하게 됨
                - 예시: Sale이라는 object를 만들었다면 real-world처럼 계산을 하는 로직을 넣는다
    
    ### Domain Layer 와 Domain Object의 관계
    
    - 객체 중심으로 software를 구성한다는 것은 현실에 있는 Domain model에서 파악한 중요한 concept들을 보고 도움을 받는다(실제 domain과 매칭이 잘 돼요)
        
        ![Screen Shot 2022-05-10 at 3.52.52 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_3.52.52_PM.png)
        
        - 분석단계의 domain model이 설계 단계에 직접적인 도움을 준다.
        - representational gap을 줄여준다!(현실과의 괴리)
        - 주의: domain model(요구분석레벨)과 UP Design Model의 domain layer of the architecture(design level)은 다른 것이다!
        

### Tier, layer, partition

Tier

- logical한 구조와 physical한 구조를 연결해주는 것
- 논리적인 구분이 아님!

Layer

- 수직적인 slicing
    - higher layer, lower layer

Partition

- 수평적인 분류 방법
- layer 내에서 여러 관심사나 그룹으로 나누는 것

→layer와 partition은 logical architecture과 연관됨

## Model과 View를 나누자!

### Model-View separation principle

1. non-ui object가 직접적으로 ui object랑 연결되면 안된다
    - ui는 자주 바뀌기 때문에 직접적으로 연결되어 있으면 나중에 ui를 바꿀 때 non-ui object까지 바꿔줘야하게 됨
2. application logic과 UI object method를 구분해야한다
    - 같은 곳에 놓으면 안돼요

- View → User Interface Layer
- Model → Domain Layer
- 모델이 직접적으로 뷰를 알지 못하도록~

### Motivation for Model-View Separation

- 가능하면 UI를 변경했을 때 Logic의 변경을 막고싶기 때문에
- 같은 data를 보여줄 수 있는 여러개의 ui의 layer를 구별하지 않으면 재사용성이나, 변경할 때 문제가 생김

## SSD, System Operation, Layer과의 관계

![Screen Shot 2022-05-10 at 4.05.54 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_4.05.54_PM.png)

- 점선 좌측에 있는 system sequence diagram 형태
    - sequence 따라서 진행 과정을 보여줌
- 이것을 점선 오른쪽에 있는 형태로 software적으로 구현

![Screen Shot 2022-05-10 at 4.09.15 PM.png](Logical%20Architecture%20and%20UML%20Package%20Diagrams%20d1f9a17c251e4d06bbe12341092c08bd/Screen_Shot_2022-05-10_at_4.09.15_PM.png)