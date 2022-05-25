# Memory Management

### **메모리**

💡 시스템에서 메인 메모리는 두 개의 부분으로 구분되는데, 하나는 운영체제(상주 모니터, 커널)을 위한 곳이며, 다른 하나는 현재 수행 중인 프로그램을 위한 공간이다.

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/1.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/1.png)

- 멀티 프로그래밍 시스템에서는 유저 공간이 **여러개**로 나뉘게 된다.
    - 이와 같은 분할 작업은 OS에 의해 동적으로 이뤄지며, 이를 **메모리 관리**라 함.

## 7-1 메모리 관리 요구조건

### 메모리 관리 관련 주요 용어 3가지

1. Frame : 메인 메모리의 고정된 길이(fixed-length)의 블럭
2. Page : 보조기억장치(Second Mem)에 있는 데이터의 고정된 길이의 block.
한 페이지(Page)의 데이터는 한 프레임의 메인메모리(Frame of Main mem)에 임시로 복사 될 수 있다.
3. Segment : 보조기억장치에 있는 데이터의 가변길이(variable-length) 블록
    - Segmentation : 전체의 Segment가 메인메모리의 사용가능한 공간에 임시로 복사되는 것.
    - Combined Segmentation and paging : 메인메모리로 일일이 복사될 수 있는 페이지들로 세그먼트를 분할 가능.(세그먼트를 페이지로 분할!)

### 메모리 관리 요구조건 5가지

1. 재배치(Relocation)
2. 보호(Protection)
3. 공유(Sharing)
4. 논리적 구성(Logical Organization)
5. 물리적 구성(Physical Oraganization)

### **재배치 (Relocation)**

💡 멀티 프로그래밍 시스템에서 메인메모리는 여러 프로세스들에 의해 공유 됨.

💡 프로그래머들은 프로그램이 실행 될 때 다른 프로그램이 어떻게 실행되는지 알 수 없음.

💡 CPU 효율을 극대화 하기 위해 
`Swap-in` : Disk에서 메인메모리로 Swap-in 
`Swap-out` : 메인메모리에서 Disk(Swap 영역)로 Swap-out해야 함.

💡 Relocation : 프로세스가 Disk로 Swap-out됐다가 다음에 Swap-in 되었을 때, Swap-out되기 전 공간을 누가 쓰고 있다면 다른 공간으로 프로세스를 할당하는 것

💡Relocation하면 시스템 성능을 높일 수 있음!         

<Process Image>

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/2.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/2.png)

- 위 그림의 경우 프로세스가 주기억장치의 연속된 영역을 차지한다고 가정.
- OS는 PCB 정보, 실행 스택의 위치, 프로세스 실행을 시작하기 위한 프로그램 entry point 를 알아야 함
- 그러면 실제로 시스템에서 Data를 참조할 때 물리적인 메모리 주소가 어디인지, 현재 프로그램이 메인메모리 어디에 위치하고 있는지 어떻게 알까요~~?

## **보호 (protection)**

💡 프로세스는 다른 프로세스에 의해 우연이든 고의적이든 그 프로세스가 원하지 않는 간섭으로부터 보호되어야 한다.

- 만약 다른 프로세스의 메모리 영역을 읽거나 쓰기위해 Reference(참조)하기 위해서는 Permission을 받아야한다.
- 보호를 보장하기 위해 Compile time에 주소를 알아내는 것은 불가능
    - 대부분의 언어들은 실행 시간(Run time)에 동적으로 주소를 계산 하는것 을 허용함.(C 같은 경우 포인터나 배열의 인덱스 계산)
- 따라서 실행 시간(Run time)에 검사를 받아야 함.
- OS(즉, 소프트웨어)가 아니라 **프로세서(하드웨어)**에 의해서 이뤄 져야 함.
    - OS는 프로그램이 만들어내는 모든 메모리 참조를 예측할 수 없음.
    - 예측 가능하더라도 모든 프로그램을 검사하는 일은 엄청난 시간을 소비하게 됨.
- 소유 하지 않은 메모리에 접근 할 경우
    - 분할 오류 또는 스토리지 위반 예외(hw error)이 발생

## **공유 (Sharing)**

💡 보호 매커니즘은 주 기억 장치의 같은 부분을 접근하려는 여러개의 프로세스들을 **융통성 있게 허용**할 수 있어야 한다.

- 필수적인 보호 기능을 침해하지 않는 범위에서 제한된 접근을 통해 메모리의 일부분을 공유할 수 있도록 허용해야 함.

## **논리적인 구성**

💡 메인 메모리는 일련의 바이트나 워드로 이루어진 선형 또는 **1차원의 주소 공간**으로 구성되어 있음.

### **대부분의 프로그램들은 모듈이라는 단위로 구성.**

모듈로 구성된 프로그램과 데이터를 효과적으로 처리 한다면, 다음과 같은 이점이 있음.

1. 각 모듈의 작성과 컴파일은 독립적으로 이루어 질 수 있고, 서로 다른 모듈 간에 이루어지는 참조는 수행 시간에 시스템에 의해 해결 됨.
2. 비교적 적은 추가 비용(Overhead)으로 각 모듈 마다 서로 다른 보호 등급(읽기 전용, 수행 전용)을 수행할 수 있다.
3. 프로세스들 간에 모듈을 공유할 수 있는 기법을 제공할 수 있다. 모듈 레벨의 공유를 제공함으로써 얻을 수 있는 장점은 사용자가 문제를 보는 관점과 부합된다는 것이며, 그 결과 사용자가 자신이 원하는대로 공유를 명시 할 수 있다.

이러한 요구조건을 가장 쉽게 충족시키는 수단은 **Segmentation**이다.

## **물리적인 구성**

- 메인 메모리: 상대적으로 비싼 비용으로 빠른 접근을 제공. 휘발성
- 보조 기억장치: 메인 메모리보다 느리고, 가격이 저렴하며, 일반적으로 비휘발성, (ssd,hdd)
- 용량이 작은 메인메모리가 현재 사용 중인 프로그램과 데이터를 저장
- 용량이 큰 메인 메모리는 프로그램과 데이터를 장기간 저장하는데 사용.

---

# **Memory Partitioning**

💡 메모리 관리의 주된 작업은 CPU에 의해 실행될 프로세스를 메인메모리로 가져오는 것이다.

## **고정 분할**

시스템 생성 시 메인메모리가 이미 특정 크기로 고정된 파티션들로 분할되는 방식

### **균등 분할**

### **비균등 분할**

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/3.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/3.png)

### **균등 분할 문제점**

- 프로그램이 파티션보다 클 수 있는데, 이때 프로그램의 필요한 부분만 메인 메모리에 load 해야 함..
    - overlay를 사용하는 프로그램을 설계 해야함.
    - `*overlay**`: data or instruction 블록을 다른 블록으로 교체하는 프로그래밍 방법. ex) 임베디드 시스템
- 메인 메모리 이용이 매우 비효율적
    - 매우 작은 크기의 프로그램이라도 전체의 파티션을 차지
    
    → **내부 단편화,internal fragmentation**
    

### **배치 알고리즘**

**균등 분할**

- 사용 가능한 파티션이 존재하기만하면 프로세스는 해당 파티션으로 load.
- 모든 파티션이 비어있지 않다면 프로세스들 중 하나가 swap out 되어야 함.

**비균등 분할**

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/4.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/4.png)

- 각 파티션 마다 스케줄링 큐
    - 프로세스의 용량에 맞는 가장 작은 파티션을 할당.
    - 프로세스들이 항상 메모리 낭비(내부 단편화)를 최소화시키는 파티션에 적재됨.
    - But. 어느 시점에 12M보다 크고 16M 보다 작은 크기를 가진 프로세스가 없을 경우 16M 파티션은 방치 될 수 있음.
- 하나의 큐로 모든 프로세스를 처리
    - **메모리 load 시점**에 사용 가능한 파티션 중 프로세스를 load할 수 있는 **가장 작은 크기의 파티션 선택**

## **고정 분할 기법 단점**

- **시스템 생성시간**에 미리 정해진 **파티션의 수에 의해** 시스템 내에서 활성화된 **프로세스 들의 개수가 제한**됨.
- **시스템 생성시간**에 파티션의 사이즈가 정해지기 때문에 **크기가 작은 작업**들의 경우 파티션 공간을 **효율적으로 활용할 수 없음**.

---

## **동적 분할**

💡 앞서 말한 문제점을 해결하기 위해 동적 분할 기법이 개발되었음. 동적 분할에서 **파티션의 크기와 개수**는 **가변적**이다. 한 프로세스가 메모리에 적재 될 때, 정확히 요구된 크기만큼의 메모리만 할당 받음.

- 주기억 장치 64 MB를 사용하는 경우

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/5.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/5.png)

- a: OS만 메인 메모리에 적재되어 있음.
- **b,c,d**: 프로세스들은 운영체제가 끝난 곳으로부터 적재되기 시작해서 각 프로세스들에게 꼭 맞는 공간만을 차지
- 메모리 끝에 프로세스4에게 할당하기에는 너무 작음. (”hole” 처럼 남게 됨)
- **e**: 운영체제는 프로세스2를 swap out 하여 메모리 확보
- **f**: 프로세스4 가 적재되기 충분한 공간을 가짐. 프로세스 4 할당
- 프로세스 4는 프로세스 2보다 작기 때문에 또 하나의 작은 “hole”이 생성.
- **g**: 프로세스2가 ready 상태로 수행 가능한 시점이되면, OS는 프로세스 2를 위한 빈 공간이 없으므로 프로세스1을 Swap out 함.
- **h**: 프로세스2를 그자리에 할당

💡 위 예제에서 봤듯이 결국은 메인 메모리에 작은  **“hole”들이 다수 만들어짐**. 시간이 지날수록 메모리 단편화는 심해지고, 메모리 이융룔은 감소 이와 같이 모든 파티션 영역 이외의 메모리가 점차 사용할 수 없는 조각으로 변하는 현상을 **외부 단편화(External fragmentation)**라 함.

이러한 외부 단편화를 극복하는 방법으로 Compaction(메모리 집약)이 있음. 

- 메모리의 모든 빈 공간을 하나의 블록이 되도록 한다.
- h의 경우 Compaction 결과 16M 크기의 메모리 블록이 사용 가능하게 됨.
- Compaction은 시간이 많이 걸리며 CPU시간을 낭비하는 단점이 있음.
- 동적 재배치 기능이 필요

## **배치 알고리즘**

- best-fit: 요청된 크기와 가장 근접한 크기의 메모리 선택
- first-fit: 메모리 처음부터 검사해서 크기가 충분한 첫번째 메모리 블록
- Next-fit: 가장 최근에 배치되었던 메모리 위치에서 크기가 충분한 첫번째 사용 가능한 메모리 블록
- worst-fit: 사용가능한 메모리 중 가장 큰 것을 선택
    - 프로세스 load후 hole의 크기가 커지므로 외부 단편화를 줄일 수 있지 않을까 라는 생각에서 나온 알고리즘입니다.

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/6.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/6.png)

- 16MB의 할당 요구에 대한 그림
- worst fit은 next-fix과 같은 위치에 할당 됨.
- best-fix 알고리즘은 명칭과 달리 가장 안좋은 알고리즘.
    - 가장 작은 단편들을 만들기 때문
- 속도 측면에서는 first-fit 좋음.

# Buddy System

- UNIX kernel 메모리 할당에서 쓰임
- fixed + dynamic partitioning
- 두개를 섞으면 internal plagmentation을 줄이고 compaction도 덜해도 되지 않을까?
- 크기가 $2^U$인 single block을 먼저 만듬
- single block를 $2^K$개로 쪼갬. L≤ K≤ U

## Buddy System Algorithm

1. 들어갈 크기가 $2^U$보다 작고 $2^{U-1}$보다 크다면 전체 공간을 할당해줌
    1. 그렇지 않다면 $2^U$를 두개로 쪼개서(그러면 크기 $2^{U-1}$) 하나만 할당
2. 만약  $2^{U-2}$< s ≤ $2^{U-1}$라면 다시 한번 쪼갠다
3. 이렇게 맞을 때까지 계속 쪼개버린다
4. 다 쓰고 난 뒤 반납을 하게 되면 안 쓰고 있는 애들을 하나로 다시 묶어버린다
    1. 묶을 때는 같은 크기인 buddy끼리만 합친다
5. 이걸 계속 반복하면서 사용한다
    
    ![Screen Shot 2022-05-12 at 2.36.21 PM.png](Memory%20Management%2053f83fcd37ab4df49cf1f3ce25ed2344/Screen_Shot_2022-05-12_at_2.36.21_PM.png)
    

# Address Type

- swap out, swap in할 때마다 메모리 위치가 달라지는데 어떻게 주소를 잘 관리할까 고민을 하다가 나온 것이 세가지이다
    - Logical
        - 실질적으로 assign된 아이들과 상관없이 관리
        - 0~2^n까지 연속적으로 있는 것처럼 보이지만 main(physical) memory에는 연속적이지 않게 올라가있음
        - 그래서 memory access할 때 translation이 무조건 필요하다
    - Relative
        - logical한 녀석에서 얼만큼 떨어져있나?
        - 0~2^n 사이에서 linear하게 배치돼있으므로 100+x 이런 식으로 찾아갈 수 있다
    - Physical or Absolute
        - 실제 main memory에서의 주소
        

# Hardware Support for Relocation

![Screen Shot 2022-05-12 at 2.48.23 PM.png](Memory%20Management%2053f83fcd37ab4df49cf1f3ce25ed2344/Screen_Shot_2022-05-12_at_2.48.23_PM.png)

- relocation할 때는 소프트웨어적으로 처리하게 되면 overhead가 많이 발생하므로 하드웨어로 처리해줘야함
- base register
    - 프로그램의 시작 주소를 가리키고 있음
- bound register
    - 이 프로세스가 접근할 수 있는 메인 메모리의 마지막 지점을 가리키고 있음
    - 얘보다 큰 곳으로 접근하려고 하면 못하도록 막게 해준다
- relative address는 시작지점부터 얼마나 떨어진지를 확인해서 absolute한 메모리 주소로 갈 수 있도록 한다

# **Paging**

💡 앞선 연속 메모리 할당과 다르게 페이징은 프로세스가 사용하는 메모리 공간을 **일정 단위**로 잘게 나누어서 ****비연속적으로** 실제 메모리에 할당**하는 메모리 관리 기법.

- 프로세스의 메모리 공간을 얘기할 때는 **페이지**라 하고, 실제 메모리에서는 **프레임**이라고 한다.
- 일반적인 OS에서 페이지와 프레임의 크기는 보통 4KB가 됨.
- 이 기법에서는 외부단편화(external franmentation)로 인 메모리 낭비는 없고, 내부단편화 낭비만이 존재함.
    
    ![Screen Shot 2022-05-12 at 2.53.11 PM.png](Memory%20Management%2053f83fcd37ab4df49cf1f3ce25ed2344/Screen_Shot_2022-05-12_at_2.53.11_PM.png)
    
    - 내부단편화는 마지막 페이지에서만 나오게 됨

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/7.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/7.png)

- 프로세스가 실행될 때 연속된 위치의 메모리라고 이해한 상태로 각각의 페이지가 어디 있는지를 매번 빠르게 찾을 수 있어야 한다.
- page table:
    - 프레임의 위치를 가지고 있음
    - logical address(page number, offset) → physical address(frame number, offset)
    - 각각의 페이지가 실제 메모리 어떤 프레임에 저장이 되는가에 대한 매핑 정보를 담고 있는 자료구조
    - 프레임 번호를 담고 있는 배열이고, 그것의 인덱스는 페이지 번호를 가리키고 있음.
    - fixed partitioning과의 차이점
        - partition size가 매우 작음
        - 하나 이상의 partition에 함께 적재할 수 있음
    
- 페이지에서 연속적이어도 frame(실제 메모리)에서는 연속적일 필요가 없으므로 main memory의 frame은 꽉꽉 채워서 담을 수 있다
    
    ![Screen Shot 2022-05-12 at 2.57.36 PM.png](Memory%20Management%2053f83fcd37ab4df49cf1f3ce25ed2344/Screen_Shot_2022-05-12_at_2.57.36_PM.png)
    

## **주소 표현**

💡 페이징 기법에서는 실제 어떤 변수(논리 주소)가 저장되어 있는 어떤 바이트 주소(물리 주소)를 알아야 되긴 하지만, 이것들이 페이지 단위의 묶음으로 배치가 되기 때문에 먼저 페이지가 어디에 있는지를 알아내야 함. 다음에 그 안에서의 변수의 실제 바이트 위치는 페이지 안에서의 위치가 일치하게 된다.

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/8.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/8.png)

- 논리 주소 공간의 크기 2^m이고, 페이지의 크기가 2^n 일 때
- 상위 (m-n) 비트는 페이지 번호, 하위 n 비트는 offset이다.
    - page 내의 모든 바이트를 가리켜야하므로 offset 기준으로 page number를 구하는 것 같음.

## logical → physical 방법

- 16bit logical address에서 6비트는 page number로 쓰이게 되고, 뒤의 10비트는 offset으로 쓰이게 됨
- page number를 가리키는 수에 맞는 process page table의 값을 앞에 붙이고, offset을 뒤에 붙이면 physical address가 나오게 된다
    
    ![Screen Shot 2022-05-12 at 3.27.48 PM.png](Memory%20Management%2053f83fcd37ab4df49cf1f3ce25ed2344/Screen_Shot_2022-05-12_at_3.27.48_PM.png)
    

- Question
    
    ### **Q1) 페이지 크기가 4KB이고, 메모리 크기가 256KB인 메모리 페이징 시스템이 있다.**
    
    - **페이지 프레임 수는? (페이지 번호 수 (상위 m-n비트) )**
        
        페이지 크기가 4KB라 하였다. 메모리 관련 개념에서는 사용되는 단위가 바이트(Byte)이므로 바이트로 우선 변환해보자.
        
        그럼 페이지는 2^12 Byte임을 알 수 있다. 마찬가지로 메모리를 보면 메모리는 2^18 Byte가 됨을 알 수 있다.
        
        페이지 프레임 수는 메모리 전체 크기에 페이지가 최대 몇개 들어 갈 수 있는지 묻고 있는것이다.
        
        따라서 2^18/2^12 = 2^6 = **64개**이다.
        
    - **이 메모리 주소를 해결하는 데 필요한 비트 수는?**
        
        메모리 주소를 모두 표현하기 위해서는 메모리가 2^18임을 알고있고 결국 비트가 18개가 있어야 2^18을 만족 할 수 있게 된다. 따라서 **18비트**가 있으면 된다.
        
        ![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/9.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/9.png)
        
    - **페이지 번호에 사용하는 비트와 페이지 오프셋에 사용하는 비트는?**
        
        페이지는 모두 64개가 있다 했다. 따라서 그림은 다음과 같아진다.
        
        [https://camo.githubusercontent.com/3b421a766e83a270183da0f8032646b0a7bf5da063e884fa11d0f18d77cff41a/68747470733a2f2f626c6f672e6b616b616f63646e2e6e65742f646e2f324e7470532f627471414a753637486d432f53485a686c6367783747354d6b426c4b316b3767694b2f696d672e706e67](https://camo.githubusercontent.com/3b421a766e83a270183da0f8032646b0a7bf5da063e884fa11d0f18d77cff41a/68747470733a2f2f626c6f672e6b616b616f63646e2e6e65742f646e2f324e7470532f627471414a753637486d432f53485a686c6367783747354d6b426c4b316b3767694b2f696d672e706e67)
        
        [https://camo.githubusercontent.com/bf80c22025c1ca339d404c0f17f08681f650116dcc209e76ecfaed227b2841b4/68747470733a2f2f626c6f672e6b616b616f63646e2e6e65742f646e2f62587267776e2f627471414b33414b6531532f7241317a49414e36566750476a5a7554726e6d73706b2f696d672e706e67](https://camo.githubusercontent.com/bf80c22025c1ca339d404c0f17f08681f650116dcc209e76ecfaed227b2841b4/68747470733a2f2f626c6f672e6b616b616f63646e2e6e65742f646e2f62587267776e2f627471414b33414b6531532f7241317a49414e36566750476a5a7554726e6d73706b2f696d672e706e67)
        
        페이징을 적용한 프로세스의 논리 주소
        
        위의 그림에서 보면 알 수 있듯이 **페이지 번호**를 모두 나타내기 위해서는 **6개의 비트**가 있어야
        
        페이지 번호 0번(000000)부터 페이지 번호 63번(111111)까지 모두 나타 낼 수 있다.
        
        그리고 **오프셋에 사용되는 비트 수**는 한 페이지의 모든 위치를 다 나타내기 위해
        
        k번 페이지의 0번(0000 0000 0000)부터 2^12-1번(1111 1111 1111)까지 나타내는 총 **12비트**가 필요하다.
        

## **페이징 (paging) 하드웨어 - MMU**

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/10.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/10.png)

- 논리적인 주소를 실제 물리적 주소로 매핑시켜줌.
- 이러한 페이징 작업을 하는 주체는 OS인 소프트웨어가 아닌 MMU 라는 하드웨어이다.

### **특징**

- 사용자 / 프로세스의 편의성
- 프레임 단위의 비연속적 메모리 할당
    - 동적 메모리 할당에 따른 문제가 없음 → **외부 단편화가 없음.**
- 내부 단편화
    - 페이지 크기보다 작은 메모리를 요청하는 경우에 발생
    - 페이지 크기가 작으면 내부 단편화는 감소 → 페이지 테이블 크기가 증가
    
    ex) 4KB인데 10KB의 메모리를 필요 하는 프로세스는 이 페이지가 2개하고 2KB 할당 되는것이 아니라 3개가 할당되어야 하지만 10KB메모리를 쓸 수 있음.
    
- 프레임 테이블
    - **프로세스마다** **각각 다른 페이지 테이블**을 가지고 있음.
    - OS 실제로 **어떤 프레임이 어떤 프로세스의 어떤 메모리에 할당되어 있는가**라는 정보를 쉽게 찾아내기 위해 프레임 테이블을 관리하고 있음.
    
    → 어떤 프로세스의 어떤 프레임이 사용되고, 사용되지 않냐를 빠르게 확인할 수 있음. (**메모리 보호**)
    
- 페이징과 문맥 교환
    - 페이지 테이블을 재설정하기 위해 **Context switching 시간이 증가**.
- 공유 페이지
    - process 마다 다른 논리 메모리 공간을 가지지만, **다른 프로세들의 특정 페이지**가 **실제 물리 메모리의 같은 프레임**을 가리킨다면 동일한 메모리를 사용할 수 있음.
    - 메모리 침범 제약만 관리한다면 프로세스 내에 메모리 공유를 간단하게 할 수 있음. **(메모리 공유)**

## **페이지 테이블 구현**

- 페이지 테이블도 메모리에 저장
    - 기준 레지스터 (Page-Table Base Register, PTBR) 에 메모리에 저장된 페이지 테이블의 시작 주소를 저장함.
        
        → Context switching 시 페이지 테이블 교체 비용이 적음.
        
- 메모리 접근 시간 문제
    - 페이지 테이블 접근 → 논리 주소를 물리주소로 변환 → 실제 메모리에 접근, 2번 메모리 접근하는 상황이 발생
        - 이는 페이징 성능 상에 중요한 문제임!

### **TLB (Translation Look aside Buffer)**

💡 이러한 문제를 해결하기 위한 방법으로 페이지 테이블을 위한 소형의 **하드웨어 캐시**가 있다. 어쨌든 두 번 메모리에 접근해야 하지만 페이지 테이블을 읽는 속도 만큼은 매우 빠르게 한다는 것이 페이지 테이블 캐싱의 원리이다. 그리고 MMU는 이런 페이지 테이블 캐싱을 위해서 컴퓨터 시스템에 있는 캐시 메모리를 그대로 이용하는 것이 아니고 MMU내에 TLB라고 하는 페이지 테이블만 전용으로 캐싱하는 별도의 캐시 메모리를 두게 된다.

**특징**

- 접근 속도가 매우 빠른 연관 메모리 (associative memory)
- TLB 내의 각 항목(entry)은 키(key)와 값(value)로 구성되며 key는 페이지 번호, value는 페이지 번호에 해당하는 프레임 번호이다.
- 메모리 참조 시간은 주소 변환을 하지 않는 경우보다 10% 이하의 시간만 더 소요된다.

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/11.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/11.png)

- TLB는 페이지 테이블 전체 중 일부만 담고 있기 때문에, 페이지 번호가 TLB안에 있는 검색
- 있다면 (TLB hit): 프레임 번호를 얻어내어 물리 주소를 형성
- 없다면 (TLB miss): page table도 참조해야 함.

# **Segmentation**

메모리에 연속적으로 할당하는 것이 아니라 여러개의 작은 단위로 쪼갠 다음 메모리 할당하는 방법. 분할하는 단위가 프로그램의 **논리적인 단위**에 따라 프로세스의 메모리 공간을 구분함.

- logical address 구성
    - segment number
    - offset
- method, procedure, function, object, variables, stack 등 함수 단위로 나눌 수 있음
    - C 컴파일러 관점에서는 코드, 전역 변수, 힙 ,스택, 표준 c 라이브러리 단위로 구분지어 나눌 수 있음.

### **Segment Table**

- 세그먼트 실제 메모리 위치, 크기 (끝 주소) 정보를포함.
    - 세그먼트가 차지하는 크기가 다르기 때문

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/12.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/12.png)

- 논리 주소는 세그먼트 번호와 세그먼트 안에 오프셋으로 구성 되어 있음.

### **Segmentation HW**

![https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/13.png](https://github.com/Nabaang/Nabaang-CS-School/raw/6aff2b76ac912f909042535861e41bb677b39579/OperatingSystem/MemoryManagement/13.png)

- 세그먼트 테이블에서 시작 주소와 그 세그먼트 테이블의 끝 주소를 읽어 내고, 오프셋 위치를 비교
    - 끝 주소 보다 클 경우 세그먼트 침범 에러가 발생함.
    - ex) c에서 허용되지 않은 메모리에 접근할 경우 Segmentation fault 에러 발생