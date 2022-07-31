# crypto/A ROCk

## Challenge

I used to love RSALib, but not anymore...

[arock.zip](https://drive.google.com/uc?export=download&id=1YlGBKe7x0K5erD9O0SxrFjWcCE7sUvfM)

## Solution

The only thing inside the zip file is a text file with the following contents:

```
n=8721839759350069167278335804233522732628197837843780295129915563071581371083672792796544014245271677069445637593635290140076123157617389715458979685368727
e=65537
ct=3831129304332239255280117442393824529915871197799278700713657686877437561020805823052809122048327006270135775002283258453774226602640149683603252934547033
```

These three numbers hint at the [RSA cryptosystem](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) (if it wasn't obvious enough from the challenge text). `n` represents the public key modulus, `e` represents the public key exponent, and `ct` represents the ciphertext, or the encrypted message.

However, under normal circumstances, decrypting a message using only the public key isn't computationally feasible (as this is what makes RSA secure). This is because we need to factor `n` into two primes `p` and `q`, but factoring numbers as large as the given `n` is extremely computationally expensive.

Of course, there's a vulnerability in the problem. A quick search for "RSALib vulnerability" reveals the existence of the [ROCA vulnerability](https://en.wikipedia.org/wiki/ROCA_vulnerability). Essentially, RSALib generates primes only of a certain format, so factoring `n` becomes much more feasible since the search space is reduced. Based on the name of the problem, we can assume this solution is intended and that `n` was generated using RSALib's algorithm.

Now, we have to actually factor `n`. There are a few programs online to exploit the ROCA vulnerability; the one that our team used was [this one](https://github.com/jix/neca). After running the code and waiting for some time, we got this:

```
NECA - Not Even Coppersmith's Attack
ROCA weak RSA key attack by Jannis Harder (me@jix.one)

 *** Currently only 512-bit keys are supported ***

N = 8721839759350069167278335804233522732628197837843780295129915563071581371083672792796544014245271677069445637593635290140076123157617389715458979685368727
Factoring...

 [==============          ] 57.07% elapsed: 1267s left: 953.01s total: 2220.08s

Factorization found:
N = 70672758723177433011581077796363544664410141195648490771860623139983928782297 * 123411621633636675541493070644959834875873057348494554188657868973313372350191
```

The factorization that was found are the primes `p` and `q` in the RSA algorithm. To finish off, we can just plug these numbers all into [dcode.fr](https://www.dcode.fr/rsa-cipher) to get the flag.

## Flag

`LITCTF{rsalib_n0_m0r333}`
