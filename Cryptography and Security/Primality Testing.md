# Primality Testing

- 큰 수가 주어졌을 때 이 수가 소수냐 아니냐를 판별하기 위한 방법

# Determinstic Algorithm

## Divisibility Algorithm

- n 이라는 수를 n^1/2까지 for문을 돌리며 나눠지는 수가 있는지 확인하는 것.
- 수행 시간
    - $n^{1/2}$ 즉, $2^{nb/2}$개의 나눗셈 필요
    - ex: n이 200비트다
        - $2^{100}$개의 정수 나눗셈 필요
        - 1초에 $2^{30}$개 나눗셈 가능한 컴퓨터로 거의 할 수 없는 수준

## Probabilistic Algorithm

- 앞과 동일하게 prime인지 composite인지 판별
- 소수가 주어지면 소수라고 말해줌
- 소수가 아니라면 아주 작은 확률로 잘못 알려줄 수도 있음

| 입력\판단 | prime | composite |
| --- | --- | --- |
| prime | 1 | 0 |
| composite | epsilon(아주작은확률) | 1-epsilon |
- error가 아주 작을 경우 유용하게 사용 가능
- prime이라 판단 → 아주 작은 확률을 빼고 맞음
- composite라 판단 → 100% 맞음

### 페르마의 소정리

- p가 소수이고, a가 정수일 때 p가 a로 나눠지지 않으면 $a^{p-1}$ mod p = 1
- n이 소수라면 $a^{n-1}$ mod n = 1 for all 1<a<n

→

역으로 probabilistic algorithm 나옴

### Fermat Test

2...n-2의 숫자에서 

$a^{n-1}$ mod n ≠ 1이라면 composite

맞다면, prime

→ composite라 판단하면 n은 확실히 composite

prime이라 판단하면 n은 `아마도` prime

### Square Root Test

![Screen Shot 2022-05-10 at 9.32.53 PM.png](Primality%20Testing%20901ae713613d452791eac0b5ebdb1204/Screen_Shot_2022-05-10_at_9.32.53_PM.png)

![Screen Shot 2022-05-10 at 9.33.42 PM.png](Primality%20Testing%20901ae713613d452791eac0b5ebdb1204/Screen_Shot_2022-05-10_at_9.33.42_PM.png)

- 항상
    - $1^{1/2}$ mod n = 1
    - $(n-1)^{1/2}$ mod n = 1이므로

→ n이 소수라면, $1^{1/2}$ mod n = { 1, n-1 }

## Miller-Rabin Test

- Fermat test + Square Root test
- Fermat test
    - $a^{n-1}$ mod n ≠ 1이라면 composite
- Square Root Test
    - $1^{1/2}$ mod n ≠ { 1, n-1 } 이라면, n은 composite

![Screen Shot 2022-05-10 at 9.49.55 PM.png](Primality%20Testing%20901ae713613d452791eac0b5ebdb1204/Screen_Shot_2022-05-10_at_9.49.55_PM.png)

- 방법
    - 랜덤한 정수 a 고르고
    - $a^{m}$ mod n을 계산.
    - 그 값이 1이나 n-1이면 prime (by fermat test(2))
    - 그 값을 T라고 하면 $T^2$ mod n의 값이 1이라면 composite (by Square Root Test)
    - n-1이라면 prime (by fermat test(1))
    - k-1번 반복
        
        ![Screen Shot 2022-05-10 at 10.06.17 PM.png](Primality%20Testing%20901ae713613d452791eac0b5ebdb1204/Screen_Shot_2022-05-10_at_10.06.17_PM.png)
        
    - 이거 a를 다르게 10번 돌리면 거의 맞음~
    

## Factorization

- 큰 정수 소인수분해 하는 것이 어려움
- 몰랑 ㅜ

## Fast Exponentiation

이게 무슨 소리일까~

![Screen Shot 2022-05-10 at 10.20.16 PM.png](Primality%20Testing%20901ae713613d452791eac0b5ebdb1204/Screen_Shot_2022-05-10_at_10.20.16_PM.png)

- x의 비트수만큼 하는거라고~
- x는 비트 작은 쪽부터 하나씩 넣는거라고~