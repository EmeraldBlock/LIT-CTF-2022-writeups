# crypto/Running Up That Hill

**Chall:**
If I only could, I'd make a deal with god, and I'd get him to swap our places.
*This challenge also includes a zip file, which only contains [`challenge.txt`](https://drive.google.com/file/d/1xqQawB_pdjbodbIo4GGUjzrPxDnjv9V5/view?usp=sharing)

**Solution:**
Honestly, I feel like this challenge took us a while longer than it should have. The flavortext mentions swapping, and the challenge has a very unique name seemingly unrelated to the challenge, so doing some googling reveals there's actually a cipher called a [Hill Cipher](https://en.wikipedia.org/wiki/Hill_cipher#:~:text=In%20classical%20cryptography%2C%20the%20Hill,than%20three%20symbols%20at%20once.), which uses some interesting matrix number theory to encrypt text. As per usual [dCode](https://www.dcode.fr/hill-cipher) is clutch, and inputting our given information into the website reveals our flag.

**Flag:**
`LITCTF{B3_RUNN1NG_UP_TH4T_H1LLLL}`