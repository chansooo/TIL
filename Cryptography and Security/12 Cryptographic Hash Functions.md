# 12. Cryptographic Hash Functions

## Iterated Hash Fucntion

---

### Merkle-Damgard Scheme

![스크린샷 2022-06-13 16.58.36.png](12%20Cryptographic%20Hash%20Functions%206527eb256c59417eab69cb941d44319b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_16.58.36.png)

- 길이가 M(가변길이)인 메시지에 해당하는 digest (m 비트, 고정길이)를 만들고자 한다.
- n비트씩 나눠야 하므로, n의 배수가 아닌 M에 대해 padding을 삽입힌다.
- H1은 미리 정해진 초기값 H0 (m 비트)이 필요하다.
- f, n, m은 설계자가 결정한다.

![스크린샷 2022-06-13 17.07.50.png](12%20Cryptographic%20Hash%20Functions%206527eb256c59417eab69cb941d44319b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_17.07.50.png)

### 해시 함수의 종류

1. f 함수를 새로 개발 (made from scratch)
    - Message Digest(MD) : MD2, MD4, MD5 (Rivest 개발)
    - Secure Hash Algorithm (SHA) : SHA-1, SHA-512 (미국 표준)
2. 블록 암호에 기반하여 f함수를 만든다.
    - Whirlpool

### SHA-512

Merkle-Damgard scheme를 따라서 만들어졌으며, 미국 표준 (NSA, NIST)

![Message digest creation SHA-512](12%20Cryptographic%20Hash%20Functions%206527eb256c59417eab69cb941d44319b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_17.12.47.png)

Message digest creation SHA-512

- Compression function f 의 설계는 강의에서 다루지 않도록 한다.

### 분석

collision attack을 포함한 hash resistant를 잘 만족하는 512bit digest를 잘 생성한다.