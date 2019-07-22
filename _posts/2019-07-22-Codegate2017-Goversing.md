---
title: "Codegate2017 Goversing"
date: 2019-07-22
categories: write-up
tags: Golang Codegate
---

```
codegate 2017 prequal에 출제되었던 Golang binary이다.
```

그냥은 디컴파일이 되지 않는데 저번 포스트에서 소개했던데로 Options-Compiler-sizeof(int)를 8에서 4로 고치면 정상적으로 디컴파일이 된다.

![input](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Goversing/input.png)

id_check과 pw_check은 모두 입력값이 33~126의 범위인지 판별한다.

맞다면 id_pw_check로 이동하는데 이번에도 두가지 부분을 유심히 봐야한다.

![point1](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Goversing/point1.png)

1.id에 연산을 하는부분

![point2](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Goversing/point2.png)

2.id^pw가 pw_key인지 판별하는 부분

2번은 1번에서 id를 구해준 후 PW_key를 xor 하면 되는데 1번이 너무 분석하기 힘들어서 게싱으로 풀었다.

![guess](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Goversing/guess.png)

id의 배열을 보면 배열의 인덱스가 모두 0 이상 7 이하인 것을 알 수 있다.
즉 0이나 1로 바뀔 수가 있을 것이라고 생각하였고 각 인덱스에 4를 나눈 후 연산에 있었던 xor 1을 해보니

이진수가 나와서 8비트씩 끊어서 계산하니 ID를 구할 수 있었다.

```python
from pwn import *
p=process("./Goversing")
main_ID_KEY=[7, 0, 4, 5, 4, 7, 7, 0, 4, 2, 0, 6, 6, 3, 4, 5, 4, 0, 3, 6, 1, 0, 6, 1, 7, 2, 0, 6, 1, 7, 5, 3, 4, 2, 0, 6, 1, 0, 1, 5, 6, 3, 4, 5, 4, 7, 7, 7, 7, 0, 4, 5, 4, 0, 3, 1, 5, 6, 3, 3, 6, 6, 4, 7]
main_PW_KEY=[18,86, 46, 27, 92, 52, 106, 93, 115, 41, 15, 91, 28, 103, 52, 111, 17, 80, 30, 58, 25, 112, 53, 84, 63, 69, 45, 71, 46]	
ID=''
for i in range(len(main_ID_KEY)):
	tmp=int(main_ID_KEY[i])/4
	ID+=str(tmp^1)
Realid=''.join(chr(int(ID[i:i+8],2))for i in range(0,len(ID),8))
print(Realid)
Realpw=''.join(chr(ord(Realid[i%len(Realid)])^main_PW_KEY[i])for i in range(len(main_PW_KEY)))
print(Realpw)
p.sendlineafter(">","1")
p.sendlineafter(":",Realid)
p.sendlineafter(":",Realpw)
p.recv()
p.sendlineafter(">","2")
print p.recv()
p.interactive()


```
![result](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Goversing/result.png)
```
FLAG{39e1316661e80847b12dae96789a13e4a3d3b496}
```

다음에 한번 더 제대로 풀어봐야겠다...
