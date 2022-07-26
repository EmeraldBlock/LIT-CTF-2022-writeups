# web/Personal Website

## Chall
I am Esther Man, a brilliant student at Lexington High School. Welcome to my [personal website](http://litctf.live:31777/) <3 <3 <3

## Solution
Opening the link gives us a very menacing site, not because the infrastructure looks tough to break, but because of the gyrating amogus. Anywho, if we attempt to scroll down the page we realize there's an infinite scroll that prevents us. Inspecting the HTML reveals a paragraph tag at the bottom that reveals the first third of the flag: `LITCTF{E5th3r_M4n`.

Next we try looking at other parts of the website's design, and in sources we're able to access the CSS and JS files. These files have the second and third part of the flag respectively hidden in comments: `_i5_s0_OTZ_0TZ_OFZ_4t_` and `3v3ryth1ng_sh3_d03s}`. Combining the three thirds gets us our flag >:D

## Flag
`LITCTF{E5th3r_M4n_i5_s0_OTZ_0TZ_OFZ_4t_3v3ryth1ng_sh3_d03s}`