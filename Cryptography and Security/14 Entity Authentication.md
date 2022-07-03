# 14. Entity Authentication

두 entity (개체)가 반대쪽 개체의 신원을 확인한다.

이 때 entity는 사람, 프로세스, 클라이언트, 서버 등 다양하다

- **claimant** : 신원을 인증 받고자 요청하는 개체
- **verifier** : 신원을 인증해준다.
    
    claimant의 비밀에 기반한다
    

## Message vs. Entity Authentication

---

- **메시지 인증** : 송신자가 MAC을 생성하고 나면 검증은 수신사자 수신 과정에서 진행한다. (ex MAC)
    - 메시지 1개마다 인증해야 한다.
- **개체 인증** : 송/수신자 모두 온라인으로 연결되어 동시에 인증에 참여하고, 인증된 이후에는 서로 통신할 수 있다.
    - entity 인증 이후에는 세션 (session)이 생성되는데, 세션동안에는 계속 claiment의 신원이 보장된다.

## Password

---

### Password - first approach (비밀번호 직접 저장)

![스크린샷 2022-06-13 20.09.59.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.09.59.png)

- verifier는 모든 password 정보를 가지고 있다.
- claimant가 요청하면서 전송한 passwrod를 비교하여 인증한다.

- Attacks (약점)
    - passwrod file이 도난될 경우
    - 만약 password를 암호화하여 저장하여도, key가 노출되면 끝이다.

### Password - second approach (비밀번호의 해시 저장)

![스크린샷 2022-06-13 20.12.13.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.12.13.png)

- 비밀번호를 직접 저장하는 것이 아닌, 비밀번호의 해시 (다이제스트)를 저장한다.
- 입력한 패스워드의 해시와 비교한다.

- **Dictionary Attack**
    
    ![스크린샷 2022-06-13 20.16.21.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.16.21.png)
    
    - 비밀번호 조합에 대해서 미리 (password-hash) dictionary를 만든다.
    - 훔친 password file에서 일치하는 해시 쌍이 있는지 확인한다.
        
        → ID, PW를 찾을 수 있다.
        

### Password - third approach (salting: 소금치기)

![스크린샷 2022-06-13 20.17.07.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.17.07.png)

- 각 password마다 랜덤 문자열 salt를 생성한다.
- password + salt를 붙인 단어에 대해서 해시를 생성하여 저장한다.
- 이 때 salt도 테이블에 함께 기록한다.

- 이점
    - 공격자가 dictionary를 만들 때 salt까지 함께 만들기는 어려워진다.
    - 사용자가 같은 password를 여러 곳에 사용해도 salt가 다르기 때문에 다이제스트가 다르다.

## Challenge Response

---

secret을 보내지 않고 비밀을 알고 있다는 것을 증명한다.

- challenge (질문) : 시간에 따라 변하는 값 verifier → claimant
- respone (응답) : challenge의 결과를 계산해서 응답한다.

### 대칭키를 이용한 일방향 인증

![스크린샷 2022-06-13 20.22.20.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.22.20.png)

- Secret : $K_{A-B}$
    
     = A와 B 사이의 비밀, DES 같은 대칭키로 이루어짐
    
- Nonce : 일회성 숫자

verifier는 K_(A-B)를 알고 있다.

→ claimant도 K_(A-B)를 알고 있는지 확인하고자 한다.

1. 일회성 숫자 R_B를 생성
2. verifier는 challenge로 R_B를 claimant에게 전송
3. claimant는 K_(A-B)로 R_B를 암호화하여 response한다.
4. verifier가 K_(A-B)로 복호화하여 비교한다.

이 때 R_B는 시간에 따라 매번 바뀌어야 한다. (replay 방지)

### 대칭키를 이용한 양방향 인증

![스크린샷 2022-06-13 20.27.39.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.27.39.png)

- claimant, verifier는 각각 R_A, R_B를 생성한다.
- 두 R의 순서를 바꾸어 서로 다른 암호문을 이용해 서로 인증한다.

### 공개키를 이용한 일방향 인증

![스크린샷 2022-06-13 20.33.50.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.33.50.png)

- challenge : claimant의 공개키로 암호화된 R
- response : claimant의 개인키로 복호화한 결과
    
    → verifier는 자신이 생성한 R이 맞는지 확인
    

> 약점이 많지만 이러한 프로토콜도 있다
> 

### 공개키를 이용한 양방향 인증

![스크린샷 2022-06-13 20.37.12.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.37.12.png)

1. claimant는 R_A를 생성
2. claimant는 verifier의 공개키로 암호화하여 전송
3. verifier는 수신된 메시지를 자신의 개인키로 복호화하여 R_A를 얻음
4. verifier는 R_B를 생성
5. verifier는 R_A+R_B를 claimant의 공개키로 암호화하여 전송
6. claimant는 수신된 메시지를 자신의 개인키로 복호화하여 R_A, R_B를 얻음
7. claimant는 R_A가 맞는지 확인
8. claimant는 R_B를 전송
9. verifier는 R_B가 맞는지 확인

## Zero-Knowledge (영지식)

---

- claimant는 secret을 노출시키지 않고 증명하고자 한다.
- 이 때 verifier는 secret에 대해 유추도 할 수 없어야 한다.

> 영지식에 관한 자세한 프로토콜은 다루지 않는다.
> 

### 동굴의 예시

![스크린샷 2022-06-13 20.43.10.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.43.10.png)

- 여러 차례 반복하기 때문에 우연히 Door를 통과하지 않고 나오는 확률이 매우 낮아진다.
- Bob은 비밀번호에 대해 전혀 얻은 지식 없이 Alice가 비밀번호를 알고 있다는 것을 검증할 수 있다.

### 색맹과 두 공 예시

![스크린샷 2022-06-13 20.46.26.png](14%20Entity%20Authentication%2038a135289cd34104a9929d96e121640e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-13_20.46.26.png)