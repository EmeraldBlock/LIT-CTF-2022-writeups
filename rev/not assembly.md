# rev/not assembly

## Challenge

Find the output and wrap it in `LITCTF{}` !

[`notassembly.png`](https://drive.google.com/file/d/1mV8gjChjRDSLuFUYDS_1lZemxA-SBNa7/view)

## Solution

The image contains the following table:
$$
% no proper text mode :(
% also GitHub escapes \\ as \
\begin{array}{l|c|r} \hline
\text{FLAG} & \text{DC} & \text{0} \cr \hline
\text{CODETIGER} & \text{DC} & \text{23} \cr \hline
\text{ORZ} & \text{DC} & \text{4138} \cr \hline
& \text{LOAD} & \text{ORZ} \cr \hline
\text{C0DETIGER} & \text{MULT} & \text{=3} \cr \hline
& \text{ADD} & \text{CODETIGER} \cr \hline
& \text{STORE} & \text{FLAG} \cr \hline
& \text{SUB} & \text{=9538} \cr \hline
& \text{BL} & \text{0RZ} \cr \hline
& \text{BU} & \text{C0DETIGER} \cr \hline
\text{0RZ} & \text{LOAD} & \text{FLAG} \cr \hline
& \text{SUB} & \text{=3571} \cr \hline
& \text{STORE} & \text{FLAG} \cr \hline
& \text{PRINT} & \text{FLAG} \cr \hline
& \text{END} & \cr \hline
\end{array}
$$
Wow! (An) Assembly (language)!

Some of the more unclear ones can be searched up. Here're some notes:
* $\text{DC}$ is obviously "initialize value".
  * The structure also looks like [HLASM](https://www.ibm.com/docs/en/zos/2.1.0?topic=statements-dc-instruction), but other instructions aren't there.
* $\text{LOAD}$ and $\text{STORE}$ suggest we're operating on a single register.
  * The necessity of $\text{STORE}$ suggests this register is independent from the variables (so arithmetic instructions don't directly modify the variables).
* $\text{BL}$ and $\text{BU}$ are "branch" (aka "jump") instructions (like in ARM).
  * $\text{C0DETIGER}$ and $\text{0RZ}$ (with $0$'s, not $O$'s) must then be labels, not variables.
  * Coming immediately after a $\text{SUB}$ instruction, $\text{BL}$ is "branch if less than" (not "branch with link", whatever that is).  $\text{BU}$ must then be "unconditional branch", as otherwise the code flow would make no sense.
    * Here, "less than" with respect to $\text{SUB}$ means "register (after sub) less than 0" or "register (before sub) less than subtrahend", as per assembly convention.
    * As it turns out, no need to worry about overflow! Anyway, this is pseudocode written in $\LaTeX$ with base-10 numbers, so what do you expect?

Let's just rewrite it in actual code!
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int r;
    int flag = 0;
    int codetiger = 23;
    int orz = 4138;
    r = orz;
    c0detiger: r *= 3;
    r += codetiger;
    flag = r;
    r -= 9538;
    if (r < 0) goto _0rz;
    goto c0detiger;
    _0rz: r = flag;
    r -= 3571;
    flag = r;
    cout << flag;
    return 0;
}
// 5149
```

## Flag

`LITCTF{5149}`
