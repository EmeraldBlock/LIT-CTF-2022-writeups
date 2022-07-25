# misc/Rythm's Double Identity

**Chall:**
It is known that Rythm is an unparalleled prodigy at competitive programming. Anybody who dares challenge instantly pulverizes into mere dust. But just as the old adage goes, 无敌是多么的寂寞~ (Invicibility is lonely). After days of constantly getting INFINITE positive rating changes on the Competitive Programming platform CodeForces as [Superj6](https://codeforces.com/profile/superj6), he decides to adopt a secret double identity. She was conceived in Rythm's own image, and the only person who could compete with Rythm.

It is known that Rythm competed in the majority of CodeForces contests starting from July 2019, but sometimes as his second identity. Find his 2nd most used account for contests, and wrap the username (case-sensitive) in `LITCTF{}` to obtain the flag

**Solution:**
With a whole load of CF users, where in the world do we even start?? We can use some of our own CF knowledge and the problem statement to give us some aid. Codeforces grants us access to their API calls, so we can use Python requests to automate our process significantly.


The first bit of insight is that since this is Rythm's alt, the handle most likely does not have a contest in common with SuperJ6.

Since this is SuperJ6's alt, we can also set reasonable bounds about where the account might be rating-wise. We can say that the account most likely has a rating no lower than 1600, and probably isn't significantly higher than SuperJ6's max rating. As his max is 2319, we can say the range `[1600, 2400]` is probably reasonable.

Lastly we have to realize that if SuperJ6 started in June 2019 and competed in the majority of contests since July 2019, the alt account most likely didn't even exist before then.


Now came the hard part (mainly because I've never even worked with APIs before): coding. The Python code [here](https://pastebin.com/YKRQ824a) was used to generate all the info we needed.

`contests` is a set that contained the ID for every contest SuperJ6 participated in, and `users` contained all users in the rating interval `[1600, 2400]`. Looping through each handle is `user`, we confirmed that the handle did not have a common contest with SuperJ6 (the variable `same`) and wasn't created prior to late June 2019 (the variable `old`). If `old` and `same` were both false, the handle was added to the list `valid` and cached into a file in case of program breakdown.


Lastly was finding the handle itself, which we started about halfway through the program running from a list of ~500 users. After some scrolling, the account [MaddyBeltran](https://www.youtube.com/watch?v=dQw4w9WgXcQ) caught our eye. The account name was famliar to one of our members, and the name "Maddy" matched the fact the account was referred to as a "she" in the problem statement. With some luck on our side, we confirmed the flag as...


**Flag:**
`LITCTF{MaddyBeltran}`
