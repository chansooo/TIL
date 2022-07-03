# 22. Designing for visibility & etc.

# Visibility

- 어떤 object가 다른 object를 볼 수 있는 능력 or 레퍼런스를 가지고 있는 능력
- scope에 관련된 issue

- object A가 object B에 대한 visibility를 확보하는 단계
    1. Parameter Visibility
        - 클래스 B가 A의 메소드에 파라미터로 전달됨
        - A가 B를 활용 할 수 있는..
            
            → UML Dependency
            
    2. Local Visibility
        - B가 A의 local object로 선언되었을 경우
            
            → UML Dependency
            
            ![Screen Shot 2022-05-27 at 2.49.07 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_2.49.07_AM.png)
            
    
    ---
    
    1. Attribute Visibility(1, 2보다 훨씬 강력하게 연결되어있음)
        - A의 attribute로 B가 존재
        - A라는 클래스의 Definition에 B라는 클래스에 대한 참조가 있는 것임.
        
        → UML Association
        
        ![Screen Shot 2022-05-27 at 2.50.35 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_2.50.35_AM.png)
        
    2. Global Visibility
        - 모든 class에 Visible.
        - 모든 Class에서 볼 수 있음.
    
    밑으로 내려갈수록 넓은 범위임
    

- **Design단계에서 Visibility를 고려해야하는 이유**
    - interaction diagram을 그릴 때 object A가 object B로 메시지를 보내야할 때가 있다.
    - 이때는 Class A에서 Class B가 보여야만 가능!
    - 이때 B는 A에게 visible해야함

---

# Mapping Designs to code

design model → code

- Design 기간에 진행했던 interaction diagram, DCD(design level class diagram)등이 code generation의 input으로 쓰임

## Implementation Model

- implementation artifacts(산출물)
    - source code, database definitions, JSP/XML/HTML pages, 등등…
- 객체지향언어를 사용할 경우 code를 작성하기 위해서 필요한 것
    - class, interface의 정의
    - method 정의
- class definition → class diagram으로부터
- method body → interaction diagram으로부터

### Creating Class Definitions from DCDs

![Screen Shot 2022-05-27 at 3.09.04 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.09.04_AM.png)

- 밑의 class diagram을 통해서 코드를 작성
- description이라는 Role의 이름으로 ProductDescription이라는 Class에 대한 참조가 필요함을 알 수 있음.

### Creating Methods from Interaction Diagrams

![Screen Shot 2022-05-27 at 3.11.18 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.11.18_AM.png)

- interaction diagram을 보고 코드를 짤 수 있다….!

![Screen Shot 2022-05-27 at 3.13.11 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.13.11_AM.png)

## Collection Classes in Code

1:다 관계 코드로 어떻게?

- ArrayList(List)
- Map(HashMap)
- Array(Simple Array)

등을 사용해서 여러개를 구현할 수 있지롱~!

## Order of Implementation

- 가장 coupling이 없는 것부터 → coupling 많은 것으로 구현 순서를 가지자
- 그리고 그 순서로 Unit Test를 하자
    
    ![Screen Shot 2022-05-27 at 3.16.05 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.16.05_AM.png)
    
    - payment 클래스는 다른 클래스에게 요구를 안 하니까 제일 먼저!
    - 만든 애들에게 요구하는 애들을 그 다음으로…!

## Example

![Screen Shot 2022-05-27 at 3.17.44 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.17.44_AM.png)

![Screen Shot 2022-05-27 at 3.18.10 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.18.10_AM.png)

![Screen Shot 2022-05-27 at 3.18.55 AM.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/Screen_Shot_2022-05-27_at_3.18.55_AM.png)

![스크린샷 2022-05-29 17.53.03.png](22%20Designing%20for%20visibility%20&%20etc%20adf9e873734e40cbaf50abb638520b14/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-29_17.53.03.png)