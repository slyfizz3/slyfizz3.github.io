---
title: "Re_sign" 
date: 2019-08-06
categories: CTF
tags : UPX custom-table-base64
---
de1ctf의 re_sign 이라는 문제였고 문제 설명에는 UPX라고 적혀있었다.
올리 디버거로 MUP 방식으로 덤프를 뜨고 ida로 동적디버깅 해보려고했는데 kernel32.dll 오류때문에 할수없어서 
ida는 정적으로 immunity 디버거를 동적으로 사용하면서 분석했다.

분석을 해보니 custom table base64encode라는것을 알았고 분석해서 얻은 테이블들을 이용해 복호화했다.

```python
import string,base64
table="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="
	
m=[0x08,0x3B,0x01,0x20,0x07,0x34,0x09,0x1F,0x18,0x24,0x13,0x03,0x10,0x38,0x09,0x1B,0x08,0x34,0x13,0x02,0x08,0x22,0x12,0x03,0x05,0x06,0x12,0x03,0x0F,0x22,0x12,0x17,0x08,0x01,0x29,0x22,0x06,0x24,0x32,0x24,0x0F,0x1F,0x2B,0x24,0x03,0x15,0x41,0x41]
enc=''
for i in range(len(m)):
	enc+=table[m[i]-1]
enc="H6AfGzIeXjSCP3IaHzSBHhRCEFRCOhRWHAohFjkjOeqjCU=="
table=base64.b64encode(table)
arr="UA93I4S6IjB9OqpEPAcYA55OAkISSwZGDSyBGeRqHDHrJ6wuJlfpKemdLF9hZ7SlZzBcXM0fEMEjRPGzT3qiWhj="
tb=string.maketrans(arr,table)
rb=enc.translate(tb)
print(base64.b64decode(rb))
```

```
de1ctf{EBL4Zguag3B1sBK3KeJ3_MtJi4	
```

중간에 base64충돌이 일어나서 플래그가 이상하게 나오는데 이부분을 직접 디버깅하면서 수정했다.
```
최종 플래그:de1ctf{E_L4nguag3_1s_K3KeK3_N4Ji4}
```


xctf 관련 문제는 항상 못풀고 끝냈는데 이번에는 풀어서 다행이다.
