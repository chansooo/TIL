# 9. Class Diagram-1

### Overview of UML Diagrams

![스크린샷 2022-04-08 오후 4.37.26.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.37.26.png)

- Class diagram은 구조를 표현하는 다이어그램
- **Object diagram** (**=인스턴스 다이어그램**)은 class diagram의 인스턴스화

## Object

---

![스크린샷 2022-04-08 오후 5.04.06.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.04.06.png)

- 밑줄 : object를 나타냄
- 익명 object 같은 경우 클래스 이름만 적을 수 있음

### Object Diagram

![스크린샷 2022-04-08 오후 5.08.45.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.08.45.png)

- = instance diagram
- 시스템의 object와 그들간의 관계를 나타냄
- 특정한 시간에서의 object들의 스냅샷
    - 다른 시점에서는 상태, 링크 등이 달라질 수 있음
- **Link :** object간의 관계

- OOP의 개념
    - Object는 클래스의 인스턴스이다.
    - 속성 (Attribute) : 클래스의 구조적인 특징
        - 값은 인스턴스마다 다를 수 있다.
    - Operation : 클래스의 behavior
        - 모든 인스턴스가 동일하다

## Class Diagram

---

### **Class** 표기법

![스크린샷 2022-04-08 오후 5.19.30.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.19.30.png)

- class name / attribute / operation
- **static** → 밑줄로 표시 (class variable, class operation)
- 접근 제한자
    - **`+`** : public, **`-`** : private, **`#`** : protected

### **Abstract** 표기법

- 개념
    - abstract operation은 body를 가지지 않는다. → 인스턴스로 못만듬
    - concrete operation은 body를 가짐 → 인스턴스화 가능
    - abstract operation 하나라도 포함하면 abstract class
- Abstract class 세가지 표현법
    
    ![스크린샷 2022-04-08 오후 5.27.36.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.27.36.png)
    
    - 이탤릭체 사용
    - `<<abstract>>` 스태래오타입 사용
    - `{abstract}` 프로퍼티 사용
- Abstract operation 표기법
    - 이탤릭체 사용
    - `{abstract}` 프로퍼티 사용

### Interface

- 개념
    - interface : public abstract operation을 하나 이상 가지는 명명된 콜렉션
- **Provided interface**
    
    ![스크린샷 2022-04-08 오후 5.35.07.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.35.07.png)
    
    - (다른 클래스나 컴포넌트로부터) 구현된다는 것을 의미
        - ball (lollipop) 심볼
        - **realization connector**
    
    ```java
    public interface Observer {
    	void update(Object arg);
    }
    class TimerObserver implements Observer {
    	void update(Object arg) {
    		// implementation
    	}
    }
    ```
    
- **Required interface**
    
    ![스크린샷 2022-04-08 오후 5.35.18.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.35.18.png)
    
    - (다른 클래스나 컴포넌트가) 인터페이스를 필요로 한다는 것을 의미
        - socket 심볼
        - dependency arrow → ball 심볼
        - dependency arrow → 스테레오타입 클래스
    
    ```java
    public interface Observer {
    	void update(Object arg);
    }
    class Timer {
    	// 이 부분은 위의 다이어그램으로부터는 알 수 없음
    	Observer o;
    	void someMethod() { o.update(this); }
    }
    class TimerObserver implements Observer {
    	void update(Object arg) { // implementation
    }
    ```
    

### 구현 레벨에 따른 다양한 클래스 다이어그램 형태

![스크린샷 2022-04-08 오후 6.27.02.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.27.02.png)

- 상황에 맞게 구현하도록

## 클래스 간의 관계 (relation)

---

![스크린샷 2022-04-08 오후 6.27.43.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.27.43.png)

- 상속 (**Inheritance**) : 어떤 클래스가 다른 클래스의 하나의 타입(종류)일 때 → 부모-자식 관계
- 구성 (**Composition**) : 어떤 클래스가 다른 클래스의 객체를 보관(소유)할 때, = 컨테인 관계
- 집합 (**Aggregation**) : 어떤 클래스가 다른 클래스의 객체를 갖고 있지만, 다른 클래스와 공유함 (소유가 아니라 **reference**를 가짐)
- 연관 (**Association**) : 어떤 클래스의 객체가 다른 클래스의 객체와 함께 일하며, 지속적으로 함께 사용될 때 (클래스간 default 관계)
- 의존 (**Dependency**) : 어떤 클래스의 객체가 다른 클래스의 객체가 함께 일하지만, 그 시간이 짧을 때 → 잠깐만 사용되는 경우 여기 해당

### Dependency

![스크린샷 2022-04-08 오후 6.38.07.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.38.07.png)

- 다른 클래스의 객체를 잠시동안 사용할 때 or 그 객체를 알아야 할 때
    - 파라미터나 매개변수의 타입으로 이용될 때
    - 내부에서 인스턴스를 생성하지만 오래 유지되지 않는 경우
- `<<use>>` 스테레오타입 : 경우에 따라 부가적인 의미를 부여하기 위해서 사용함

### Association

- object diagram에서 링크로 표현되던 것들이 class diagram에서는 다양한 방법으로 표시된다

### Binary Association

![스크린샷 2022-04-08 오후 6.45.59.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.45.59.png)

- 두 클래스 간의 연결
- Navigability : 해당 방향으로 찾을 수 있는지 (액세스할 수 있는지)
    
    ![스크린샷 2022-04-08 오후 8.44.46.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.44.46.png)
    
    - A → B : A는 B를 찾을 수 있다.
        - 정석 : 한 쪽 방향만 기입되어 있으면 나머지 undefined는 의도적으로 미표기된 상태
        - 실무 : x가 생략된 것으로 A -x→ B로 해석
    - B -x A : B는 A를 찾을 수 없다.
    - A - B : undefined;
        - UML 정석 : 의도적으로 정보를 미표기하는 상황
            - ex) 분석 단계에서는 navigation 방향까지 고려 x
        - 실무 (practice) : 관행적으로 양방향으로 해석한다.
- Association name : 보통 자연어로 표기
- Reading direction : association name을 이해(읽는)하는 방향
- Role : 자격(역할)을 표시함, Professor는 lecture의 자격으로 관계에 임한다
- Visibility : + / - / #
- Multiplicity : association에서 상응하는 인스턴스가 몇개 나올 수 있는가?
    - 교수는 여러 명의 학생에게 강의할 수 있다.
    - 학생은 여러 명의 교수에게 강의를 들을 수 있다.
    - 따라서 다대다 관계
- 예제
    
    ![스크린샷 2022-04-08 오후 6.41.30.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.41.30.png)
    

### Binary Association을 attribute에서 표현

![스크린샷 2022-04-08 오후 8.52.31.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.52.31.png)

- attribute에서 위와 같이 표현 가능
- 이를 코드로 변환하면 다음과 같음
    
    ```java
    class Professor { ... }
    
    class Student {
    	public Professor[] lecture;
    }
    ```
    
- 선호하는 것은 좌측의 표현 (같은 의미지만 해석하기 편하다)

### Binary Association의 Multiplicity과 Role

- multiplicity : 반대편 1개와 연관될 수 있는 객체의 개수
    
    ![스크린샷 2022-04-08 오후 8.57.47.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.57.47.png)
    
    - 하나의 assignment에 lecturer은 한 개 존재
    - 하나의 lecturer에 assignment는 여러개 존재
    
    ![스크린샷 2022-04-08 오후 8.57.40.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.57.40.png)
    
    - 한 명의 lecturer에 lecture은 1~INF
    - 한 lecture에 lecturer은 1~INF
- role : 객체가 association에 관련되는 방식 (역할)
    
    

### Unary Association

![예제) 시험감독 - 수험생](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.07.33.png)

예제) 시험감독 - 수험생

![예제) 남편 - 아내](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.16.41.png)

예제) 남편 - 아내

- 순회적으로 role에 맞게 연결되도록 함
- 한 클래스에 같은 이름의 role이 여러개 있으면 안된다.
- 고려사항
    
    ![스크린샷 2022-04-08 오후 9.20.37.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.20.37.png)
    
    - 위의 object diagram에서 자기 자신과 결혼한 상태
    - 하지만 class diagram만으로는 이에 대해 제약을 할 수 없다
    - 따라서 방지하기 위해서는 OCL이나 자연어를 통해 추가적인 제약조건이 필요함

### Association Class

association에 **attribute**를 추가하는 방법

![Enrollment (association class) → Student가 StudyProgram과 연관되어 링크가 생성될 때 수강시작 attribute를 기록](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.24.54.png)

Enrollment (association class) → Student가 StudyProgram과 연관되어 링크가 생성될 때 수강시작 attribute를 기록

- association 과정에서 링크가 만들어졌을 때 association class를 통해 attribute를 사용 가능
- **N:N 관계** - Association class가 반드시 필요한 경우
    
    ![예제) 임금과 직급 ](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.29.50.png)
    
    예제) 임금과 직급 
    
- 1:1, 1:N 관계 - Association class 필수 x
    
    ![스크린샷 2022-04-08 오후 9.39.26.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.39.26.png)
    
    - association class로 표현해도 되지만, N쪽에서 정보를 기입해서 표현가능

### Association Class vs. Regular Class

![스크린샷 2022-04-08 오후 9.43.49.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.43.49.png)

![스크린샷 2022-04-08 오후 9.46.20.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.46.20.png)

![스크린샷 2022-04-08 오후 9.46.10.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.46.10.png)

- 링크는 주어진 연결에 대해 1개만 생김 → association class도 1개만 허용됨
    - 하지만, `{non-unique}` 를 이용해 중복 링크를 허용할 수 있음
        
        ![스크린샷 2022-04-08 오후 9.50.00.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.50.00.png)
        
- 하지만 Binary association을 이용하면 여러 개의 연결이 허용되게 됨

### Qualified Association

![스크린샷 2022-04-08 오후 10.02.17.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.02.17.png)

- **Qualifier** : association에 포함된 attribute
    - N쪽의 객체들을 하나로 지칭하는 역할 → 모호함을 없앤다
    - qualifier를 가지고 (여러개 중에) 특정한 인스턴스를 지칭할 수 있다는 것을 명시하는 방법

### Aggregation

- 어떤 클래스가 다른 클래스의 일부 (part)라는 것을 나타냄
- Aggregation의 특성
    - Transitive : A → B & B → C 이면 A → C (삼단논법 같은거)
    - Asymmetric(비대칭) : A → B 면서 B → A는 불가능
- Shared Aggregation과 Composition으로 나뉨

### Shared Aggregation

![스크린샷 2022-04-08 오후 10.14.50.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.14.50.png)

- 파트가 약한 소유 관계를 가지고 있음 (독립성을 가짐)
    - ex) 컴퓨터 주변기기 : 각각 독립적이면서 서로를 공유하면서 전체를 구성
- aggregation의 끝 부분에서 multiplicity는 1보다 커도 된다. (여러 개의 whole 객체에 속해 있을 수 있다)
- acyclic graph (사이클이 없고 방향성이 있는 그래프를 구성하게 됨)

### Composition

![예제) 빌딩 ← 강의실 ← 프로젝터](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.24.22.png)

예제) 빌딩 ← 강의실 ← 프로젝터

- 강한 결합 - composite 객체가 없으면 부분 객체들도 소멸됨 (모두 종속)
    - ex) 나무
- composite 객체 쪽의 multiplicity는 최대 1
- 현실에 존재하는 물리적인 관계를 표현하는데 주로 사용됨

- 예제
    - 2 : 문법적으로는 맞지만, 차에 장착되지 않은 타이어 표현 불가
    - 3 : 문법적으로는 맞지만, 타이어가 여러 차에 포함될 수 있음
    - 4 : 타이어의 종류(ex 사계절용 겨울용) → 옳은 예제

![스크린샷 2022-04-08 오후 10.31.10.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.31.10.png)

### Generalization (= 상속)

![스크린샷 2022-04-08 오후 10.50.56.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.50.56.png)

- private 제외한 [attribute, operation, association, aggregation] 자식 클래스에 다 물려준다
- 하위 클래스의 인스턴스는 상위 클래스의 인스턴스로 인정된다.
- transitive하다 (A→B & B-C ⇒ A→C)
- Abstract class : 직접 인스턴스를 생성할 수 없다
- 다중 상속
    - 언어마다 지원여부는 다르지만, UML은 모두 지원하기 위해서 다중 상속을 지원

### Class Diagram 작성

- 가이드라인
    - 명사 → 클래스 이름 (후보)
    - 형용사 → attribute
    - 동사 → operation
- 실무에서의 예제
    
    ![스크린샷 2022-04-08 오후 11.02.27.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.02.27.png)
    
    ![스크린샷 2022-04-08 오후 11.05.58.png](9%20Class%20Diagram-1%206d0ab783f642476ea8e81a0884c7da60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.05.58.png)