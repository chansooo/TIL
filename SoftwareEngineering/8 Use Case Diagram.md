# 8. Use Case Diagram

### 다이어그램 관점에서 Use Case

---

### Use Case를 통해 알 수 있는 것

- System → 무엇에 대한 설명인가?
- Actors → 누가 시스템과 상호작용을 하는가?
- Use cases → actor가 시스템을 사용해서 무엇을 할 수 있는가?

### Use Case

- 시스템에 기대하는 기능을 기술
- 객관적인 이익(→ goal)이 있는지로 평가할 수 있다.
- 고객의 기대사항으로부터 유도됨

### Actor

- 시스템과 상호작용
    - use case를 사용
    - use case로부터 사용
        - 외부의 시스템에서 기능을 제공
- 유저의 역할 (role)을 표현함
    - 같은 사람이라도 상황에 따라 역할이 달라지기 때문에 다른 actor로
- Actor는 시스템 외부에 있음
- 스틱맨 또는 사각형으로 표현 가능. 외부 시스템도 actor가 될 수 있다.
    
    ![스크린샷 2022-04-05 오후 9.43.31.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.43.31.png)
    
- 시스템과 상호작용하는 Asistant(외부에서 구현됨)의 경우 구현 대상은 아니지만 다이어그램에는 들어갈 수 있음. 또한 외부 시스템도 같은 방식으로 (eg Email server)
    
    ![스크린샷 2022-04-05 오후 9.45.09.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.45.09.png)
    
    - 여러명일 경우 1..3 같은 식으로 표현
- actor는 적어도 하나의 use case와 연결되어야 한다.
- use case도 적어도 하나의 actor와 연결되어야 한다.

### Use case간의 Relationship

- **<<include>>**
    
    ![A : Base use case, B : Included use case](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.50.53.png)
    
    A : Base use case, B : Included use case
    
    - use case간에 동일한 behavior가 사용될 때 **Base use case →** **Included use case**로 포함시킨다.
    - base use case 스탭 실행 중 `include` 키워드가 나오면 included use case를 실행하고, 끝나면 다시 돌아온다.
        - A 스탭 중에 include B가 나오면 B를 실행하고, 끝나면 A 실행
    - included use case도 actor와 관계가 있다면 독립적으로 실행 가능
- **<<extend>>**
    
    ![A : Base use case, B : Extending use case](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.02.29.png)
    
    A : Base use case, B : Extending use case
    
    - Base use case에서 `<<extension point>>` 를 만나면 Extending use case를 실행, 끝나면 돌아옴
    - extension point는 인터럽트될 시점만 명시하며, extending use case에 대한 정보는 없음 (누구든지 extend해서 들어올 수 있다)
    - 상속 (자바의 extend)과는 관련 없음
- include vs. extend
    - UML의 화살표 방향은 누가 누구를 알고 있는가 (dependency)
        - include : A가 B의 정보를 갖고 있음
            - 따라서 B가 없으면 A는 동작 불가 (B는 사실상 A의 일부)
        - extend : B가 A의 정보를 갖고 있음
            - B없어도 A는 괜찮음
- **Generalization (일반화, 상속)**
    
    ![스크린샷 2022-04-05 오후 10.41.12.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.41.12.png)
    
    - Base use case (A)의 모든 기능을 Sub use case (B)가 상속 받는다.
    - `{ abstract }`
        
        ![스크린샷 2022-04-05 오후 10.45.51.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.45.51.png)
        
        - base use case에 라벨링할 수 있다.
        - 단독으로 실행 불가 (자식 use case는 실행 가능)
        

### Actor 간의 Relationship

- **Generalization (일반화, 상속)**
    
    ![스크린샷 2022-04-05 오후 10.50.06.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.50.06.png)
    
    - A가 B를 상속함
    - B가 할 수 있는 모든 use case를 A가 사용할 수 있다.

![AND](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.51.51.png)

AND

![OR](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.52.00.png)

OR

- 첫번째 사진의 경우 실제 뜻은 실행을 위해 Professor와 Assistant가 같이 필요함 (AND)
    - 하지만, 실무에서 관행적으로 OR을 나타내기 위해 이렇게 사용하기도 함 (잘못된 거지만 워낙 많은 사람들이 그렇게 씀..)
- OR을 의도한다면 두번째 사진처럼 작성해야함

### 자주 보이는 실수

- use case diagram의 의존성을 일의 처리 순서 (프로세스/워크플로우)로 생각하면 안된다.
- actor는 시스템 내부에 있으면 안된다.
- actor 관계에서의 OR과 AND의 혼동
- 작은 use case 단위는 그룹화 시켜야 한다
    - eg) CRUD를 분리하지 말고 Manage로 묶어버리자
    - 이것은 일반적인 가이드라인이고, 상황에 따라 독립시키는 것이 옳을 수 있다.
- use case에서는 기능적으로 분해하지 말 것
    - 요구사항을 명세하는 도구이므로 구현 단계에서 필요한 기능들을 고려하지 말 것
    

### 요약

![스크린샷 2022-04-05 오후 11.07.31.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.07.31.png)

![스크린샷 2022-04-05 오후 11.07.53.png](8%20Use%20Case%20Diagram%2082d5452d3ff14a7f9f0276d17f3d9c3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.07.53.png)