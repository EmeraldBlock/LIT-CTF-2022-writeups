# web/Kevin's Cookies

## Challenge

Welcome to Kevin Zhu's [cookie store](http://litctf.live:31778/)! I heard he sells many super delicious cookies :yum:

## Solution

Web challenge + cookies in the name = probably abusing cookies. Opening the site reveals Kevin telling us that he will not give us cookies because we don't like any. This is a horrible accusation as I love to eat cookies (particularly chocolate chip), so I decided to investigate the cookies to figure out what happened. 

On Google Chrome, `Inspect -> Application -> Cookies -> http://litctf.live:31778` reveals a set of three cookies, including one named `likeCookie`. This is initially set as false, and setting it to true reveals the following message when the page is reloaded.
> Oh silly you. What do you mean you like a true cookie? I have 20 cookies numbered from 1 to 20, and all of them are made from super true authentic recipes.

Calling the cookie a "true cookie" after I set the value to "true" must have meant that the value should've actually been a number. Brute-forcing all the numbers from 1-20, the value `17` reveals that Kevin loves the 17th cookie too, and he gives us our tasty flag as a reward. Oh, and a cookie too which is tastier obviously.

## Flag

`LITCTF{Bd1mens10n_15_l1k3_sup3r_dup3r_0rzzzz}`

## Notes

We can also automate the last step, but 20 really isn't much to just brute-force:
```js
await (async () => { // IIFE (to use await) not necessary on Chrome
    for (let i = 1; i <= 20; ++i) {
        document.cookie = `likeCookie=${i}` // worst API in existence
        const content = await (await fetch("http://litctf.live:31778/")).text();
        if (!content.includes("It's not my favorite cookie though")) {
            return content;
        }
    }
})()
// ...<b>LITCTF{Bd1mens10n_15_l1k3_sup3r_dup3r_0rzzzz}</b>...
```
