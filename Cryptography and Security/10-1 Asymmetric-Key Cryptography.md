# Asymmetric-Key Cryptography

## 차이점

Symmetric-key cryptography: sharing secrecy( 공유 비밀)

→ 사전에 키 값을 서로 알고 있어야 함

Asymmetric-Key Cryptography(public-key cryptography): personal secrecy(개인 비밀)

→ 그런 거 없음

## Keys

public-key cryptography는 두 개의 키를 가짐

- one private key(개인 키)
- one public key(공개 키)

모든 user는 한 쌍의 키를 보유

### Encryption

- public key로 암호화
- public key: 잠그는 키
- 복호화를 하는 사람의 public key로 암호화를 진행함

### Decryption

- private key로 복호화
- private key: 여는 키

$C = f(K_{public}, P)$

$P = g(K_{private}, C)$

### General Idea

- key generating 할 때 두가지 키를 만듬
    - private는 나만 가지고 있음
    - public key는 모든 사람들에게 뿌림
- A에게 정보를 보내고 싶으면 A의 public key로 암호화를 해서 보내야함

## 대칭 키는 필요가 없는 걸까...?

아니다. 

Symmetric-key: 빠르고, 큰 메시지를 암호화하는데 좋다

Public key: 느리지만 인증, 디지털 서명, key exchange 하는데 좋다

예시

- 큰 파일 F를 K로 암호화하여 공용서버에 저장 (symmetric-key사용)
- F를 복호할 수 있는 K를 Public key로 암호화하여 필요한 사람에게 보냄

## Trapdoor One-Way Function

### One-Way Function

$y = f(x)$

1. f는 계산하기 쉽다.
    - x가 주어지면 y는 쉽게 구할 수 있음
2. $f^{-1}$은 계산하기 어렵다
    - y가 주어지면 x를 구하는 것이 매우 어렵다
    

### Trapdoor One-Way Function

- trapdoor가 있어서, 쉽게 y에서 x를 구하는 것이 쉽다

# RSA Cryptosystem

### 암호화

$C = P^e$ mod n (e, n: public key)

### 복호화

$P = C^d$ mod n (d: private key)

## 과정

1. `정보를 받으려고 하는 사람`이 세 개의 키를 만든다
    - e, n: public key
    - d: private key
2. `정보를 주려고 하는 사람`이 `정보를 받으려고 하는 사람`의 public key를 이용해서 암호화를 해서 넘겨준다
3. `정보를 받으려고 하는 사람`이 암호문을 받아서 자신의 private key로 복호화 

### RSA Key generation 과정

1. 두 소수 p, q 생성 (miller-rabin 사용)
2. n = p * q 계산
3. Euler pi(n) = (p-1)(q-1) 계산
    - p와 q가 서로소라면 Euler pi(n) = Euler pi(pq) = Euler pi(p) * Euler pi(q )
4. 1<e<Euler pi(n)이고 Euler pi(n)과 서로소인 n 찾기
5. $d=e^{-1}$ mod Euler pi(n) 계산

→ e, n: public key

d: private key

p, q, Euler pi(n)은 비밀 폐기해야한다

e와 d 모두 $Z^*_{Euler pi(n)}$에 속함

- 예시
    
    ![Screen Shot 2022-05-16 at 9.05.46 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_9.05.46_PM.png)
    

### Proof of RSA

$C = P^e$ mod n (e, n: public key)

$P = C^d$ mod n (d: private key)

![Screen Shot 2022-05-16 at 10.18.50 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_10.18.50_PM.png)

by Euler’s Theorem First Version

- If a and n are relatively prime, then $a^{eulerpi(n)}$mod n =1

Second Version

- If n = pq and a<n,
    
    ![Screen Shot 2022-05-16 at 10.24.18 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_10.24.18_PM.png)
    

### RSA 암호, 복호 구현

![Screen Shot 2022-05-16 at 10.25.59 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_10.25.59_PM.png)

![Screen Shot 2022-05-16 at 10.26.11 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_10.26.11_PM.png)

![Screen Shot 2022-05-16 at 10.26.22 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_10.26.22_PM.png)

### Trapdoor one-way function

- 어떤 사람이 암호문을 탈취함.
- e와 n은 public이므로 알고 있으므로 계산할 수 있음
- $C = P^e$ mod n
- 하지만 Trapdoor one-way function이라 암호화는 쉽지만,
- 복호화는 소인수 분해를 알아내는 것만큼 어렵다
- Easy to compute C from P
- Difficult to compute P, the e-th root of C from C
    - RSA problem, Factorization
- d를 알고 있으면 쉽게 복호화 가능

## Attacks on RSA

### Factorization attack

- n을 소인수분해하여 p와 q를 알아내는 것.
- n이 크기때문에 쉽지 않다
- RSA는 factorization이 어렵다! 에 기반함
- Factorization하는 아주 빠른 알고리즘이 나오지 않는 한 RSA는 유용하다~

RSA recommendation

1. n은 1024 bit 이상으로 해야한다
2. p와 q는 512bit 이상으로 해야한다
3. n은 공유되면 안된다
    - 다른 사람과 나의  n 값이 같다? 그럼 상대방의 d 값을 e를 통해 알아낼 수 있다
4. 선택하는 값인 e는 $2^{16}$+1로 하면 좋다. (소수이기 때문에 e와 eulerpi(n)은 서로소인지 체크하기 쉬움)
5. d(개인 키값)이 노출되면 n, e, d를 전부 다 바꿔야한다

## OAEP

- Optimal asymmetric encryption padding

지금가지 설명한 암호화의 문제점

- $C = P^e$ mod n
- P가 같으면 C도 같다 (deterministic)

### Encryption

1. m-bit message M(보내려는 메시지).
    - M이 m bit가 안되면 padding을 해서 m bit로 만든다
2. k bit짜리 random number r을 생성
3. G(r) 계산. G는 public one-way 함수
    - G()는 k bit를 m bit로 확대시켜줌
4. $P_1 = M \oplus G(r)$ 계산
    - $P_1$: masked message
5. $P_2 = H(P_1) \oplus r$
    - H: another public one-way 함수 (m bit를 k bit로 축소)
6. $C = (P_1||P_2)^e \bmod n$으로 암호
    
    ![Screen Shot 2022-05-16 at 10.52.29 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_10.52.29_PM.png)
    
    - $P_1$은 masked version of M
    - $P_2$는 r에 대한 정보를 보내기 위해 필요함
    - r은 매번 바꿔줘야함
    - r때문에 M이 같더라도 C는 달라짐
    

### Decryption

1. 복호 $P=C^d\bmod n = P_1||P_2$
    - P1과 P2를 얻을 수 있음
2. r을 얻기 위해 계산
    - $H(P_1) \oplus P_2 = H(P_1)\oplus H(P_1) \oplus r = r$

1. 마스크 벗김
    - $G(r) \oplus P_1 = G(r) \oplus M \oplus G(r) = M$
2. M에서 padding 없애면 원래 메시지

### 예시

512bit q와 p가 있음 (둘 다 소수)

![Screen Shot 2022-05-16 at 11.02.05 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_11.02.05_PM.png)

![Screen Shot 2022-05-16 at 11.02.14 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_11.02.14_PM.png)

![Screen Shot 2022-05-16 at 11.02.24 PM.png](Asymmetric-Key%20Cryptography%20909d5b228dd84d0497bc7b67e10db13a/Screen_Shot_2022-05-16_at_11.02.24_PM.png)

### 적용

RSA는 메시지가 길어지면 느려진다.

- $P^e \bmod n$을 계산하는 것이므로

RSA는 짧은 메시지에 유용하다

RSA는 디지털 서명같은 곳에서 유용하게 쓰인다