---
title: "N4NU's Rev-chall[Baby]"
date: 2019-06-26
categories: Rev-chall
tags: Baby
---
I will solve all [N4NU's Rev-chall list](https://pastebin.com/q7LGi8w5)


probs are from
[N4NU rev-chall[Baby]](https://github.com/N4NU/Reversing-Challenges-List/tree/master/Baby)

-------------------------------------

dMd

```python
import hashlib
def fixstr(a):
    return a.encode('utf8')
def md5(s):
    return hashlib.md5(fixstr(s)).hexdigest()
if md5("b781cbb29054db12f88f08c6e161c199")=="780438d5b6e29db0898bc4f0225935c0":
    print("valid key")
else:
    print("invalid key")
```
![dMd](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Rev-chall/Baby/dMd.png)

key:b781cbb29054db12f88f08c6e161c199

-----

SRM

```python
from z3 import *
arr=[Int("arr%i"%i) for i in range(16)]
s=Solver()
s.add(arr[0] == 67)
s.add(arr[15] == 88)
s.add(arr[1] == 90)
s.add(arr[1] + arr[14] == 155)
s.add(arr[2] == 57)
s.add(arr[2] + arr[13] == 155)
s.add(arr[3] == 100)
s.add(arr[12] == 55)
s.add(arr[4] == 109)
s.add(arr[11] == 71)
s.add(arr[5] == 113)
s.add(arr[5] + arr[10] == 170)
s.add(arr[6] == 52)
s.add(arr[9] == 103)
s.add(arr[7] == 99)
s.add(arr[8] == 56)
print(s.check())
m=s.model()
flag=""
for i in range(len(arr)):
	flag+=chr(int(str(m.evaluate(arr[i]))))
print(flag)
```

![SRM](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Rev-chall/Baby/SRM.png)

key:CZ9dmq4c8g9G7bAX

-----

Serial

```python
def sub9b(x):
	return 0x9b-x
def subb4(x):
	return 0xb4-x
def subaa(x):
	return 0xaa-x

arr=[0x45,0x5a,0x39,0x64,0x6d,0x71,0x34,0x63]
result=[]
for i in range(len(arr)):
	result.append(sub9b(arr[i]))
result=result[::-1]
result[3]=subb4(arr[4])
result[2]=subaa(arr[5])
print(''.join(chr(i)for i in arr+result))
```

![Serial](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Rev-chall/Baby/Serial.png)

key:EZ9dmq4c8g9G7bAV

-----


