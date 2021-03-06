---
title: "using qemu-static to analyze ARM binary"
date: 2019-06-29
categories: Tip
tags : ARM qemu pwntools
---


using qemu-arm-static to analyze in my Ubuntu environment

---


Install
-----

##### install qemu,gdb-multiarch
```
sudo apt-get install qemu-user-static
sudo apt-get install gdb-multiarch
```


##### install gcc architecture
```
sudo apt-get install -y  gcc-multilib-arm-linux-gnueabi;
sudo apt-get install -y  gcc-multilib-arm-linux-gnueabihf;
sudo apt-get install -y  gcc-multilib-mips-linux-gnu;
sudo apt-get install -y  gcc-multilib-mips64-linux-gnuabi64;
sudo apt-get install -y  gcc-multilib-mips64el-linux-gnuabi64;
sudo apt-get install -y  gcc-multilib-mipsel-linux-gnu;
sudo apt-get install -y  gcc-multilib-powerpc-linux-gnu;
sudo apt-get install -y  gcc-multilib-powerpc64-linux-gnu;
sudo apt-get install -y  gcc-multilib-s390x-linux-gnu;
sudo apt-get install -y  gcc-multilib-sparc64-linux-gnu
```

After install
-----

##### run arm binary
```
qemu-arm-static -L /usr/arm-linux-gnueabihf ./binary parameter
```

##### gdb analyze
```
local:
    qemu-arm-static -L /usr/arm-linux-gnueabihf -g 31338 ./binary parameter

in gdb:
    gdb-multiarch
    target remote localhost:31338
    (analyze binary using 'c' and break point ) 
```

##### pwntools using qemu

```python
from pwn import *
p=process(["qemu-arm-static","-L","/usr/arm-linux-gnueabihf","./binary","parameter"])
```
-----
