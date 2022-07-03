# 9. Class Diagram-2

## 예제) 대학 정보 시스템

---

- 대학은 다양한 연구실(기관)으로 구성된 여러 학부로 구성됨. 각 연구실와 학부는 이름이 있다. 각 연구실에는 주소가 있다.
- 각 학부는 대학의 직원인 학장이 이끔
- 모든 직원 수를 알고 있고, 직원들은 SSN, 이름, 이메일이 있음. 직원은 연구직과 행정직으로 구분됨
- 연구직들은 최소 하나의 연구실에 배정됨. 그들은 전공이 있음. 연구자들은 프로젝트에 참여할 수 있으며, 프로젝트마다 투자해야 되는 시간, 이름, 시작 날짜, 종료 날짜가 포함된다. 연구자들 중 일부는 강의을 진행하며 이들을 강사라고 한다.
- 강의마다 id, 이름, 학점(weekly duration)이 있음

### Step 1: 클래스 추출

- 제외
    - 대학 : 대학의 내부 시스템이기 때문에, 필요없음
    - 학장 : 나눌 필요 없이 권한을 부여하는 방법으로
- 결과
    - Institute, Faculty, Employee, Research Associate, Administrative Employee, Course, Lecture

### Step2 : Attribute 추출

### Step2: Relationship 추출

- Employee의 상속 관계

![스크린샷 2022-04-08 오후 11.41.22.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.41.22.png)

- 학부는 1개 이상의 연구실로 구성되어 있다. 연구실는 하나의 학부에만 속할 수 있다.

![스크린샷 2022-04-08 오후 11.42.24.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.42.24.png)

- 각 학부에는 학장이 필요하다. 모든 직원이 학부를 필요하진 않다.
    - 하지만 일반 직원들에 대한 관계도 필요함 → workfor을 추가

![스크린샷 2022-04-08 오후 11.49.54.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.49.54.png)

- 연구직들은 연구실의 일부이지만, 독립적이다. 또한 여러 연구실에 속할 수 있다.

![스크린샷 2022-04-08 오후 11.51.08.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.51.08.png)

- 다대다 관계에서 프로젝트에 참여를 할 때마다 시간을 기록해야하므로 Association class로 표현

![스크린샷 2022-04-08 오후 11.56.34.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.56.34.png)

- 강사와 강의는 다대다 관계로 표현

![스크린샷 2022-04-08 오후 11.58.39.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.58.39.png)

- 최종 결과
    
    ![스크린샷 2022-04-08 오후 11.59.18.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_11.59.18.png)
    

## Code 생성

---

- Class diagram을 코드로 변환하는 과정은 기계적이다. (쉽다)

- 예제
    
    ![스크린샷 2022-04-09 오전 12.01.22.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.01.22.png)
    
    ```java
    public Course {
    	public int courseNo;
    }
    
    abstract class UniversityMember {
    	public String firstName;
    	public String lastName;
    	public int ssNo;
    }
    
    enum ESemester {
    	winter,
    	summer
    }
    
    enum ERole {
    	lecturer,
    	tutor,
    	examiner
    }
    
    class Student extends UniversityMember {
    	public int matNo;
    	public CourseExecution[] completedCourses;
    }
    
    class Employee extends UniversityMember {
    	private int acctNo;
    	public int getAccNo() {
    		return this.acctNo;
    	}
    	public CourseExecution[] courseExecution;
    }
    
    class CourseExecution {
    	public int year;
    	public ESemester semester;
    	public Student[] student;
    	public Course course;
    	public Hashtable support;
    		// key: employee
    		// value: (role, hours)
    }
    ```
    
    ### Summary
    
    ![스크린샷 2022-04-09 오전 12.17.04.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.17.04.png)
    
    ![스크린샷 2022-04-09 오전 12.17.12.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.17.12.png)
    
    ![스크린샷 2022-04-09 오전 12.17.27.png](9%20Class%20Diagram-2%20545d87c2f3414431adf96f66097ec2da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-09_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12.17.27.png)