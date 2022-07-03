# 24. Unit Test

### Unit test가 필요한 이유

> 검증받지 않은 코드를 다시 사람들과 합치는 것은 위험하다
> 
- 분할정복(디바이드앤 Conquer) 전략
    - 시스템을 unit단위로 나눈다
    - unit들을 독립적으로 디버그한다.
        
        → 버그가 있는 곳을 찾기 쉬워진다.
        
    - 버그가 있다는 것을 찾기 위한 도구, 버그가 없다는 것을 증명하는 것은 아니다!

### Test suites

- ad hoc
    - 테스트를 자기가 하고싶을때 한다.
- test suite
    - 원할 때는 언제든지 테스트를 할 수 있다
    - 단점
        - 너무 많은 extra programming 필요
            - 사실이지만 test framework 사용해서 절감 가능
        - extra work 아닌가?
            - 연구들에 의하면 테스트 해야 debugging time 줄일 수 있고 전체 일하는 시간도 줄일 수 있음
        
        → 결코 낭비가 아니라 오히려 이득이다
        
    - 장점
        - 더 적은 bug
        - 유지보수, 수정하기에 편하다~

### Regression testing (회귀 테스트) → 대부분 자동화 되어서 한다.

- 테스트의 종류 중 하나임
    - 이미 기존에 개발되었던 software가 변화가 있을 때 여전히 잘 동작되는지
    - 변화가 있을 때 잘 동작되는지 확인해야함
    - 내가 고친 부분은 잘 동작하는데 내가 고친 부분때문에 다른 부분이 동작하지 않으면 어떡하지???
        
        → Regression testing 해라!
        
    - 매일 밤마다 돌려놓고 집에 가면 돌고 있다가 어떤 부분(커밋 히스토리)에서 버그가 나는지 메일로 쏴주는 형식도 있어요
    - test suite로 만들어서 자동으로 테스트된다.
- Regression test가 필요한 이유
    - stable하고, maintainable한 code base를 가지기 위해서
    

## Drivers and Stubs in Testing

![Screen Shot 2022-06-02 at 1.21.26 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_1.21.26_PM.png)

- **Unit under test** (UUT): 테스트 대상
    - module, function, class, object등이 대상이 될 수 있음
- **Driver** : unit의 테스트 케이스를 불러온다
- **Stub**: UUT가 의존성을 가지는 모듈
    - 개발이 덜 되었으면 대체할 수 있는 모듈을 만들어서 돌리기도 함
    - 대체할 수 있는 모듈 : Stub

## JUnit

---

- java unit test를 작성하기 위한 framework
    - test case
        - 테스트 케이스
    - test suite
        - 여러 test case들을 포함한 collection
- Unit test는 agile한 관점에서 매우 권장된다.
    - 모든 것을 미리 설계를 해두고 개발하는 것이 아니기 때문에!
    - agile에서는 코드의 수정이 많이 되기 때문에 그렇겠죠
    - 그러다 test에서 통과 못하면 바로바로 해결할 수 있겠죠

### JUnit의 역사

- kent Beck이 만들었다….
- 뭐시기 뭐시기 Unit으로 언어들 test할 수 있는 걸 만듬
- TDD의 근본 툴
- Java의 reflection이라는 걸 사용해서 만들었당
    - 클래스의 메타정보만을 이용해서 객체를 생성할 수 있는 기능
    

### 예시

```java
public final class IMath{
	public static int isqrt(int x){
		int guess = 1;
		while(guess*guess < x) {
			guess++;
		}
		return guess;
	}
}
```

위 코드를 테스트해보자!

- JUnit을 사용하지 않고 Test를 하는 경우
    
    ```java
    printTestResult(0);
    printTestResult(1);
    ...
    printTestResult(10);
    ```
    
    이런 식으로… 눈으로 봐야함…
    
    즐거운 작업이 아니겠죠?
    
- JUnit과 같은 test framework를 사용할 경우
    
    ```java
    public void testIsqrt(){
    	assertEquals(0, IMath.isqrt(0)); //23번째 line이라고 가정
    	assertEquals(1, IMath.isqrt(1));
    	assertEquals(1, IMath.isqrt(2));
    	assertEquals(2, IMath.isqrt(4));
    	assertEquals(2, IMath.isqrt(7));
    	assertEquals(3, IMath.isqrt(9));
    	assertEquals(10, IMath.isqrt(100));
    }
    
    ```
    
    - 첫번째 파라미터는 예상 output
    - 결과
        
        ![Screen Shot 2022-06-02 at 1.35.51 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_1.35.51_PM.png)
        
        - 이런 식으로 나옴.
        - expected, 실제 결과값이 나와요
    - test framework의 중요성~~!~!~!~!~!

![Screen Shot 2022-06-02 at 1.38.00 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_1.38.00_PM.png)

### JUnit method종류

![Screen Shot 2022-06-02 at 1.39.14 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_1.39.14_PM.png)

- 주의!!!! x.equals(y)가 많이 사용되는데… 이건 주소값을 비교하는 것이랍니다…
    - 그래서 overrride해서 사용해야된답니다?

### Key JUnit Notions

- **Tested Class**
- **Tested Method**
    - 테스트 당하는 method
- **Test Case**
    - 테스트하고싶은 메소드들을 검사해주는 독립적인 친구
- **Test Case Class**
    - test case를 여러개 담고 있는 친구
- **Test Case Method**
    - test case를 메소드로 구현한 것
- **Test Suite**
    - test case를 의미 있는 그룹으로 묶은 것.
    - Test case Class 안에 여러개 Test Suite가 있을 수 있다

### JUnit (Java Unit Testing Framework)의 사용법

- JUnit3 예시: Exception handling test
    
    ```java
    import junit.framework.*;
    
    public class TestFailure extends TestCase{ //Test Case Class
    	public void testSquareRootException(){ // Test Case Method
    		try{
    			SquareRoot.sqrt(-4); // Tested Class and Method
    			fail("Should raise an exception");
    		}
    		catch(Exception success) {...} //Assertion Statement
    	}
    }
    ```
    
- JUnit4 예시: `@Test` 어노테이션
    
    ```java
    import junit.framework.*;
    import org.junit.Test;
    
    public class TestAddition extends TestCase{ //Test Case Class
    	private int x= 1;
    	private int y = 2;
    	@Test public void squareRootTest1(){ // Test Case Method
    		int z = SquareRoot.sqrt(4); // Tested Class and Method
    		assertEquals(2, z);	
    	}
    }
    ```
    
    `@Test`  → 메소드의 prefix로 “test”를 사용하는 제약이 사라짐 (어노테이션만 붙이면 된다)
    

### Fixture

- test case들을 수행할 때마다 그 직전/직후에 뭔가 추가하고 싶을 때
- 종류
    
    ```java
    protected void setUp() {...} //초기화
    protected void tearDown() {...} //Clean Up
    ```
    
- 기본적으로 비어있으며 상속해서 사용하면 된다.

```java
protected void runTest(){
	setUp();
	<run the test>
	tearDown();
}
```

이렇게 된 것처럼 실행되는데,,,, setup, teardown을 호출을 안해도 자동으로 불려옴

JUnit4에서는 `@Before` `@After` 어노테이션을 통해 구현할 수 있다.

![Screen Shot 2022-06-02 at 2.21.51 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_2.21.51_PM.png)

![Screen Shot 2022-06-02 at 2.22.03 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_2.22.03_PM.png)

### Testing exception

![Screen Shot 2022-06-02 at 2.23.33 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_2.23.33_PM.png)

- testDivisionByZero에서 int n = 2/0; 에서 error가 나면 fail(”Divided by zero!”)가 출력이 안 돼야함.
- JUnit4에선, try-catch문 없이 어노테이션 내에서 `expected`를 사용해서 간소화 가능

## Test Method의 구조

---

- 결과를 return하는 형태로 만들지 않음
- test가 정확히 진행되면 test method는 뭘 더 하려고 하지 않음
- test가 실패하면 AssertionFailedError을 throw
- 그 에러를 JUnit framework가 받아서 프린트 조져준다

### 예시

![Screen Shot 2022-06-02 at 2.29.04 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_2.29.04_PM.png)

- 주의: test method 사이에 순서 없음.
- testSetX가 Y보다 앞에 있다고 먼저 실행되는 것이 아님
- 순서
    - setUp → testSetX → tearDown

## TDD(Test-first(driven) development)

---

> Agile에서 많이 사용된다.
> 

- 코드를 작성하기 전에 test code를 먼저 작성
    - 우리가 구현해야하는 요구들을 명확하게 할 수 있음
- test들은 프로그램들로써 작성됨
    - 자동으로 수행됨
    - 결과적으로 쉽게 체크할 수 있다

- 과정
    - test code를 만들고
    - test돌린다 → 당연히 implementation안 했으니 실패.
    - 그러면 그때 implementation
    - 그리고 다시 test 돌림

![Screen Shot 2022-06-02 at 2.47.02 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_2.47.02_PM.png)

- TDD를 하는 이유
    - Test를 implementation을 하고 나서 잘 하다가도 시간에 쫒기게 되면 안 하게 된다.
    - 그럴수록 예전의 test code와 implementation과의 괴리가 커지게 된다.
    
    → TDD는 test code를 먼저 작성함으로써 무조건 test를 할 수 있도록 강제한다
    

> 실무에선 너무 작은 단위의 클래스는 테스트 케이스를 만들지 않는다
> 

### Example: Counter class

```java
public class CounterTest{
    Counter counter1; //declare counter

    @Before
    void setUp(){
        counter1 = new Counter(); //initialize the Counter
    }

    @Test
    public void testIncrement(){
        assertTrue(counter1.increment() == 1);
        assertTrue(counter1.increment() == 2);
    }

    @Test
    public void testDecrement(){
        assertTrue(counter1.decrement() == -1);
    }
}
```

- 주의
    - testDecrement가 testIncrement보다 뒤에 실행되는 것이 아님
    - 각 Test 전에 setup 메소드가 호출돼서 초기화시켜야 원하는대로 작동할 수 있다

### Writing a JUnit test class

test class가 생성되기 전/후에 실행되도록 해줄 수 있다.

1. `setUpClass()` , `tearDownClass()` 오버라이딩
2. `@BeforeClass`, `@AfterClass` 어노테이션

## Unit Test 예시: Stack Class

---

테스트를 위해서 `toString()` 을 오버라이딩하였다

![Screen Shot 2022-06-02 at 4.02.06 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_4.02.06_PM.png)

![Screen Shot 2022-06-02 at 4.02.28 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_4.02.28_PM.png)

위처럼 하나의 테스트 케이스 안에 많은 테스트를 넣는 것은 지양한다.

오류 발생 시 발생 지점을 찾아야 하기 때문에

따라서 분리하여 수정하면 다음과 같다.(잘게 쪼개서 가는거야~)

![스크린샷 2022-06-14 00.51.35.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-14_00.51.35.png)

![Screen Shot 2022-06-02 at 4.09.04 PM.png](24%20Unit%20Test%2008ce10c2f15f4bc9bd1ca1eb5eb6bcfd/Screen_Shot_2022-06-02_at_4.09.04_PM.png)