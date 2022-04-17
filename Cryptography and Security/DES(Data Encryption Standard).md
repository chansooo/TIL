# DES(Data Encryption Standard)

- block cipher
- symmetric- key cipher
- 동일 plaintext라도 key에 따라 다른 cipher text 생성
- Feistel Ciper
    - 블록 사이퍼를 만드는 형태 중 하나.
    - 절반은 그대로, 절반은 다른 절반을 키값과 짬뽕한 것과 xor

## DES Structure

![Screen Shot 2022-04-17 at 7.52.07 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_7.52.07_PM.png)

- 64bit plain text
- 56bit key
1. Initial permutation 
2. 1~16Round
3. Final permutation

### Initial and Final Permutation

- Initial Permutation box를 통해 위치를 바꿔준다
    
    ![Screen Shot 2022-04-17 at 7.50.10 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_7.50.10_PM.png)
    
- p-box를 보고 숫자가 적혀있는 plainText[index]에 해당하는 아이를 삽입한다
- Final Permutation은 Initial Permutation과 서로 역 Permutatoin이다

### Rounds

- 64bit의 plain text에서 왼쪽 절반은 $L_i$, 오른쪽 절반을 $R_i$라고 하면
    
    $L_i = R_{i-1}$
    
    $R_i = f(R_{i-1}, K_i) \oplus L_{i-1}$
    

$f$함수에 대해 아라보자

![Screen Shot 2022-04-17 at 8.08.48 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_8.08.48_PM.png)

1. Expansion P-box
    
    ![Screen Shot 2022-04-17 at 8.17.44 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_8.17.44_PM.png)
    
    - 4bit씩 끊어서 옆집에 붙인다...!
2. key와 XOR
3. S-box
    
    ![Screen Shot 2022-04-17 at 8.48.13 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_8.48.13_PM.png)
    
    - 첫번째, 마지막 비트를 row index로 사용하고 2,3,4,5 비트를 column index로 사용해서 값을 찾아낸다
4. Straight P-box
    - straight p-box를 통해 permutation 한다

### Reverse Cipher

- FP와 IP는 서로 역작업
- cipher와 reverse cipher는 코드가 같다.
    - key 값만 역으로 넣으면 된다

### Key Generator

![Screen Shot 2022-04-17 at 8.59.41 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_8.59.41_PM.png)

- parity drop
    - 64 → 56비트로 만드는 과정
    - parity bit 8비트를 버린다
    
    ![Screen Shot 2022-04-17 at 9.09.31 PM.png](DES(Data%20E%20d4635/Screen_Shot_2022-04-17_at_9.09.31_PM.png)
    

## Double DES

- DES 두번 한다
- 112bit인데 57bit 효과밖에 나지 않는다

## Triple DES

- DES 세번
- k1, k2, k1 순서로 쓸 수도 있고
- k1, k2, k3 순서로 키 세개를 쓸 수도 있다
- DES cipher → DES reverse cipher → DES cipher 순서로 진행한다
    - 그래서 첫번째와 두번째 키를 같게 넣어주면 상쇄되므로 single DES처럼 사용할 수 있다