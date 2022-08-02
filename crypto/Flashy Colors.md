# crypto/Flashy Colors

## Challenge

Computer pixels are so interesting. Do you know that they are actually made of three lights? It's like each pixel has 3 tiny light switches, red, green, and blue, that can go on or off to create so many different colors.
<br>
<br>
Anyway, here are some flashy colors for you.

[`flashycolors.png`](https://drive.google.com/file/d/1X7PMv0vi-Cp_xKzYeFd_wxxPGY373jgz/view)

## Solution

Alright, colors and RGB it is. First thing we can try to do is represent each square in the image in RGB, with a `1` or a `0` for whether it contains a primary color or not. For instance, red is `100`, magenta is `101`, and white is `111`. We write the numbers in a way that they keep their relative cell positions.

Here's how we convert the colors to numbers. There's definitely a way to do this directly from the image, but of course that's not what we did at all.
```cpp
#include <iostream>
using namespace std;
int main() {
    // black is K
    string s[10] = {
        "KKBYKYBKRM",
        "BCRRBRRYMR",
        "BGKRYCYKCC",
        "RMRMYGMRWC",
        "GKYCCKWYCY",
        "GRMWBCMMKW",
        "GCKCGBRCYB",
        "MBWCMMYGCB",
        "GWGKWYCKKW",
        "BCWCMCCWBM"
    };
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            if (j != 0) cout << ' '; // spacing separator

            // bools are printed as 1 (true) and 0 (false)
            cout << ( // red switch
                s[i][j] == 'R'
                || s[i][j] == 'M'
                || s[i][j] == 'Y'
                || s[i][j] == 'W'
            );
            cout << ( // green switch
                s[i][j] == 'G'
                || s[i][j] == 'C'
                || s[i][j] == 'Y'
                || s[i][j] == 'W'
            );
            cout << ( // blue switch
                s[i][j] == 'B'
                || s[i][j] == 'C'
                || s[i][j] == 'M'
                || s[i][j] == 'W'
            );
        }
        cout << '\n';
    }
}
```
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

Unfortunately comes the part that caused us to miss this during contest. The 1s and 0s probably indicated binary to ASCII, so 8 numbers per char. Even if we had realized that there were padding zeros (the above section has 300 bits, not a multiple of 8), we also failed to realize that we were actually supposed to read the colors vertically and not horizontally.

Now's time for code!! Insert Python here and...
```py
bits = '''\
000 000 001 110 000 110 001 000 100 101
001 011 100 100 001 100 100 110 101 100
001 010 000 100 110 011 110 000 011 011
100 101 100 101 110 010 101 100 111 011
010 000 110 011 011 000 111 110 011 110
010 100 101 111 001 011 101 101 000 111
010 011 000 011 010 001 100 011 110 001
101 001 111 011 101 101 110 010 011 001
010 111 010 000 111 110 011 000 000 111
001 011 111 011 101 011 011 111 001 101\
'''.split("\n")

# time to read bits by column, rather than row...
s = ""
for j in range(0, 39, 4):
    for i in range(10):
        # i = row number, j = col (4-wide)
        s += bits[i][j:j+3] # pick up 3 characters (4th character is space)
        
s = s[4::] # remove 4 padding zeros at start

# lastly, convert the string to ASCII!
x = int('0b' + s, 2)
# .to_bytes(# of bytes = # of bits / 8 rounded up, byte order = most significant first)
print(x.to_bytes((x.bit_length() + 7) // 8, 'big').decode())

# there might've been a more efficient way to do this, but it's solved regardless :D
```

## Flag

`LITCTF{0MG_I_l0ve_th3s3_fla5hy_c0lor}`

So sad we didn't solve this problem in contest, but minus the column bit it was pretty cool.

## Notes

Of course, you could also just type the RGB codes manually.

Or, you could automate it entirely. Feeling overkill?
```js
// yes, to read the pixel data of an imported file in web JS,
// you have to go input -> filereader -> image -> canvas

const listen = async (target, type) =>
    new Promise((resolve) => {
        target.addEventListener(type, resolve);
    })

const input = document.createElement("input");
input.type = "file";
input.click();
await listen(input, "change");

const file = input.files[0];
const reader = new FileReader();
reader.readAsDataURL(file);
await listen(reader, "load");

const image = new Image();
image.src = reader.result;
await listen(image, "load");

const canvas = document.createElement("canvas");
canvas.width = image.width;
canvas.height = image.height;
const context = canvas.getContext("2d");
context.drawImage(image, 0, 0);

let str = "";
for (let y = 0; y < 10; ++y) {
    if (y != 0) str += "\n";
    for (let x = 0; x < 10; ++x) {
        if (x != 0) str += " ";
        const [r,g,b] = context.getImageData(image.width*(x+.5)/10, image.height*(y+.5)/10, 1, 1).data;
        str += [r,g,b].map(n => +(n>=128)).join("");
    }
}
console.log(str);
```
