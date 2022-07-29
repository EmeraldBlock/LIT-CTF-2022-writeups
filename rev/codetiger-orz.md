# rev/codetiger-orz

## Challenge

codetiger orzzz

[codetiger-orz.zip](https://drive.google.com/uc?export=download&id=12gmnfSKFI2pknJSJbGYTejS5dNcSOOQK)

## Solution

We are given a Python script which seems to be a self-contained flag decryptor.
However, it seems incomplete, with commented-out code and unused functions.

Here's the main interesting function:
```py
def derivePassword():
    kw = ['~#+', 'v~s', 'r~st', '%xvt#', 'st%tr%x\'t']
    userKeyInput = input('Enter the key: ')  # 7-digit integer

    try:
        retrievePasswordKey = list(map(int, list(userKeyInput)))
        # retrievePasswordKey = list(str(10*0) + len(kw[2]) + str(2**0) + len(kw[0]) + '2' + len("orz) + '0')

        ct = kw[retrievePasswordKey[0]] + kw[retrievePasswordKey[1]] + kw[retrievePasswordKey[2]] + \
            kw[retrievePasswordKey[3]] + kw[retrievePasswordKey[4]] + \
            kw[retrievePasswordKey[5]] + kw[retrievePasswordKey[6]]
        # return ROT(ct, s)
        return 'defaultplaceholderkeystringabcde'
# ...
```
We're asked to input a key, but `# retrievePasswordKey = ...` seems to be one. Yay, no need to brute-force!

Some of the terms need to be casted to `str`, and there's a missing closing `"`. Also, it hasn't been `map`'d to `int`s. Let's fix it up:
```py
    userKeyInput = str(10*0) + str(len(kw[2])) + str(2**0) + str(len(kw[0])) + '2' + str(len("orz")) + '0'
```
(We undo the `list` and replace `userKeyInput`, instead of adding `list(map(int, ...))` and replacing `retrievePasswordKey`, since it's a bit easier.)

Next up, we uncomment `return ROT(ct, s)`. (The original `return` is obviously incorrect.) But `s` isn't defined! However, lower down there's a comment under the definition of `ROT()`:
```py
# s = 1 (mod 2), s = 7 (mod 11), 7 < |s| < 29
# ROT|s| used to create password ciphertext
```
$s = 7$ obviously fits the modulo conditions, so by Chinese Remainder Theorem, $s \equiv 7 \pmod{22}$. At first, $7 < |s| < 29$ seems impossible, but note the absolute value, so $s = -15$ works, and we define `s = -15`. (No idea why `ROT|s|` has `||`.)

Also, `ROT(ct, s)` comes before the definition of `ROT()`, so we move the definition up.

Running the script now prints
```
codetiger orz orz codetiger codetiger orz orz
codetiger orz orz codetiger orz orz codetiger
codetiger orz codetiger orz codetiger orz orz
... (a bunch more)
```
Note `solutionDecrypt()` remains unused, and it seems to parse those words:
```py
def solutionDecrypt(cipher):
# ...
                if t == 'codetiger':
                    b += '1'
                elif t == 'orz':
                    b += '0'
# ...
```
So we (move the `print` statement below its definition and) do `print(solutionDecrypt(solution))`. Run now and we get our flag.

## Flag

`LITCTF{1m_73ry_6ad_a1_r3v_en9in33r1ing}`
