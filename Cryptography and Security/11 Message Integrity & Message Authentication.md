# 11. Message Integrity & Message Authentication

# Message Integrity(메시지 무결성)

예시

- 등본, 영수증, 각서, 유언장, 도장, 싸인, 지장

## Message and Message Digest

- 메시지를 hash function을 통해 message digest로 변환
    
    ![Screen Shot 2022-05-31 at 2.44.44 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_2.44.44_PM.png)
    

## Difference

- document and fingerprint
    - 종이 문서에 지장을 찍으면 물리적으로 하나다.
- message and message digest
    - 하나로 연결되어있지 않고 따로 존재
- message digest는 함부로 수정할 수 없어야함

## Checking Integrity

- 메시지에 대해 메시지 다이제스트를 계산해놓음
- 나중에 확인할 때 현재 message의 message digest를 만들어서 이전의 message digest와 비교해서
    - 같으면 → 바뀐 것 x
    - 다르면 → 바꼈다.
- 쓰는 이유?
    - 예전의 파일과 지금이 같은지 확인하기 위해서.

## Hash function이 갖춰야 할 조건

![Screen Shot 2022-05-31 at 2.51.09 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_2.51.09_PM.png)

- M: message
- h: hash function
- y: message digest

조건

1. M은 길이가 변할 수 있다.(가변길이)
2. y는 고정길이이다.
3. h함수를 돌리는 건 쉽다.
4. y를 알 때 M을 찾는 건 거의 불가능(Preimage resistance)
    
    ![Screen Shot 2022-05-31 at 4.18.30 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_4.18.30_PM.png)
    
5. M과 y가 주어졌을 떄 결과가 y로 똑같은 M`을 찾는 것은 어려움(second preimage resistance)
    
    ![Screen Shot 2022-05-31 at 4.19.40 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_4.19.40_PM.png)
    
    → 주어진 것 M, y. 찾는 것: M’
    
6. M과 M`이 다른데 같은 결과를 가지는 M과 M`을 찾는 것은 어려움 (Collision resistance)
    
    ![Screen Shot 2022-05-31 at 4.20.27 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_4.20.27_PM.png)
    
    →주어진 것 X, 찾는 것 M, M’
    

## Cryptographic Hash Function Criteria

![Screen Shot 2022-05-31 at 3.37.46 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_3.37.46_PM.png)

- M은 가변 길이, y는 고정 길이이므로!
- 그래서 역함수가 존재할 수 없다
- 어떤 M에 대해서 M`이 같은 y값을 가질 수도 있음
    
    → Collision
    

# Random Oracle Model

## Pigeonhole Principle

하나의 집에는 두마리가 들어가야한다

## Birthday Problem

확률이 절반 이상이라면 “likely”라고 함

![Screen Shot 2022-05-31 at 4.27.22 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_4.27.22_PM.png)

1. 생일이 00월 00일인 친구가 있을까? 몇명이 있어야 1/2 넘을까?
    - 확률이 절반 이상이 되어야 likely
2. 한 학생과 생일이 똑같은 학생이 있을까? 몇명이 있어야 1/2 넘을까?
3. 두명 이상이 생일이 같은 학생이 있을까? 몇명이 있어야 1/2 넘을까?
4. 두 학급에서 한쪽의 학생과 다른 쪽의 학생의 생일이 같은 경우가 있을까? 몇명이 있어야 1/2 넘을까?

### Prob.1

미리 정한 생일: y

학생의 생일이 y가 아닐 확률: 1 - (1/n)

k명의 학생의 생일이 y가 아닐 확률: (1 - (1/n))^k

k명 중 생일이 y인 학생이 있을 확률: 1-(1-1/n)^k

![Screen Shot 2022-05-31 at 4.52.55 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_4.52.55_PM.png)

- (테일러 급수)
- e^x 식에 -x를 넣으면 1-x와 e^-1이 근사
- 대입해버리고 log 씌우면
- k ≥ ln2 X N

→ k 가 253 이상이면 확률 1/2 이상

### Prob.3

![Screen Shot 2022-05-31 at 5.01.49 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_5.01.49_PM.png)

![Screen Shot 2022-05-31 at 5.03.26 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_5.03.26_PM.png)

- 23명만 있으면 생일이 같은 사람이 있는 확률이 있다…!

## Preimage Attack

![Screen Shot 2022-05-31 at 5.05.20 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_5.05.20_PM.png)

- 조건 4를 깨려고 공격 실행
- 메시지를 하나 만들고 hash돌려서 결과랑 같은지 계속 돌리는 겨
- 생일 문제와 같이
    
    k ≥ 0.69 X N = 0.69 X 2^n (n: y의 길이. 즉 digest의 길이)
    
    그러니까 지금은 n = 50
    
- 5번 조건도 이렇게 깰 수 있다.

## Collision Attack

![Screen Shot 2022-05-31 at 5.08.32 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_5.08.32_PM.png)

- 조건 6을 깨려고 공격 실행
- 1~k까지 돌리면서 message 만들어내고
- digest값 계산.
- 그 값이 이전의 1~k-1 사이에서 같은게 있는지 확인

- k ≥ 1.18 X N^1/2 = 1.18 X 2^n/2 ( n: y의 길이)
    
    성공할 가능성이 절반 넘음
    

⇒ N을 크게 만들어야 좋다.

### 예시

for문 1초에 2^30번 도는 pc하고 치면

Preimage Attack

- k는 0.69 X 2^64와 근사
- n = 64bit면 500년이 걸린다 (0.69 X 2^34)

→ 안전띠

Collision Attack

- k는 1.18 X 2^n/2와 근사
- 1.18 X 2^2 seconds

→ 안전하지 않아요

### 현실

MD5: 128 bits

SHA-1: 160 bits

SHA-512: 512 bits

# Message Authentication

- MDC: Modification Detection Code
- MAC: Message Authentication Code

## MDC

- 메시지의 integrity 보장
- A가 B에게 메시지 보내려 하는데 전송 중에 변하지 않았음을 보장하기 위해
- 메시지와 MDC를 함께 상대에게 보냄

![Screen Shot 2022-05-31 at 5.31.44 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_5.31.44_PM.png)

- 받은 아이도 message를 hash를 통해 MDC를 만들고 받은 아이와 비교.

→ MDC는 keyless hash function이다

## MAC

- 메시지의 integrity와 data origin authentication(누가 보냈냐)를 동시에 보장
- keyed hash functions라고도 불림

MDC와의 차이점

- A와 B 사이에 비밀값을 공유해야함
- secret key K: 사전에 A와 B가 공유

![Screen Shot 2022-05-31 at 5.39.42 PM.png](11%20Message%20Integrity%20&%20Message%20Authentication%202222574b26264ba9aaa4910a13bf2e9e/Screen_Shot_2022-05-31_at_5.39.42_PM.png)

- 공격자가 M을 고치더라도 키를 모르기 때문에 MAC을 만들 수 없음.
- Accept를 하면 M이 그대로 왔다(무결성)과 보낸 사람의 신원을 확인 가능(key를 공유했으니까)

### 종류

- HMAC
    - hash 함수 이용
- CMAC
    - CBCMAC(대칭키 암호를 이용)
- 전용 MAC 함수를 따로 개발