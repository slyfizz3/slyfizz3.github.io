---
title: "N4NU's Rev-chall[Baby]"
date: 2019-06-26
categories: Rev-chall
tags: Baby MIPS ARM
---
I will solve all [N4NU's Rev-chall list](https://pastebin.com/q7LGi8w5)

solved (Baby)[10/11]

probs are from
[N4NU rev-chall[Baby]](https://github.com/N4NU/Reversing-Challenges-List/tree/master/Baby)

-------------------------------------

dMd
-----

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

```
Flag:b781cbb29054db12f88f08c6e161c199
```

SRM
-----

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

```
Flag:CZ9dmq4c8g9G7bAX
```

Serial
-----

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

```
Flag:EZ9dmq4c8g9G7bAV
```


SPIM
-----

```
#"IVyN5U3X)ZUMYCs"

User Text Segment [00400000]..[00440000]
[00400000] 8fa40000  lw $4, 0($29)        ; 183: lw $a0 0($sp) # argc 
[00400004] 27a50004  addiu $5, $29, 4     ; 184: addiu $a1 $sp 4 # argv 
[00400008] 24a60004  addiu $6, $5, 4      ; 185: addiu $a2 $a1 4 # envp 
[0040000c] 00041080  sll $2, $4, 2        ; 186: sll $v0 $a0 2 
[00400010] 00c23021  addu $6, $6, $2      ; 187: addu $a2 $a2 $v0 
[00400014] 0c100009  jal 0x00400024 [main]; 188: jal main 
[00400018] 00000000  nop                  ; 189: nop 
[0040001c] 3402000a  ori $2, $0, 10       ; 191: li $v0 10 
[00400020] 0000000c  syscall              ; 192: syscall # syscall 10 (exit) 
[00400024] 3c081001  lui $8, 4097 [flag]  ; 7: la $t0, flag 
[00400028] 00004821  addu $9, $0, $0      ; 8: move $t1, $0 
[0040002c] 3401000f  ori $1, $0, 15       ; 11: sgt $t2, $t1, 15 
[00400030] 0029502a  slt $10, $1, $9          
[00400034] 34010001  ori $1, $0, 1        ; 12: beq $t2, 1, exit 
[00400038] 102a0007  beq $1, $10, 28 [exit-0x00400038] 
[0040003c] 01095020  add $10, $8, $9      ; 14: add $t2, $t0, $t1 
[00400040] 81440000  lb $4, 0($10)        ; 15: lb $a0, ($t2) 
[00400044] 00892026  xor $4, $4, $9       ; 16: xor $a0, $a0, $t1 
[00400048] a1440000  sb $4, 0($10)        ; 17: sb $a0, 0($t2) 
[0040004c] 21290001  addi $9, $9, 1       ; 19: add $t1, $t1, 1 
[00400050] 0810000b  j 0x0040002c [for]   ; 20: j for 
[00400054] 00082021  addu $4, $0, $8      ; 24: move $a0, $t0 
[00400058] 0c100019  jal 0x00400064 [printstr; 25: jal printstring 
[0040005c] 3402000a  ori $2, $0, 10       ; 26: li $v0, 10 
[00400060] 0000000c  syscall              ; 27: syscall 
[00400064] 34020004  ori $2, $0, 4        ; 30: li $v0, 4 
[00400068] 0000000c  syscall              ; 31: syscall 
[0040006c] 03e00008  jr $31               ; 32: jr $ra 
```

just Hand-ray mips asm!!!!

```python
arr="IVyN5U3X)ZUMYCs"
for i in range(len(arr)):
	print(chr(ord(arr[i])^i),end="")
```

```
Flag:IW{M1P5_!S_FUN}
```

File_Checker
-----

```python
table=[
4846,4832,4796,4849,4846,4843,4850,4824,
4852,4847,4818,4852,4844,4822,4794]
for i in table:
  print(chr(4919-i),end="")
```

```
Flag:IW{FILE_CHeCKa}
```

ServerfARM
-----

```python
from pwn import *
import string
#context.log_level = "debug"
p=process(["qemu-arm-static","-L","/usr/arm-linux-gnueabihf","./serverfarm",'f','i','z','z'])
argv=["#"]
for i in string.printable:
	for j in string.printable:
		if ord(i)%ord(j)==65:
			argv.append(i+j)
			break
argv.append("1337")
argv.append("A")
print "Key:"+' '.join(i for i in argv)
flag=""
for i in range(4):
	p.sendline(argv[i])
	if i==3:
		p.recvuntil(":")
	else:
		p.recvuntil("block:")
	flag+=p.recvuntil("\n")
flag=flag.replace("\n","")
print "Flag:"+flag
```

[using qemu-static to analyze ARM binary](https://slyfizz3.github.io/tip/qemu-arm-static/)

```
Flag:IW{S.E.R.V.E.R>=F:A:R:M}
```

Matriochka_-_Step_1
-----

```
Flag:Much_secure__So_safe__Wow
```

Matriochka_-_Step_2
-----

```python
from z3 import *
arr=[Int('a%i'%i) for i in range(11)]
s=Solver()
s.add(42 * (len(arr) + 1) == 504)
s.add(arr[0] == 80 )
s.add( 2 *arr[3] == 200 )
s.add(arr[0] + 16 ==arr[6] - 16 )
v4 =arr[5]
s.add(v4 == 9 * len(arr) - 4 )
s.add(arr[1] ==arr[7] )
s.add(arr[1] ==arr[10] )
s.add(arr[1] - 17 == arr[0] )
s.add(arr[3] ==arr[9] )
s.add(arr[4] == 105 )
s.add(arr[2] -arr[1] == 13 )
s.add(arr[8] -arr[7] == 13 )
print(s.check())
m=s.model()
print ("Flag:"+''.join(chr(int(str(m.evaluate(arr[i]))))for i in range(11)))
```

```
Flag:Pandi_panda
```

Matriochka_-_Step_3
-----

```python
table=[68,105,100,95,121,111,117,95,108,105,107,101,95,115,105,103,110,97,108,115,63,]
print(''.join(chr(i)for i in table))
```

```
Flag:Did_you_like_signals?
```

warm_up
-----


```c
typedef long int int64_t;
```

if you add this script to given c source and compile,
you can get Flag

![warm_up](https://raw.githubusercontent.com/slyfizz3/slyfizz3.github.io/master/image/Rev-chall/Baby/warmup.png)

```
Flag:ASIS{hi_all_w31c0m3_to_ASISCTF}
```

-----

```
Still editing....
```
