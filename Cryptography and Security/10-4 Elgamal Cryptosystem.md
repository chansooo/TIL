# 10-4. Elgamal Cryptosystem

## Discrete Logarithm Problem (이산 대수 문제)

---

### 도입

p가 소수일 때

$<Z_p^*,\times>$ 는 abelian group이다.
이 때 $g^xmod\space p$를 계산해본다.

![스크린샷 2022-05-24 16.10.21.png](10-4%20Elgamal%20Cryptosystem%20a81664c712b24bb3b04ccbebbac04b9b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_16.10.21.png)

**primitive root** : 만약 $g^xmod\space p$ (1≤x≤p-1)결과가 $Z_p^*$와 같다면 해당 group에 대해서 g가 primitive root이다.

g=3, g=5는 primitive root이다.

모든 $<Z_p^*,\times>$는 primitive root를 가지며, 이를 찾는 알고리즘은 없다. (직접 테스트해야 함)

→ random pick and test 필요

### Discrete Logarithm (이산 대수)

p : 소수

g : $<Z_p^*,\times>$ 의 primitive root 일 때

$$
y=g^xmod\space p
$$

를 계산하려고 한다.

- p, q, x가 주어진다면 → y는 계산하기 쉽다. (exponentiation)
- p, q, y가 주어진다면 → x는 계산하기 어렵다. (최적 알고리즘이 없다)
    
    ⇒ discrete logarithm problem (이산 대수 문제)
    
    → == factorization (소인수분해 정도의 어려움을 가진다)
    

## ElGamal 암호

---

이산 대수 문제가 어렵다는 것에 기반한다.

### ElGamal 암호 Procedure

![스크린샷 2022-05-24 16.23.50.png](10-4%20Elgamal%20Cryptosystem%20a81664c712b24bb3b04ccbebbac04b9b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_16.23.50.png)

### Key Generation

1. 소수 p를 선택 (생성)
2. e1 : $<Z_p^*,\times>$ 의 primitive root 선택
3. d : 1 ≤ d ≤ p-2 선택
4. e2 : $e_1^dmod\space p$ 

- 공개키 : e1, e2, p
- 개인키 : d

⇒ e1, e2, p를 알아도 d를 계산하는 것은 어렵다.

### Encryption (암호화)

M : 평문 (M < p)

1. r 랜덤 선택 (비밀키) : 1 ≤ r ≤ p-1
2. $C_1=e_1^rmod\space p$ 계산
3. $C_2=M\cdot e_2^r mod\space p$ 계산

⇒ (C1, C2)가 M의 암호문이 된다.

- 단점
    
    : 암호문의 크기가 커지는 message expansion이 일어난다.
    
- 장점
    
    : 무작위 r에 따라 암호문이 달라지는 랜덤 encryption
    
    r은 매번 다르게 사용해야 한다. (must be fresh)
    

### Decryption (복호화)

1. $M=C_2/C_1^d\space mod\space p$ 계산

- 증명
    
    ![스크린샷 2022-05-24 16.34.35.png](10-4%20Elgamal%20Cryptosystem%20a81664c712b24bb3b04ccbebbac04b9b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_16.34.35.png)
    
    RSA와 달리 특별한 정리가 필요하지 않다.
    

### 예제

![스크린샷 2022-05-24 16.36.13.png](10-4%20Elgamal%20Cryptosystem%20a81664c712b24bb3b04ccbebbac04b9b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_16.36.13.png)

1. Bob의 키 선택
    
    p = 11 (선택), e1 = 2 (선택), d = 3 (선택), e2 = 8 (계산)
    
2. Bob의 키 생성
    
    공개키 : (2, 8, 11), 개인키 : 3
    
3. Alice가 r을 선택 및 C1, C2를 계산
    
    r = 4
    
4. Alice가 M을 암호화
5. Bob은 (C1, C2) 암호문을 받아서 개인키로 복호화

### DLP (Discrete Logarithm Problem)

$e_2=e_1^d \space mod \space p$

- e1,  d → e2 : 쉽다
- e1, e2 → d : 어렵다

⇒ 일방향 함수