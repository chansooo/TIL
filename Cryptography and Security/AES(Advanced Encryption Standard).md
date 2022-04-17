# AES(Advanced Encryption Standard)

![Screen Shot 2022-04-17 at 10.57.41 PM.png](AES(Advanc%20b2902/Screen_Shot_2022-04-17_at_10.57.41_PM.png)

- 블록 단위가 128bit
- key가 128, 192, 256 bit 가능
- 순서
    1. pre-round transformation
    2. M개의 라운드 수행
        1. SubBytes
        2. ShifgtRows
        3. MixColumns
        4. AddRoundKey(첫번째 라운드 전에 추가로 진행)

### SubBytes

- s-box를 통해 대응되는 값으로 plain text를 cipher text로 변환
    - 1byte(8bit)를 hex로 두자리로 표시하고, s-box에서 해당하는 값 찾아서 다시 1byte로 표시
    
    ![Screen Shot 2022-04-17 at 10.38.23 PM.png](AES(Advanc%20b2902/Screen_Shot_2022-04-17_at_10.38.23_PM.png)
    

### ShiftRow

- 0번째 row: shift 안함
- 1번째 row: left shift 1번
- 2번째 row: left shift 2번
- 3번째 row: left shift 3번

### Mixing

- state의 1개의 cloumn(1x4 matrix)씩 뽑아서 constant 행렬(4x4 matrix)과 곱한다.
- 그러면 새로운 column 하나 나옴!
    
    ![Screen Shot 2022-04-17 at 10.52.21 PM.png](AES(Advanc%20b2902/Screen_Shot_2022-04-17_at_10.52.21_PM.png)
    

### AddRoundKey

- 해당하는 자리의 Roundkey와 xor 처리 진행
    
    ![Screen Shot 2022-04-17 at 10.57.30 PM.png](AES(Advanc%20b2902/Screen_Shot_2022-04-17_at_10.57.30_PM.png)
    

## Key Expansion

- 각 라운드의 128bit짜리 키를 만들어주는 방법
- 라운드마다 4개의 word + pre round → 총 4(N+1)개의 word 생성

![스크린샷 2022-04-08 오후 2.24.45.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.24.45.png)

- pre-round
    - ciper key를 그대로 사용
- t에 이전 round의 word들을 XOR 진행

### **t(i)**를 구하는 법

w(i) (i mod 4 = 0)에 XOR해줄 것으로, 1word = 4byte이다

- **RotWord** : byte 단위로 left-shift 1번
- **SubWord** : 각 byte를 table에 의해 substitution 시킴 (S-box 존재)
- **RCon** : 라운드마다 constant 정의되어 있음
    
    ![스크린샷 2022-04-08 오후 2.31.51.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.31.51.png)
    
    - RC는 가장 왼쪽 byte에서 정의됨 (오른쪽 3byte는 all zero)
    
    ![스크린샷 2022-04-08 오후 2.52.44.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_2.52.44.png)
    
    - RC(i)에 대해서 2^(i-1)이라고 생각하면 됨 : 2^0~2^9
    - 이 때, 기약다항식(84310)으로 mod해주는 거임
    - 참고로 맨 위의 표는 hex 표시임
    

---

### 예제

- (24 75 A2 B3 34 75 56 88 31 E2 12 00 13 AA 54 87)16 에 대해서
    
    ![스크린샷 2022-04-08 오후 3.02.54.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.02.54.png)
    
    - 4word로 나눠서 pre-round로 들어가고 (w0~w3)
    - 왼 XOR 위로 새로운 word구함
    - 왼쪽 없으면 t를 사용 (i mod 4 = 0인경우)
- 각 라운드 키는 **nonlinear**한 dependency를 가진다.
- RC를 매번 다르게 넣어주는 이유 : 혹시라도 결과가 같을까봐 아예 상수를 더해줘버림
- key가 1byte만 달라도 round key는 뒤로 갈 수록 완전 달라짐
    
    ![스크린샷 2022-04-08 오후 3.07.48.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.07.48.png)
    
- 이러한 특성 때문에 DES에 있던 **weak key**가 없다
    
    ![스크린샷 2022-04-08 오후 3.08.36.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.08.36.png)
    

## Cipher

---

cipher는 암호화, inverse cipher는 복호화 알고리즘

### Cipher

state → SubByte → ShiftRow → MixColumn → AddRoundKey

### Inverse cipher

AddRoundKey는 자기 자신이 inverse이다.

![스크린샷 2022-04-17 오후 4.11.41.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.11.41.png)

- cipher의 마지막 라운드에는 MixCoulum이 없음
    
    ⇒ 복호화를 설계하기 쉽도록
    
- AES에는 (DES와 달리) 암호화하는 알고리즘과 복호화하는 알고리즘이 다름
    
    라운드 진행되는 알고리즘의 순서도 다르다!!!!!
    

- 복호화에서도 마지막 라운드 (round10)은 InvMixColumn이 없다

- Cipher ↔ InvCipher에서 내부적으로도 서로 역변환인 것들
    - AddRoundKey (kn) ↔ AddRoundKey(kn)
    - SubByte (Round n) ↔ InvSubByte (Round 10-n+1)
    - ShiftRow (Round n) ↔ IvShiftRow (Round 10-n+1)
    - MixColumn (Round n) ↔ InvMixColumn (Round 10-n)
        - 예시
            
            Cipher Round 9 ↔ InvCipher Round 1
            
            Cihper Round 8 ↔ InvCipher Round 2 ...
            
        

## Security

---

- 브루트 포스
    
    AES는 큰 key 사이즈 → DES보다 더 안전함
    
- 통계적 공격
    
    ciphertext를 분석하는 것으로부터 안전함
    
- Differential and Linear attack
    
    여기에 대해서도 AES를 깨는 방법은 나온 적이 없다
    

## 구현

---

1. S-box 테이블을 미리 저장해놓는 방법
2. 메모리 문제 등으로 테이블을 저장하기 어렵다면 프로그램 상에서 계산을 시켜도 됨
    
    ![스크린샷 2022-04-17 오후 4.31.43.png](AES(Advanc%20b2902/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-17_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.31.43.png)
    

- 장점
    - 복잡한 연산이 없어서 cost가 적다.
    - S-box도 작아서 메모리 용량을 크게 차지하지 않는다.