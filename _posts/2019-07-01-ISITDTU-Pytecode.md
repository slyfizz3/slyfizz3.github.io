---
title: "ISITDTUCTF2019[prequal]-Pytecode"
date: 2019-06-30
categories: write-up
tags : Bytecode Python
---


파이썬 바이트 코드 문제는 처음 접해봤는데 천천히 분석해서 풀었다.

-----

```
C0rr3ct func:
  6           0 LOAD_CONST          1 ('Wow!!!You so best^_^') 
              3 PRINT_ITEM          
              4 PRINT_NEWLINE       
              5 LOAD_CONST               0 (None)
              8 RETURN_VALUE        

Ch3cking func:
  8           0 LOAD_CONST               1 (0)
              3 STORE_FAST               1 (check)

  9           6 LOAD_GLOBAL              0 (ord)  
              9 LOAD_FAST                0 (flag)
             12 LOAD_CONST               1 (0)  
             15 BINARY_SUBSCR       
             16 CALL_FUNCTION            1
             19 LOAD_CONST               2 (52)   
             22 BINARY_ADD                          
             23 LOAD_GLOBAL              0 (ord)
             26 LOAD_FAST                0 (flag)
             29 LOAD_CONST               3 (-1)  
	                              =>arr[0]+52==arr[-1]
             32 BINARY_SUBSCR       
             33 CALL_FUNCTION            1
             36 COMPARE_OP               3 (!=)
             39 POP_JUMP_IF_TRUE        78
             42 LOAD_GLOBAL              0 (ord)    
             45 LOAD_FAST                0 (flag)
             48 LOAD_CONST               3 (-1)
             51 BINARY_SUBSCR       
             52 CALL_FUNCTION            1
             55 LOAD_CONST               4 (2)         
             58 BINARY_SUBTRACT     
             59 LOAD_GLOBAL              0 (ord)
             62 LOAD_FAST                0 (flag)
             65 LOAD_CONST               5 (7)    
             68 BINARY_SUBSCR       
             69 CALL_FUNCTION            1
             72 COMPARE_OP               3 (!=)
             75 POP_JUMP_IF_FALSE       88  
	                             => flag[-1]-2==flag[7]

 10     >>   78 LOAD_GLOBAL             1 (F41l)
             81 CALL_FUNCTION            0
             84 POP_TOP             
             85 JUMP_FORWARD           752 (to 840)

 11     >>   88 LOAD_FAST                0 (flag)   
             91 LOAD_CONST               5 (7)
             94 SLICE+2             
             95 LOAD_CONST               6 ('ISITDTU')
             98 COMPARE_OP               3 (!=) 
            101 POP_JUMP_IF_FALSE      120  
	                       =>flag[0]~flag[6]='ISITDTU'

 12         104 LOAD_GLOBAL              2 (sys)
            107 LOAD_ATTR                3 (exit)
            110 LOAD_CONST               1 (0)
            113 CALL_FUNCTION            1
            116 POP_TOP             
            117 JUMP_FORWARD           720 (to 840)

 13     >>  120 LOAD_FAST                0 (flag)     
            123 LOAD_CONST               7 (9)
            126 BINARY_SUBSCR       
            127 LOAD_FAST                0 (flag)
            130 LOAD_CONST               8 (14)
            133 BINARY_SUBSCR       
            134 COMPARE_OP               3 (!=)  
            137 POP_JUMP_IF_TRUE       180  
	                         => flag[9]==flag[14]
            140 LOAD_FAST                0 (flag)   
            143 LOAD_CONST               8 (14)       
            146 BINARY_SUBSCR       
            147 LOAD_FAST                0 (flag)
            150 LOAD_CONST               9 (19)
            153 BINARY_SUBSCR       
            154 COMPARE_OP               3 (!=)     
            157 POP_JUMP_IF_TRUE       180    
	                      => flag[14]==flag[19]
            160 LOAD_FAST                0 (flag)
            163 LOAD_CONST               9 (19)        
            166 BINARY_SUBSCR       
            167 LOAD_FAST                0 (flag)
            170 LOAD_CONST              10 (24)    
            173 BINARY_SUBSCR       
            174 COMPARE_OP               3 (!=)
            177 POP_JUMP_IF_FALSE      193     
	                       => flag[19]==flag[24]

 14     >>  180 LOAD_FAST                1 (check)
            183 LOAD_CONST              11 (1)
            186 INPLACE_ADD         
            187 STORE_FAST               1 (check)
            190 JUMP_FORWARD           647 (to 840)

 15     >>  193 LOAD_GLOBAL              0 (ord)
            196 LOAD_FAST                0 (flag)
            199 LOAD_CONST              12 (8)
            202 BINARY_SUBSCR       
            203 CALL_FUNCTION            1
            206 LOAD_CONST              13 (49)
            209 COMPARE_OP               3 (!=)    
            212 POP_JUMP_IF_TRUE       235     
	                         => flag[8]==49
            215 LOAD_FAST                0 (flag)
            218 LOAD_CONST              12 (8)
            221 BINARY_SUBSCR       
            222 LOAD_FAST                0 (flag)
            225 LOAD_CONST              14 (16)
            228 BINARY_SUBSCR       
            229 COMPARE_OP               3 (!=)   
            232 POP_JUMP_IF_FALSE      245      
	                       => flag[8]==flag[16]

 16     >>  235 LOAD_GLOBAL              1 (F41l)
            238 CALL_FUNCTION            0
            241 POP_TOP             
            242 JUMP_FORWARD           595 (to 840)

 17     >>  245 LOAD_FAST                0 (flag)
            248 LOAD_CONST              15 (10)
            251 LOAD_CONST               8 (14)
            254 SLICE+3                              
            255 LOAD_CONST              16 ('d0nT')
            258 COMPARE_OP               3 (!=)
            261 POP_JUMP_IF_FALSE      277 
	                  =>flag[10]~flag[13]=='d0nT'

 18         264 LOAD_FAST                1 (check)
            267 LOAD_CONST              11 (1)
            270 INPLACE_ADD         
            271 STORE_FAST               1 (check)
            274 JUMP_FORWARD           563 (to 840)

 19     >>  277 LOAD_GLOBAL              4 (int)
            280 LOAD_FAST                0 (flag)   int(flag[18])
            283 LOAD_CONST              17 (18)
            286 BINARY_SUBSCR                         +
            287 CALL_FUNCTION            1
            290 LOAD_GLOBAL              4 (int)
            293 LOAD_FAST                0 (flag)
            296 LOAD_CONST              18 (23)   int(flag[23])
            299 BINARY_SUBSCR       
            300 CALL_FUNCTION            1
            303 BINARY_ADD                             +
            304 LOAD_GLOBAL              4 (int)
            307 LOAD_FAST                0 (flag)
            310 LOAD_CONST              19 (28)    int(flag[28])
            313 BINARY_SUBSCR       
            314 CALL_FUNCTION            1
            317 BINARY_ADD          
            318 LOAD_CONST               7 (9)
            321 COMPARE_OP               3 (!=)       ==9
            324 POP_JUMP_IF_TRUE       347
            327 LOAD_FAST                0 (flag)    
            330 LOAD_CONST              17 (18)
            333 BINARY_SUBSCR       
            334 LOAD_FAST                0 (flag)
            337 LOAD_CONST              19 (28)
            340 BINARY_SUBSCR       
            341 COMPARE_OP               3 (!=)
            344 POP_JUMP_IF_FALSE      357  
	                    => flag[18]==flag[28]

 20     >>  347 LOAD_GLOBAL              1 (F41l)
            350 CALL_FUNCTION            0
            353 POP_TOP             
            354 JUMP_FORWARD           483 (to 840)

 21     >>  357 LOAD_FAST                0 (flag)
            360 LOAD_CONST              20 (15)
            363 BINARY_SUBSCR       
            364 LOAD_CONST              21 ('L')
            367 COMPARE_OP               3 (!=) 
            370 POP_JUMP_IF_FALSE      386
	                          =>  flag[15]=='L'

 22         373 LOAD_FAST                1 (check)
            376 LOAD_CONST              11 (1)
            379 INPLACE_ADD         
            380 STORE_FAST               1 (check)
            383 JUMP_FORWARD           454 (to 840)

 23     >>  386 LOAD_GLOBAL              0 (ord)
            389 LOAD_FAST                0 (flag)
            392 LOAD_CONST              22 (17)
            395 BINARY_SUBSCR       
            396 CALL_FUNCTION            1
            399 LOAD_CONST              23 (-10)
            402 BINARY_XOR          
            403 LOAD_CONST              24 (-99)
            406 COMPARE_OP               3 (!=)
            409 POP_JUMP_IF_FALSE      422
	                      => flag[17]^-10==-99
            .
            .
            .
            .
            .
            .
```

이런식으로 Hand-ray해준후 해석한 내용을 바탕으로 코드를 짜면된다.

```python
flag=[0]*30
one="ISITDTU"
two="d0nT"
for i in range(len(one)):
	flag[i]=ord(one[i])
for j in range(10,10+len(two),1):
	flag[j]=ord(two[j-10])
flag[-1]=flag[0]+52
flag[7]=flag[-1]-2
flag[18]=ord("3")
flag[22]=flag[13]+32
flag[14]=ord("_")
flag[9]=flag[14]
flag[19]=flag[14]
flag[24]=flag[19]
flag[8]=49
flag[16]=flag[8]
flag[28]=flag[18]
flag[15]=ord('L')
flag[17]=-10^-99
flag[27]=100
flag[20]=flag[27]-2
flag[25]=ord('C')
flag[23]=ord("3")
count=2441
for i in range(len(flag)):
	count-=flag[i]
for i in range(ord("0"),ord("9")+1):
	check=1
	for j in range(2,5):
		if i%j!=0:
			check=0
	if check:
		flag[26]=i
flag[21]=count-flag[26]
print(''.join(chr(i)for i in flag))
```

```
Flag:ISITDTU{1_d0nT_L1k3_b:t3_C0d3}
```

혼자 ctf하려니 힘이 안난다아아~

ctf 같이 하실분 메일 주세요...

rcercerc3@gmail.com
