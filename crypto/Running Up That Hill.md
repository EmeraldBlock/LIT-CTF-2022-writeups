# crypto/Running Up That Hill

## Challenge

If I only could, I'd make a deal with god, and I'd get him to swap our places.

[`RunningUpThatHill.zip`](https://drive.google.com/file/d/17IAxOBN1G5POQ9wwZ4rpd0hY_ayfCJFi/view) 

## Solution

The zip file only contains a text file containing:
```
ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_{}

5 19 27
10 19 24
11 6 16

A8FC7A{H7_ZODCEND_8F_CCQV_RTTVHD}
```

Honestly, this challenge took us a while longer than it should have. The flavor text mentions swapping, and the challenge has a very unique name seemingly unrelated to the challenge, so doing some internet searching reveals there's actually a cipher called the [Hill Cipher](https://en.wikipedia.org/wiki/Hill_cipher), which uses a matrix to encrypt text. As per usual [dCode](https://www.dcode.fr/hill-cipher) is clutch, and inputting our given information into the website reveals our flag.

## Flag

`LITCTF{B3_RUNN1NG_UP_TH4T_H1LLLL}`
