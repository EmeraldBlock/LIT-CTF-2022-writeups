# web/Among Us

## Challenge

Hello! I am Polopopy, and my friends like to call me Ryan. I have an unhealthy ~~fetich~~obsession with Among Us, so I made [this website](http://litctf.live:31779/) to demonstrate my unyielding enthusiasm!

## Solution

Viewing the HTML code of the website doesn't reveal much. The only thing that might be of interest is the [sussy-yellow-amogus](http://litctf.live:31779/sussy-yellow-amogus) gif file, but the flag isn't in there either (and this is a web challenge, not a steganography challenge).

However, if we press F12 and go to the `Network` tab, we can see the request the site makes for the [sussy-yellow-amogus](http://litctf.live:31779/sussy-yellow-amogus) file. Viewing the `Response Headers` section of this request, we can see this:

```
accept-ranges: bytes
cache-control: public, max-age=0
date: Mon, 25 Jul 2022 18:34:45 GMT
etag: W/"4d917-1820ef91815"
keep-alive: timeout=5
last-modified: Mon, 18 Jul 2022 01:43:01 GMT
sussyflag: LITCTF{mr_r4y_h4n_m4y_b3_su55y_bu7_4t_l3ast_h3s_OTZOTZOTZ}
x-powered-by: Express
```

## Flag

`LITCTF{mr_r4y_h4n_m4y_b3_su55y_bu7_4t_l3ast_h3s_OTZOTZOTZ}`

## Notes

We spent a while thinking the flag was hidden in the gif itself. What saved us was a comment that mentioned how they too were not doing web on a web challenge!
