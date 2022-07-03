# 18. MVC

### Model-View-Controller Pattern - 아키텍쳐 패턴임.

- MVC에서 사용되는 디자인 패턴들
    1. Strategy pattern
    2. Observer pattern(소공에서 좀 다룰거임)
    3. Composite pattern

### MVC의 구성요소

- **Model**
    
    : application object(비지니스 로직을 담고 있음)
    
    : 자료구조나 핵심 알고리즘이 여기 들어있다.
    
    : 공을 많이 들인다.
    
- **View**
    
    : UI(화면에 직접 보이는 부분)
    
- **Controller**
    
    : UI가 user input에 반응하는 방법에 대한 정의
    

### MVC Structure

![스크린샷 2022-05-14 13.11.20.png](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_13.11.20.png)

- **Model(Non-UI Layer- 하위레이어)(데이터를 가지고 있는 부분)**
    
    : 실질적인 데이터를 가지고 있다.
    
    : 데이터가 바뀌면 View에 알려준다.(Change Notification)
    
    : UI layer와 구분되기 때문에 하위 layer (Model) → 상위 layer (UI)로 직접적인 참조는 할 수 없다.
    
    자주 변경되는 UI 계층의 변화에 대해 side effect를 줄여야 한다
    
    non UI x→ UI, UI → non UI
    
    다만 데이터의 변경이 생겼을 때 옵저버 패턴을 이용해서 Notificatoin하는 약한 coupling은 존재
    
- **View(UI Layer - 상위레이어) - 눈에 보이는 것, 결정권 별로 없음, 수동적임.**
    - 모델에 있는 데이터들을 렌더링한다(눈에 보이게끔..)
    - Model에 데이터가 Update됐다는 통보를 받으면 Model에 Request(요청)해서 Update된 Data들을 받아온다.
    - User가 버튼을 누르는 등 Gesture를 취하면 Controller에게 알려준다.(해석해달라고 부탁함)
    - Controller의 요청에 따라 보여주기만 하는 수동적인 존재
    - Model에서의 데이터를 표시만 해준다.
- **Controller(UI Layer - 상위레이어)(Brain역할)**
    - application logic을 다루는 것은 아니고, UI 작동방식에 대해 다룸
    - 어플리케이션의 행동을 제어
    - User Action에 따라 Model의 데이터를 Update한다.
    - View를 선택
- 동작순서
    1. User가 View를 통해 interact(버튼누르기)한다.
    2. View가 Controller에 버튼이 눌렸다고 알려줌
    3. Controller가 User의 Input을 Handling(해석)해준다.
    4. Controller는 Model의 `increase()`라는 메소드를 호출해서 logic을 실행시킴
    5. Model은 자기가 갖고있던 Data를 변경하고 Model의 변화를 알고 싶어하는 View에 알려준다.
    6. View는 그걸 인식하고 화면에 보여준다.(바뀐부분을 보여준다)

UI Layer가 상위 Layer

Non-UI Layer가 하위 Layer

상위 Layer에서 하위 Layer는 부를 수는 없다.

- observe pattern에서 model이 변하면 view에 notification을 해준다. 그러면 query를 날려서 값을 render할 수 있다
- UI layer는 Non-UI Layer를 알고잇지만, Non-UI Layer는 UI Layer를 알고있지 못한다
    - oberver라고 하는 interface만 알고 있다.
- UI Layer는 자주 바뀌게 되고, Non-UI Layer는 잘 바뀌지 않는다
    - Non-UI Layer가 UI Layer를 알고 있게 한다면 계속 바뀌는 UI Layer에 맞춰 계속 바꿔줘야한다
    - 따라서 Non-UI Layer는 UI Layer를 몰라야한다.(Non이
    - UI-Layer의 수정사항이 훠어러어얼씬 많이 생긴다.

### Motivations

- 하나의 Enterprise data가 여러 형태의 뷰로 표현되고자 한다.
- Enterprise data는 여러 상호작용을 통해 갱신될 수 있다.
- UI에 대한 변화가 core functionality를 가지는 component(Model)에 영향을 끼치지 않고자 한다.
    
    → Non-UI Layer가 UI Layer를 직접적으로 모르게 하면 된다.
    

### Solution

- **책임 분리(Model, View, Controller 3개의 모듈로 나눈거임.)**
    - Core business model
        1. presentation
        2. control logic
    - 하나의 data model을 여러 뷰를 통해서 표현할 수 있도록
    - 자주 변경이 일어나는 UI의 변화가 Model에 영향을 끼치지 않도록
        
        ⇒ 구현, 테스트, 유지보수의 용이성
        
- **Model**
    
    : enterpirse data, business rules
    
- **View**
    
    : 화면을 렌더링하는 기능
    
- **Controller**
    
    : 사용자의 상호작용을 model과 연결해준다
    

### MVC의 디자인 패턴들

- **Observer pattern**
    
    : Model의 상태 변화가 일어나면 관심있는 Object들에게 notify해준다.
    
    : View와 Model을 Decouple
    
    Object들의 detail에 대해 몰라도 변화만 통보해준다.
    
    ⇒ publisher - subscriber를 decoupling시키기 위한 패턴
    
    Publisher : Model
    
    Subscriber : Model의 변화를 알고싶어하는 다른 Object들(View..)
    
- **Composite pattern**
    
    : UI의 계층적인 구성을 위해서 그룹화시킨다.
    

- **Strategy pattern**
    
    : View-Controller 사이에서 View에게 주어진 user input를 Controller에게 위임해서 판단하도록 한다.
    
- Factory, Decorator patterns 도 MVC에서 사용한다!

### Observer Pattern

![데이터가 compact하면 직접 전달할 수 있고, 그렇지 않은 경우 notify만 해도 된다.](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_14.17.32.png)

데이터가 compact하면 직접 전달할 수 있고, 그렇지 않은 경우 notify만 해도 된다.

: 하나 이상의 관심 있는 객체들에게 상태 변화를 알려주는 패턴

신문과 유사하게 작동한다.

- **publisher**
    
    : 상태 변화가 생기면 구독자에게 알려준다.
    
- **subscriber (=observer)**
    - **subscribe(구독신청)**
        
        : 구독하는 동안에는 publisher의 알림을 받는다.
        
    - **unsubscribe(구독취소)**
        
        : 더 이상 publisher의 정보를 받고 싶지 않을 때 취소할 수 있다.
        
- Publisher : Subscriber = Subject : Observer

![Screen Shot 2022-05-13 at 4.56.51 PM.png](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/Screen_Shot_2022-05-13_at_4.56.51_PM.png)

- cat, dog, mouse는 현재 Subscriber
- Duck은 현재 Subscriber 아님
    
    ![Screen Shot 2022-05-13 at 5.17.52 PM.png](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/Screen_Shot_2022-05-13_at_5.17.52_PM.png)
    
- 구독 신청을 하고 Observe 할 수 있다!
- 값 자체를 Model이 넘겨줄 수도 있고, Notification만 해줄 수도 있다!

![옵저버 패턴의 클래스 다이어그램 예시](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-14_14.20.49.png)

옵저버 패턴의 클래스 다이어그램 예시

- Subject는 Observer interface만 알고있다.
    - Subject 입장에서는 이들이 알고 있는 Observer에 대한 지식이 거의 없음
    - Update만 가능
    - Concrete한 Observer는 다른 많은 기능을 가지고 있지만, Subject는 알 수 없다~
    - Subject는 자신을 구독하고 있는 Observer들이 그냥 Observer interface의 한 instance구나~ 이런식으로만 알고 있음.

### Java의 Observable Class → Library로 제공한당.

- Observable(Publisher) Side
    - notification을 보내기 위해
        - Call `SetChanged()`
        - `notifyObservers()`, `notifyObservers(Object arg)`
- Observer(Subscriber) Side
    - 옵저버로 등록하기 위해
        - [Java.util.Observer](http://Java.util.Observer) 인터페이스 구독
    - subscribe하기 위해
        - Call `addObserver()` on ant Observable object
    - unsubscribe 하기 위해
        - Call `deleteObserver()`
    - update받기 위해
        - Implement `update(Observable o, Object arg)`
    
    `setChanged()` , `notifyObservers()` 
    

# MVC Example with Bouncing Ball

---

## Model

![Screen Shot 2022-05-13 at 6.20.17 PM.png](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/Screen_Shot_2022-05-13_at_6.20.17_PM.png)

- ball의 motion control
- window 사이즈 알아야함 → 언제 Ball이 벽에 부딪히는지 알 수 잇음.
- Model은 Observable이어야한다(Model은 Publisher가 되는거임)
    - java.util의 `Observer` 인터페이스가 있고, `Observable` 클래스 가 있음
    - Observable은 “observed”될 수 있음 객체
    - Observer 객체는 알림을 받는 쪽이니까 view쪽에서 하면 되겠다!

## View

![Screen Shot 2022-05-13 at 6.23.57 PM.png](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/Screen_Shot_2022-05-13_at_6.23.57_PM.png)

- ball의 상태에 대해서 접근할 수 있어야함
- ball을 그려줘야함
- View는 Observer입니다!
    - View는 본인이 판단 X
    - Model에 따라서 행동을 해야한다

## Controller

![Screen Shot 2022-05-13 at 7.09.11 PM.png](18%20MVC%206659f7f1a2cc4bb1a11a7b9f45aec215/Screen_Shot_2022-05-13_at_7.09.11_PM.png)

- Model에 대한 지시를 함
- View가 업데이트 돼야한다면 명령을 줄 수 있다

## 주의할 점

- view와 controller 의 구분이 둘 다 UI Layer이므로 어렵게 느껴질 수 있다.
- Non-UI Layer와 UI Layer는 확실하게 나눠줘야한다.
- Non-UI Layer가 UI Layer를 참조할 수 없어야한다.
    - 그래서 UI를 쉽게 바꿀 수 있어야한다.
- Model은 `JUnit` 같은 것으로 unit test를 쉽게 할 수 있다

### 실무에서의 Notes

- Litmus test
    
    : MVC가 잘 구성되었다면 Non-UI x→ UI
    
    ⇒ UI를 쉽게 바꿀 수 있는지 확인해본다.
    
- JUnit
    
    : unit test가 쉬운지 확인한다.
    
    만약 model이 UI를 거치지 않고 테스트하기 어렵다면 MVC가 책임 분리 안된 것
    
- Model은 빠르게 실행되어야 한다.
    
    model 실행 시간이 오래걸리면 GUI가 멈춘 것처럼 (frozen) 보이기 때문에
    
    ⇒ 멀티스레딩을 사용
    

## 정리

- MVC 패턴은 Observer, Strategy, Composite pattern으로 이루어져 있다.
    - Model: Observer pattern을 사용하면 쉽게 observer들과 decouple하게 유지하면서도 observer들에게 업데이트를 알려줄 수 있다.
- Important Decision
    - UI의 변화가 Non-UI module(Model)의 변화를 만들어내서는 안된다