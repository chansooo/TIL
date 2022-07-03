# 21. Object Design Example with GRASP

# Use Case Realization(실현)

- 특정한 use case가 design model 상에서 어떻게 실행되는지
    - design model에 나타나는 여러 객체들이 협업하는 형태로 나타남
- 분석단계에서부터 할 수 있지만 설계 단계에서 할 거다

![Screen Shot 2022-05-27 at 1.31.42 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_1.31.42_AM.png)

- Controller에 의해 domain layer에 전달되는 system operation이 있다~
- UI Layer에서 Domain Layer에 전달되는 정보.
- UI Layer에서 시스템 Operation에 대한 것을 처리할 수 있는 Controller가 있다.
- 그러한 Controller에 전달되는 첫번째 message가 System Operation이 되는 경우가 많다!
    - Actor가 사용하는 Enter Item을 보면  자세한 Enter Item의 구현 사항은Design Model 안에 자세하게 나타나있음.
        
        ![Screen Shot 2022-05-27 at 1.35.44 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_1.35.44_AM.png)
        
        - 각각의 Domain Layer에 전달되는 System Operation을 어떻게 처리할까를 고민하면 된다.
        - Domain layer의 Interaction Diagram
            - use case에서 요구되는 요구를 달성하기 위해 어떻게 상호작용하는지를 보여준다.
            

## System Operation

![Screen Shot 2022-05-27 at 1.39.02 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_1.39.02_AM.png)

- SSD(system sequence diagram)에 나타나는 system operation들이 starting message 형태가 된다.
- Domain layer의 Controller object에 의해 전달됨

## Operation contract

- Operation이 어떻게 진행이 될까

![Screen Shot 2022-05-27 at 2.06.35 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_2.06.35_AM.png)

- Use case로부터 바로 이렇게 진행하는 경우도 있고, Operation Contract와 같은 더 자세히 분석된 것을 보고 진행할 수도 있다.

## Design Models related with enterItem

![Screen Shot 2022-05-27 at 2.10.02 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_2.10.02_AM.png)

- 빨간색으로 된 곳이 pattern을 적용한 곳
- 위의 diagram을 통해서 밑에 있는 domain model을 설계

## 첫번째 iteration을 끝내고 난 Design Class Diagram

![Screen Shot 2022-05-27 at 2.28.41 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_2.28.41_AM.png)

## Connecting UI and Domain Layers

![Screen Shot 2022-05-27 at 2.29.38 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_2.29.38_AM.png)

## System이 처음 시작했을 때 필요한 객체들을 만드는 상황

![Screen Shot 2022-05-27 at 2.30.28 AM.png](21%20Object%20Design%20Example%20with%20GRASP%202aa3929bfe30471093a41954d739d1c5/Screen_Shot_2022-05-27_at_2.30.28_AM.png)