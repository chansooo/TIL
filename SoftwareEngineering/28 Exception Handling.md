# 28. Exception Handling

Exception

- 특수한 이벤트.
- 일반적인 흐름을 방해하는 이벤트

Exception Object

- 해당하는 상황에 대한 정보를 가지고 있음
- 특정 타입이 있음
- 상태를 보관
- 객체를 볼 수 있다.
- throw할 수 있다.

## Exception Handling을 쓰면 좋은점

1. Regular code(정상적인 코드)와 에러 핸들링 코드를 분리할 수 있다.
    
    ![Screen Shot 2022-06-12 at 1.02.05 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_1.02.05_PM.png)
    
    우웩 메인 코드, 에러 핸들링 코드 어딨는지 보기 힘듬
    
    ![Screen Shot 2022-06-12 at 1.02.56 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_1.02.56_PM.png)
    
    편안
    
2. Manual Propagation of Error Code(exceptioin 번식시킴)
    
    ![Screen Shot 2022-06-12 at 1.09.23 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_1.09.23_PM.png)
    
    이렇게 말고
    
    ![Screen Shot 2022-06-12 at 1.09.56 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_1.09.56_PM.png)
    
    이렇게 하자
    
    에러 처리 안하면 살짝 멘탈 나감.
    
    성공할 때만 생각하지 말자 친구들아
    
    ![Screen Shot 2022-06-12 at 1.11.42 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_1.11.42_PM.png)
    
    사실 이렇게가 제일 좋음 ㅎㅎ
    
3. Error type을 구분하고 그루핑할 수 있다.
    
    상위 타입과 하위 타입을 적절하게 사용할 수 있다.
    

# Exception 에티켓!

(가장 중요한 부분)

에티켓을 지켜야한다

- 핸들하지 못하는 에러는 잡지 말아야한다
    - exception을 잡고 아무것도 안하면 그냥 사라져버림
    - 완전히 해결 못하면 re-throw해야함
- catch하고 ignore하지 마라
    - catch(Exception x){} 이렇게 하지 마라
    - 예외 중 가장 상위인 Exception이 잡히고 아무것도 안하면 다른 프로그래머들의 exception handle을 쓰지도 못함

## Exception-Handling Fundamentals

![Screen Shot 2022-06-12 at 2.14.08 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_2.14.08_PM.png)

- `try`
    - 모니터 하고 싶은 코드
    - exception발생하면 throw자동으로 됨
- `catch`
    - exception 잡아서 handle
- `finally`
    - (try를 나가기 전) 반드시(항상) 실행되어야 할 코드가 들어간다.
- `throw`
    - 던져져요
- `throws`

## Throwable Inheritance Hiearchy

![Screen Shot 2022-06-12 at 2.12.00 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_2.12.00_PM.png)

## Exception Types

- **Throwable**의 두 개의 subclass
    - **Exception**
        - user program이 잡아야하는 exceptional condition
        - 우리가 집중해야할 것
    - **Error**
        - user program이 잡아도 의미 있는 처리를 할 수 없는 것
        - Java runtime library를 만드는 수준이 아니라면 처리할 일이 없다 (application 수준에서는)
            
            ex) stack overflow
            

## Three kinds of Exception

1. Checked exception
    - error, runtimeException (및 이들의 subclass 들)을 제외한 모든 예외들
        - Throwable의 하위 클래스는 Exception, Error가 있는데 여기서 Error 클래스를 제외
        - 또한 Exception을 상속하는 RunTimeException도 제외
        - ex) FileNotException은 포함
    - 잘 만들어진 로직은 반드시 처리를 해줘야하는 예외들
2. Error
    - 에러 핸들을 따로 해줄 수가 없는 예외
    - catch, specify할 의무가 없음, 어차피 할 게 없거든
    - ex) java.io.IOError → 자바 내부에서의 오류
3. Runtime Exception
    - 어플리케이션 내부에서 일어나는 버그
    - 일반적인 checked exception과 달리 회복하기 어렵다.
    - 프로그램 버그, 로직 에러, api잘못 부름
        - ex)nullpointerexception
        - nullpointerexception이 떴는데 에러핸들을 한다? 힘듬
    - catch, specify할 의무가 없음, 어차피 할 게 없거든
    - RuntimeException과 그 하위 클래스
        - checked exception, error 둘 다 속하지 않는 것들

## Checked or Run-time Exception?

- client가 해결(회복)할 수 있다 → checked exception
- client가 해결(회복)할 수 없다 → unchecked exception

### Uncaught Exception

- main까지도 잡지 않는 exception
- java run-time system의 default handler가 잡는다
    - stack trace된다.

## Using try and catch

- 장점
    - 에러 고칠 수있음
    - 프로그램 꺼지는거 방지 가능
- 에러 걸리면 그 줄에서 바로 나감
- 예시
    
    ![Screen Shot 2022-06-12 at 2.27.16 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_2.27.16_PM.png)
    
    ![Screen Shot 2022-06-12 at 2.29.02 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_2.29.02_PM.png)
    
    catch해주면 exception 생명은 거기서 끝남. 그래서 이 예제에서는 for문은 무슨 일이 있어도 10번 돈다
    

### 여러 catch문

- 예시
    
    ![Screen Shot 2022-06-12 at 2.29.57 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_2.29.57_PM.png)
    
    순서대로 실행됨
    
    c[42]에서서 인덱스 아웃남 
    
- 주의
    - 순서대로 exception이 실행되고, 하나 실행하면 그 뒤에 것들을 시행 안 하기 때문에
        
        ⇒ superclass보다 subclass가 먼저 catch되어야 한다.
        
        - 그렇지 않다면 unreachable code → 요즘 자바 컴파일러에서는 오류를 알려준다.

## Nested try Statement

여기서 안 잡으면 더 큰 곳 까지 넘어가요~

### finally

- exception과 상관없이  무조건 실행되는 아이
- `return` 이 더 빨리 있어도 얘가 먼저 실행됨
    - 다른 부분으로 넘어가려고 할 때 그 전에 무조건 실행된다
        
        ![Screen Shot 2022-06-12 at 3.00.00 PM.png](28%20Exception%20Handling%2023e03a3394cf4de78ac0992393183c50/Screen_Shot_2022-06-12_at_3.00.00_PM.png)
        
        - 예제에서 각 케이스는 주석 처리된 //n 문장만 있을 때의 기준이다.
        - 2번 같은 경우에는 JVM을 죽이기 때문에 아무 것도 안된다.

### Throwable

`Throwable` 클래스 또는 subclass의 인스턴스를 만들어서 예외에 대해서 명시적으로 처리할 수 있다.

- try-catch 문에서 파라미터로 전달된 인스턴스
- `new` 키워드를 통해 생성자를 호출하여 인스턴스 생성

### throws

`throws` 키워드를 선언한 메소드는 해당 예외를 handle하지 않고 호출한 상위 메소드로 넘긴다.

- Error, RuntimeException은 할 필요가 없다.
- Checked Exception이다.