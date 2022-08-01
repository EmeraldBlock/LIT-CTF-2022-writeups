# rev/math test

## Challenge

this math test is hard

[`math`](https://drive.google.com/file/d/1jGE3v40Xk3-Fq2GsnGvwzU8prZEoL3Iz/view)

## Solution

We're given an executable, and running it, we see we have to answer a bunch of questions correctly:
```
Question #1: What is 1+1?
2
...
Question #5: What is the 41st fibbonaci number?
(uh, we can search it up?)
...
Question #7: What number am I thinking of?
(bruh)
...
You got 4 out of 10 right!
If you get a 10 out of 10, I will give you the flag!
```
So we need to find the answers somewhere. The first few are solvable and have answers 2, 4, 240, 3.

We run `objdump -s` and look in section `.data` and find those answers.
```
 4080 02000000 00000000 04000000 00000000  ................
 4090 f0000000 00000000 03000000 00000000  ................
 40a0 6d8dde09 00000000 0a000000 00000000  m...............
 40b0 87155900 00000000 491ea106 00000000  ..Y.....I.......
 40c0 5fe96020 00000000 09000000 00000000  _.` ............
```
Now we can just extract, convert to decimal, and enter them into the program. (Note that the numbers are stored little-endian, so bytes `6d 8d de 09` represent `0x09de8d6d`.) Here's a program, but it can be done by hand with a hex-to-dec converter as well:
```js
`\
 4080 02000000 00000000 04000000 00000000  ................
 4090 f0000000 00000000 03000000 00000000  ................
 40a0 6d8dde09 00000000 0a000000 00000000  m...............
 40b0 87155900 00000000 491ea106 00000000  ..Y.....I.......
 40c0 5fe96020 00000000 09000000 00000000  _.\` ............`
    .match(/.{8} .{8}/g) // get (long) ints
    .map(intstr=>
        parseInt(
            intstr.match(/\S\S/g).reverse().join(""), // convert little-endian to number
            0x10,
        )
    ).join("\n")
/*
2
4
240
3
165580141
10
5838215
111222345
543222111
9
*/
```

## Flag

`LITCTF{y0u_must_b3_gr8_@_m4th_i_th0ught_th4t_t3st_was_imp0ss1bl3!}`
