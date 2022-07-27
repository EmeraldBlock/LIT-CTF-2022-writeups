
## Chall

Computer pixels are so interesting. Do you know that they are actually made of three lights? It's like each pixel has 3 tiny light switches, red, green, and blue, that can go on or off to create so many different colors.

Anyway, here are some flashy colors for you.

[`flashycolors.png`](https://drive.google.com/file/d/1X7PMv0vi-Cp_xKzYeFd_wxxPGY373jgz/view?usp=sharing)

## Solution
Alright, colors and RGB it is. First thing we can try to do is represent each square in the image as a series of 1s and 0s in RGB: 1 if the color is used and 0 if it isn't. For instance, red is `100`, magenta is `101`, and white is `111`. We write the numbers in a way that they keep their relative cell positions.

```
000 000 001 110 000 110 001 000 100 101 
001 011 100 100 001 100 100 110 101 100 
001 010 000 100 110 011 110 000 011 011 
100 101 100 101 110 010 101 100 111 011 
010 000 110 011 011 000 111 110 011 110 
010 100 101 111 001 011 101 101 000 111 
010 011 000 011 010 001 100 011 110 001 
101 001 111 011 101 101 110 010 011 001 
010 111 010 000 111 110 011 000 000 111 
001 011 111 011 101 011 011 111 001 101 
```

Unfortunately comes the part that caused me to miss this during contest. The 1s and 0s probably indicated binary to ASCII, so 8 numbers per char. Even if I realized that there were padding zeros (the above section has 300 bits, not a multiple of 8), I failed to realize that we were actually supposed to read these vertically and not horizontally.

Now's time for code!! Insert Python here and...

```Python

bits = ["000 000 001 110 000 110 001 000 100 101",
"001 011 100 100 001 100 100 110 101 100",
"001 010 000 100 110 011 110 000 011 011",
"100 101 100 101 110 010 101 100 111 011",
"010 000 110 011 011 000 111 110 011 110",
"010 100 101 111 001 011 101 101 000 111",
"010 011 000 011 010 001 100 011 110 001",
"101 001 111 011 101 101 110 010 011 001",
"010 111 010 000 111 110 011 000 000 111",
"001 011 111 011 101 011 011 111 001 101"]

#time to resd bits by column, rather than row...
s = ""
for j in range(0, 39, 4):
    for i in range(10):
        #i = row number, j = helps with col
        s += bits[i][j:j+3]
        
s = s[4::] # removing 4 padding zeros at start

#lastly, convert the string to ASCII!
x = int('0b' + s, 2)
print(x.to_bytes((x.bit_length() + 7) // 8, 'big').decode())

#there might've been a more efficient way to do this, but it's solved regardless :D

```
## Flag
`LITCTF{0MG_I_l0ve_th3s3_fla5hy_c0lor}`

So sad I didn't solve this problem in contest, but minus the column bit I thought it was pretty cool.
