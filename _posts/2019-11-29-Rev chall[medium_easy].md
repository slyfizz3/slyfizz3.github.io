---
title: "N4NU's Rev-chall[medium_easy]"
date: 2019-11-28
categories: Rev-chall
---


# ASISCTF2018 Destiny

#### analyze.py

```python
from ctypes import *

table1="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/@$_!\"#%&'()*+,-./:;<=>?\n[\\]^{|}~`\t\x00"
table2="@$_!\"#%&'()*+,-./:;<=>?\n"
table3="[\\]^{|}~`\t"
table4="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
libc = CDLL('/lib/x86_64-linux-gnu/libc.so.6')

def checkpass(panel):
	if "\x00" in panel:
		panel=panel[:panel.index("\x00")]
	return panel
def panel(inp,seed):
	libc.srand(seed)
	panel1=''.join([table1[libc.rand()%99] for i in range(libc.rand()%14+3)])
	panel2=''.join([table1[libc.rand()%99] for i in range(libc.rand()%14+3)])
	result=checkpass(panel1)+inp+checkpass(panel2)
	return result

def enc1(inp):
	result=""
	for i in inp:
		if i in table2:
			result+="+"
			result+=chr(table2.index(i)+97)
		elif i in table3:
			result+="++"
			result+=chr(table3.index(i)+97)
		else:
			result+=i
	while(len(result)&3):
			result+="/"
	return result

def enc2(inp):
	result=[]
	for i in range(0,len(inp),4):
		idx1=table4.index(inp[i])
		idx2=table4.index(inp[i+1])
		idx3=table4.index(inp[i+2])
		idx4=table4.index(inp[i+3])	

		v20=(idx1*4)|(idx2>>4)
		v24=((idx2*16)|(idx3>>2))&0xff
		v22=(idx3<<6)&0xff

		result.append(v20)
		result.append(v24)
		result.append(v22|idx4)

	return [(i)for i in result]


seed=#current time seed
flag=#flag
inp=panel(flag,seed)
inp=enc1(inp)
inp=enc2(inp)
print(inp)

```

주어진 바이너리를 파이썬으로 옮겨보았다.

time seed를 이용해서 플래그에 앞뒤로 랜덤한 문자열을  붙여주고  그 문자열의 특수문자들을 +를 이용해서 치환해준다.

그 후 비트연산을 해서 나온 플래그가 주어진 txt 파일이랑 같으면 답이된다.

#### solve.py
```python
from z3 import *

table1='ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
table2="@$_!\"#%&'()*+,-./:;<=>?\n"
table3="[\\]^{|}~`\t"
def enc2(inp):
	result=[]
	for i in range(len(inp)/4):
		idx1=inp[i*4]
		idx2=inp[i*4+1]
		idx3=inp[i*4+2]
		idx4=inp[i*4+3]

		s.add(enc[i*3]==(idx1*4	)|(idx2>>4))
		s.add(enc[i*3+1]==((idx2*16)|(idx3>>2))&0xff)
		s.add(enc[i*3+2]==((idx3<<6)&0xff)|idx4)


enc=open("short","rb").read()
enc=[ord(i) for i in enc]
length=(len(enc) / 3*4)
arr=[BitVec("arr%i"%i,8)for i in range(length)]
s=Solver()
for i in range(len(arr)):
	s.add(arr[i]>=0)
	s.add(arr[i]<len(table1))
enc2(arr)

print(s.check())
m=s.model()
idx=[]
for i in range(length):
	idx.append(int(str(m.evaluate(arr[i]))))
flag=""
for i in range(length):
	flag+=table1[idx[i]]
flag=flag.replace("++","!").replace("+","?")
realflag=""
i=0
while(i!=len(flag)):
	if flag[i]=="?":
		realflag+=table2[ord(flag[i+1])-97]
		i+=1
	elif flag[i]=="!":
		realflag+=table3[ord(flag[i+1])-97]
		i+=1
	else:
		realflag+=flag[i]
	i+=1
print(realflag)
print(realflag[realflag.index("ASIS{"):realflag.index("}")+1])
```

플래그는 time seed를 구하지 않고도 z3로 간단하게 구할수있다.

```
sat
O~$/cASIS{01d_4Nd_GoLD_ASIS_1De4_4H4t_g0e5_f0r_ls!}dI	)M>b|D9W//
ASIS{01d_4Nd_GoLD_ASIS_1De4_4H4t_g0e5_f0r_ls!}
```

끝
