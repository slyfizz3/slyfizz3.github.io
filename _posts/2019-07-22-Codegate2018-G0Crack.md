---
title: "Codegate2018 G0Crack"
date: 2019-07-22 18:30
categories: write-up
tags: Golang Codegate
---

```
codegate 2018 final에 나왔던 Golang binary이다.
```


![error](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/G0Crack/error.png)

먼저 바이너리를 디컴파일 하려고 하면 오류가 뜬다.

![compiler](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/G0Crack/compiler.png)


Options-Compiler-sizeof(int)를 8에서 4로 고치면 정상적으로 디컴파일이 된다.


![input](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/G0Crack/input.png)

main_checkInput을 보면 입력받는 값의 길이가 홀수인지 판별하고 각 입력값의 범위가 33~126인지 판별해낸다.

만약 맞다면 main_checkValid로 이동하는데 이곳에서 중요하게 볼 포인트는 두가지이다.

![point1](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/G0Crack/poin1.png)

1.(입력값*2)^main_key가 main_check인지를 판단하는 부분과(입력값의 마지막 부분은 포함 안함)

![point2](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/G0Crack/point2.png)

2.입력값의 마지막 부분이 main_check의 인덱스를 모두 더한후 15를 더한 v4를 
v4 - 127 * ((((4647998506761461825 * v4) >> 64) >> 5) - (v4 >> 63)) - 23
연산한 결과와 같은지를 판별해낸다.

이 두 부분을 만족시키려면

1번은 (main_key[i%len(main_key)]^main_check[i])/2

2번은 그대로 연산해주면 된다.

```python
from pwn import *
p=process("./G0Crack")
main_key=[0x4A,0x69,0x18,0x61,0x75,0x66]
main_check=[140, 1, 240, 167, 165, 216, 184, 9, 242, 133, 203, 174, 174, 13, 112, 187, 245, 230]
flag=""
for i in range(len(main_check)):
	flag+=chr((main_key[i%len(main_key)]^main_check[i])/2)
v4=sum(main_check)+15
flag+=chr(v4 - 127 * ((((4647998506761461825 * v4) >> 64) >> 5) - (v4 >> 63)) - 23 )
print(flag)
p.sendline(flag)
print p.recv()
p.interactive()
```

![result](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/G0Crack/result.png)

```
Flag is c4tch_y0ur_dr24m@@!
```

go언어 더 공부하자
