# Sequence Diagram

## Interaction Diagram(상호작용 다이어그램)

- 객체들 간에 주고받는 메시지를 통해 상호관계를 명세하기 위한 것
- 구체적인 시나리오 모델링
- interaction Diagram에서 볼 수 있는 것
    - 시스템과 환경의 상호작용
    - 시스템 부분들 간의 상호작용
    - 특정 프로토콜을 준수해야하는 프로세스간 통신
    - class level의 통신
- 종류
    - 시퀀스 다이어그램
    - 통신 다이어그램

# Sequence Diagram

- 시스템의 논리를 실제로 명세화하는 것
- 2개의 축
    - Horizontal axis(수평): 컴포넌트들
    - Vertical axis(수직): 이벤트 순서
- Interaction: 상호작용의 순서

![Screen Shot 2022-04-11 at 4.32.17 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.32.17_PM.png)

## Interaction Partners

- Interaction Partners는 lifeline으로 표현됨
    
    ![Screen Shot 2022-04-11 at 4.41.55 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.41.55_PM.png)
    
- lifeline의 head
    - roleName: Class 식이 포함된 사각형
    - role은 object보다 더 general한 개념
    - object는 lifeline에서 다양한 role을 가질 수 있음
- lifeline의 몸통
    - Vertical, dashed line으로 표현
    - 연결된 object의 lifetime을 표현

### Lifeline의 표현법

![Screen Shot 2022-04-11 at 4.42.24 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.42.24_PM.png)

1. role 이름만 기입
2. class 이름만 기입
3. role, class 둘 다 기입
4. role의 많은 객체 표현

### 메시지 교환

![Screen Shot 2022-04-11 at 4.48.27 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.48.27_PM.png)

- Interaction: 이벤트의 연속
- Message는 보내는 event와 받는 event를 통해 정의됨
- Execution specification
    - 걸린 시간과 같은 것들을 잘 보이도록
    - continuous bar: 세로로 막대기처럼 생긴 애
    - 상대방이 어떤 행동을 실행할 때 시각화할 때 사용

![Screen Shot 2022-04-11 at 4.48.51 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.48.51_PM.png)

- lifeline을 공유하면 순서대로 봐야하지만 연관이 없다면 순서는 모르는 것

### Messages

- Synchronous message(동기)
    
    ![Screen Shot 2022-04-11 at 4.50.58 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.50.58_PM.png)
    
    - sender가 response받을 때까지 기다림
    - 화살표 꽉차있다
- Asynchronous message(비동기)
    
    ![Screen Shot 2022-04-11 at 4.52.47 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.52.47_PM.png)
    
    - sender가 response 기다리지 않음
    - 화살표 → 이케 돼있다
- Response message
    
    ![Screen Shot 2022-04-11 at 4.55.53 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.55.53_PM.png)
    
    - synchronous에서 response 수신까지
    - 명확하면 생략 가능
- Object creation
    
    ![Screen Shot 2022-04-11 at 4.56.57 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.56.57_PM.png)
    
    - 점선으로 연결하고 new 붙임
    - lifeline에서 나가서 대가리로 화살 쏨
- Object destruction
    
    ![Screen Shot 2022-04-11 at 4.57.33 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.57.33_PM.png)
    
    - object 삭제
    - x쳐버림
- Found message
    
    ![Screen Shot 2022-04-11 at 4.58.51 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.58.51_PM.png)
    
    - message의 sender를 모르거나 연관이 없음
- Lost meaage
    
    ![Screen Shot 2022-04-11 at 4.58.57 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_4.58.57_PM.png)
    
    - message의 receiver를 모르거나 연관이 없음
- Time-consuming message
    
    ![Screen Shot 2022-04-11 at 5.01.11 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.01.11_PM.png)
    
    - 시간의 경과를 보여주고싶을 때 사용
    - 보통 message는 시간 손실 없이 전송되는 것으로 가정하긴 함
    - 간격 사이에 경과된 시간 표시

![Screen Shot 2022-04-11 at 5.01.17 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.01.17_PM.png)

## Sequence Diagram Frames

![Screen Shot 2022-04-11 at 5.04.32 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.04.32_PM.png)

- Frame: 끄트머리에 5각형
    - sd 상호작용 ID
    - sdID는 간단한 이름이나 operation specification

### self

![Screen Shot 2022-04-11 at 5.05.34 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.05.34_PM.png)

- this와 같음
- 내가 가지고 있는거

### Local Attribute and Parameters

![Screen Shot 2022-04-11 at 5.07.17 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.07.17_PM.png)

- 모든 시퀀스 다이어그램은 frame가지고 있는 사각형~
- sd, sequence diagram이름, 매개변수(생략가능)

### Combined Fragments(한 곳에 모아)

- 다양한 컨트롤 구조 모델

![Screen Shot 2022-04-11 at 5.08.22 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.08.22_PM.png)

- operand(피연산자): 사각형
- operator(연산자): 12ㅐ 있음~

## combined fragments 종류

![Screen Shot 2022-04-11 at 5.10.12 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.10.12_PM.png)

![Screen Shot 2022-04-11 at 5.15.23 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.15.23_PM.png)

### Alt fragment

- switch와 비슷함
- [status == ok], 그 다음 operand에 [status == waiting]이런 식으로 씀

### opt fragment

- if와 비슷함. else는 없다

### loop fragment

![Screen Shot 2022-04-11 at 5.17.16 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.17.16_PM.png)

- 시퀀스가 반복적으로 수행
- 첫번째는 조건 안 보고 무조건 실행. 두번째부터 조건 봄
- * == 무한

### break fragment

![Screen Shot 2022-04-11 at 5.19.48 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.19.48_PM.png)

- 예외처리전용
- break 안에 있는 애는 조건 성립하면 실행
- break껍데기보다 하나 큰 애에 있는 애 실행 안함

![Screen Shot 2022-04-11 at 5.20.26 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.20.26_PM.png)

### seq fragment

- 디폴트 순서
- weak sequencing
- 몽말인지 알지?
    
    ![Screen Shot 2022-04-11 at 5.23.39 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.23.39_PM.png)
    
    ![Screen Shot 2022-04-11 at 5.23.53 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.23.53_PM.png)
    

### strict fragment

![Screen Shot 2022-04-11 at 5.25.54 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.25.54_PM.png)

- 서로 다른 operand 사이에서 이벤트 발생 순서가 중요
    - 위쪽의 operand 메시지는 항상 아재쪽보다 먼저 실행
        
        ![Screen Shot 2022-04-11 at 5.26.04 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.26.04_PM.png)
        
    

### par fragment

- 다른 operand 메시지 사이의 시간 순서를 파괴할거야
- seq는 기본적으로 따라야함

![Screen Shot 2022-04-11 at 5.27.43 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.27.43_PM.png)

![Screen Shot 2022-04-11 at 5.27.57 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.27.57_PM.png)

![Screen Shot 2022-04-11 at 5.28.59 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_5.28.59_PM.png)

### critical franment

- 박스 안에 있는 친구들 사이에 아무것도 못 와!
- critical 안에 다른 애 실행하다가 중단되지 않기 위해!

![Screen Shot 2022-04-11 at 7.25.57 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.25.57_PM.png)

![Screen Shot 2022-04-11 at 7.26.08 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.26.08_PM.png)

![Screen Shot 2022-04-11 at 7.27.02 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.27.02_PM.png)

### ignore fragment

![Screen Shot 2022-04-11 at 7.28.37 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.28.37_PM.png)

- 발생할 수 있지만 별로 중요하지 않아요
- ignore{status}여기 status 자리에 들어가는 애가 중요하지 않은 것

### consider fragment

![Screen Shot 2022-04-11 at 7.29.39 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.29.39_PM.png)

- 특별히 중요한 것들을 표시
- consider{여기} 여기에 들어가는 게 중요해요~

### assert fragment

![Screen Shot 2022-04-11 at 7.31.01 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.31.01_PM.png)

- 여기에서 박스 안에 말고 다른 애들이 일어나면 안돼!

### neg fragment

![Screen Shot 2022-04-11 at 7.38.41 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.38.41_PM.png)

- 자주 발생하는 오류를 명시적으로 강조를 표시하기 위해서
- 안에 있는 피연산자 일어나면 안돼

---

### Interaction Reference

![Screen Shot 2022-04-11 at 7.45.56 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.45.56_PM.png)

- 하나의 시퀀스 다이어그램을 다른 시퀀스 다이어그램에 통합

### Time Constraints

![Screen Shot 2022-04-11 at 7.50.00 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.50.00_PM.png)

- 유형
    - 한 시점
        - relative: after(5sec)
        - absolute: at(12.00)
    - 기간
        - {lower.. upper}
- predefined actions
    - now: 지금
    - duration: 메시지 전송 지속시간

### State Invariant(상태 변하지 x)

![Screen Shot 2022-04-11 at 7.57.51 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.57.51_PM.png)

- 특정 조건이 특정 시간에 충족되어야한다~
- 항상 특정 lifeline에 할당됨
- 뒤에 오는 이벤트 전에 확인돼야함
- state invariant가 true가 아니면 model이나 implementation이 이상한 것

![Screen Shot 2022-04-11 at 7.58.05 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_7.58.05_PM.png)

## 네가지 Interaction Diagram

- 같은 개념을 기반으로
- 모두 간단한 상호작용에 적용되지만 포커스가 다름

### Sequence diagram

![Screen Shot 2022-04-11 at 8.00.10 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.00.10_PM.png)

- 시간 순서에 집중

### Communication diagram

![Screen Shot 2022-04-11 at 8.01.16 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.01.16_PM.png)

- 관계에 집중(파트너와의 관계를 모델링)
- 누가 누구랑 소통하니?가 중요
- 숫자로 메시지 순서 나타냄

### Timing diagram

![Screen Shot 2022-04-11 at 8.04.04 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.04.04_PM.png)

![Screen Shot 2022-04-11 at 8.04.31 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.04.31_PM.png)

- sequence diagram의 변종
- 이벤트 발생으로 인한 상대의 상태 변경을 보여줌
- vertical: interaction partner
- horizontal: chronological order

### Interaction overview diagram

![Screen Shot 2022-04-11 at 8.05.24 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.05.24_PM.png)

- activity diagram + sequence diagram

![Screen Shot 2022-04-11 at 8.05.43 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.05.43_PM.png)

![Screen Shot 2022-04-11 at 8.05.49 PM.png](Sequence%20D%20e2d22/Screen_Shot_2022-04-11_at_8.05.49_PM.png)