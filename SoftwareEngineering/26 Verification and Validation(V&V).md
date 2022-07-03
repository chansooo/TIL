# 26. Verification and Validation(V&V)

- **verification**: 각각의 단계가 정확하게 실행되는지 검증
    - 올바르게 프로덕트를 만들고 있는냐
    - inspection과 review로
- **validation**: 우리가 만든 산출물들이 Requirement를 충족하고 있는가
    - 올바른 프로덕트를 만들고 있는가
    - 테스팅으로 보장이된다.

### Inspections

= static verification

- static한 시스템 표현에서 문제를 찾아냄
- system의 실행을 필요로 하기 않는다
    - 구현 전에도 가능
- 어떤 산출물에도 적용 가능
    - 코드 뿐만 아니라, 요구명세, 디자인, configuration data 등등
- 소프트웨어 퀄리티 높이는데 가장 좋은 방법
    - 테스트 전에 defect(결함)를 50-90프로 줄일 수 있었음
    - 테스팅보다 매우 경제적이다

### Program Testing

- 소프트웨어가 하는 일을 제대로 하고 있는지를 확인
- defect(결함)를 찾아내는 것
    - 있다는 것을 알아내는 것이지 없다는 것을 증명하는 것이 아님
- verification과 validation의 부분
    - Dynamic Technique라고 부름

### Inspection and Testing(이 둘은 서로 상호보완적인 관계다)

![Screen Shot 2022-06-12 at 10.26.12 AM.png](26%20Verification%20and%20Validation(V&V)%201d8e293f6c664a538678a5a3003df3e1/Screen_Shot_2022-06-12_at_10.26.12_AM.png)

- Test는 runnable한 코드 대상
- Inspection은 모든 부분이 대상이 될 수 있음
- inspection단점
    - customer가 원하는 system에 대한 requirement를 제대로 충족하는지 확인하기 어려움
        
        (시스템 로직 등등 점검 가능)
        
    - 코드를 구동해야만 알 수 있는 performance, usability를 알지 못함
- 하지만 값싸고 점검하기 유용하다!

### Stage of testing

- Development testing
    - 개발자들이 버그나 defect가 있는가를 확인하기 위해 테스팅
- Release tesing
    - 개발팀과 별도로 유저에게 배포 전에 잘 작동하는지 확인
- User testing
    - 실제로 이것이 사용될 유저의 환경에서 테스트
    - release testing과는 환경이 다른 점임

### Development Testing

- **Unit Testing**
    - 각각의 프로그램 유닛, 클래스를 테스트
    - functionality가 잘 작동 되는지
- **Component testing**
    - 위의 unit들을 모아서 진행
    - component의 interface를 테스트한다
- **System testing**
    - component를 모아서 전체를 테스트한다
    - interaction을 주로 확인한다.

## Black box testing VS White box testing

- Black box testing
    - 내부 구현 신경 안 씀
- White box testing
    - 내부를 알고 있다.
    - 프로그램 구조에 따라 테스트를 가능
    - 부가적인 테스트를 반들 수 있음

## Equivalence Partition

![Screen Shot 2022-06-12 at 10.35.47 AM.png](26%20Verification%20and%20Validation(V&V)%201d8e293f6c664a538678a5a3003df3e1/Screen_Shot_2022-06-12_at_10.35.47_AM.png)

- 모든 range를 test해보는 게 아니라 동등한 범위를 대표하는 애들 중 하나만 잡아서 test case를 만든다.
- 0~14까지 다 넣는거보다 훨씬 효율적이다.
    - ex) 위에서 0-5에서 하나만 6-10에서 하나만 11-14에서 하나만.
        
        & partition의 경계에서도 (0, 5,6 10, 11, 14)
        

⇒ 비슷한 동작이 예상되는 케이스는 대표하는 하나만

## 예시) Search routine specification

---

![Screen Shot 2022-06-12 at 11.00.38 AM.png](26%20Verification%20and%20Validation(V&V)%201d8e293f6c664a538678a5a3003df3e1/Screen_Shot_2022-06-12_at_11.00.38_AM.png)

- 우리는 자세한 구현 내용은 몰라요
- input specification과 output specification만 알고 있는 상황에서 equivalence partitioning을 통해 어떻게 테스트하는지 확인해보자

### Search routine - input partitions (Black Box Test)

- 두 종류의 equivalence partition
    - input이 실제 Array의 멤버인 경우
    - input이 Array에 들어있는 멤버가 아닌 경우
    
    - input sequence가 1개인경우
    - input sequence가 2개인경우
    
    ![Screen Shot 2022-06-12 at 11.03.53 AM.png](26%20Verification%20and%20Validation(V&V)%201d8e293f6c664a538678a5a3003df3e1/Screen_Shot_2022-06-12_at_11.03.53_AM.png)
    
    1개 이상이 시퀀스 안에 속한 parition에서는 처음, 중간, 마지막을 테스트해본다.
    
- 내부 구현을 모르는 상태에서 equivalence partition만을 이용해서 테스트 조합을 만들어 냈다 → Black box test

### Binary Search equiv. partitions (White Box Test)

만약 주어진 Search 루틴이 Binary search로 구현되어 있다는 것을 알고 있다면 → white box test

![Screen Shot 2022-06-12 at 11.09.03 AM.png](26%20Verification%20and%20Validation(V&V)%201d8e293f6c664a538678a5a3003df3e1/Screen_Shot_2022-06-12_at_11.09.03_AM.png)

- bin search의 특성(가운데 기준 왼/오 나눠서 진행)을 고려하여 equiv. partition을 나눈다.

![스크린샷 2022-06-14 01.43.04.png](26%20Verification%20and%20Validation(V&V)%201d8e293f6c664a538678a5a3003df3e1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_01.43.04.png)

## Stage of Testing

- White-box testing:
    - 개발자들이 주로 함(코드 내부를 알고 있으므로)
    - development testing
- Black-box testing:
    - 개발 팀 외부에서 주로 함(코드 로직, 내부를 모르므로)
    - Release testing, User testing