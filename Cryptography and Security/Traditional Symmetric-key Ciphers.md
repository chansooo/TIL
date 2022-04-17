# Traditional Symmetric-key Ciphers

- P(lain text)에 공유하는 secret key로 Encryption algorithm을 사용하여 암호화와 복호화 진행

## Additive Cipher(=Caesar cipher)

- plainText에 secret key를 더해주고 mod26을 해서 Cipher text를 구하는 방식
- 각 문자에 동일한 secret key를 더해줘야한다.
- 복호화할 때는 동일한 secret key를 빼주면 된다
- shift cipher라고도 한다

## Multiplicative Cipher

- Plain Text에 동일한 secret key를 곱해주고 mod 26을 해서 Cipher Text를 구하는 방식
- 복호화할 때는 secret key의 곱셈역을 곱해주고 mod 26을 한다

## Affine Cipher

- additive와 miltiplicative의 혼합

## Substitution Ciphers(대치 암호)

## Monoalphabetic Substitution Cipher

- 알파벳 각각을 다른 알파벳에 1:1 대응시켜서 암호화
- 26!개의 키가 나올 수 있다

## Polyalphabetic Cipher

- monoalphabetic의 경우 1:1 대응으로 바뀐다면 polyalphabetic은 a가 어떨 때는 c로 바뀌고 어떨 때는 d로 바뀐다!

### Autokey Cipher

- 첫번째 글자에만 주어진 키를 사용하고, 두번째부터는 그 전의 p의 value를 키값으로 사용한다

### Playfair Cipher

![Screen Shot 2022-04-17 at 6.20.02 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_6.20.02_PM.png)

- 알파벳 26글자를 랜덤하게 5x5행렬에 집어넣은 것을 secret key로 사용한다.
    - 26개를 25개 안에 집어넣어야하기 때문에 두 개의 글자를 하나의 박스에 집어넣는다
- 두 글자씩 끊어서 암호화를 진행한다
- 같은 글자가 연속해서 나올 경우 우리가 사전에 정한 알파벳을 중간에 넣어준다
- 암호화
    - 두 글자가 행렬의 같은 row에 있다면 오른쪽으로 1칸 shift한 것이 cipher text가 된다
    - 두 글자가 행렬의 같은 column에 있다면 아래쪽으로 1칸 shift한 것이 cipher text가 된다
    - 두 글자가 column이나 row가 겹치지 않는다면 두 글자를 꼭지점으로 하는 사각형을 찾아 반대편 끝에 있는 알파벳을 찾으면 그것이 cipher text가 된다

### Vigenere Cipher

- secret key의 length만큼 plain text를 끊어서 암호화를 진행한다
    
    ![Screen Shot 2022-04-17 at 6.25.23 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_6.25.23_PM.png)
    

### Hill cipher

- 행렬 곱셈을 이용하여 암호화를 진행한다
- row를 채우는 식으로 plain text를 행렬로 표현한다
- 남는 자리는 z를 삽입한다
- 행렬 C = 행렬P * 행렬 K
- mod 26을 해줘야함
    
    ![Screen Shot 2022-04-17 at 6.27.18 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_6.27.18_PM.png)
    

### One-Time pad

- 완벽한 random한 secret key를 사용한다면 perfect secrecy를 구현할 수 있다

## Transposition Ciphers(치환 암호)

- symbol들의 순서를 바꿔서 암호화를 진행

### Keyless Transposition Cipher

- 키가 없다....!
- 방식들
    1. plain text의 짝수번째 나오는 글자와 홀수번째 나오는 글자를 나눠서 더해버리고 붙인다
    2. n개의 row를 가진 행렬에 text를 대입하는데 대입할 때는 row를 따라 집어넣고 세로로 읽어버린다!

### Keyed Transposition Cipher

- secret key만큼 text 잘라서 순서대로 넣는다
    
    ![Screen Shot 2022-04-17 at 7.09.50 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_7.09.50_PM.png)
    

### Combining Two Approaches(keyless + keyed)

![Screen Shot 2022-04-17 at 7.10.28 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_7.10.28_PM.png)

## Stream and Block Ciphers

- Stream Cipher
    - 한 글자씩 cipher
- Block Cipher
    - 한 블럭(크기는 알아서)씩 cipher

### Stream Cipher

- plain text의 한 글자씩 대응되는 secret key값으로 encryption 해서 cipher text만들어냄
    
    ![Screen Shot 2022-04-17 at 7.17.30 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_7.17.30_PM.png)
    
- Autokey Cipher
    - key 값으로 이전 글자의 plain text를 사용한다~
- Vigenere Cipher
    - n까지 있는 key 값을 반복해서 사용

### Block Cipher

- 같은 블록 내에서는 같은 키값으로 암호화
    
    ![Screen Shot 2022-04-17 at 7.21.14 PM.png](Traditiona%20cc52f/Screen_Shot_2022-04-17_at_7.21.14_PM.png)
    
- Hill Cipher
    - 행렬로 이루어진 키값을 pain text 행렬에 곱해서 암호화

#