# misc/Rythm's Double Identity

## Challenge

It is known that Rythm is an unparalleled prodigy at competitive programming. Anybody who dares challenge instantly pulverizes into mere dust. But just as the old adage goes, 无敌是多么的寂寞~. After days of constantly getting INFINITE positive rating changes on the Competitive Programming platform CodeForces as [Superj6](https://codeforces.com/profile/superj6), he decides to adopt a secret double identity. She was conceived in Rythm's own image, and the only person who could compete with Rythm.
<br>
<br>
It is known that Rythm competed in the majority of CodeForces contests starting from July 2019, but sometimes as his second identity. Find his 2nd most used account for contests, and wrap the username (case-sensitive) in `LITCTF{}` to obtain the flag

## Solution

With a whole load of CF users, where in the world do we even start?? We can use some of our own CF knowledge and the problem statement to give us some aid. Codeforces has an API, so we can use Python requests to automate our process significantly.

The first bit of insight is that since this is Rythm's alt, the handle most likely does not have a contest in common with SuperJ6.

Since this is SuperJ6's alt, we can also set reasonable bounds about where the account might be rating-wise. We can say that the account most likely has a rating no lower than 1600, and probably isn't significantly higher than SuperJ6's max rating. As his max is 2319, we can say the range `[1600, 2400]` is probably reasonable.

Lastly we have to realize that if SuperJ6 started in June 2019 and competed in the majority of contests since July 2019, the alt account most likely didn't even exist before then.

Now came the hard part (mainly because we've never even worked with APIs before): coding. The Python code here was used to generate all the info we needed:
```py
import requests
import json
import time
f = open("woot.txt", "w") #cache for the users in case something breaks

superj6orz = json.loads(requests.get('https://codeforces.com/api/user.rating?handle=SuperJ6').content.decode())['result']
ratedlist = json.loads(requests.get('https://codeforces.com/api/user.ratedList?activeOnly=false&includeRetired=false').content.decode())['result']

contests = set() #all the contests superj6 has participated in
for d in superj6orz:
	contests.add(d['contestId'])
print(len(contests))

users = [] # users with rating in the interval [1600, 2400]
for d in ratedlist:
	if d['rating'] >= 1600 and d['rating'] <= 2400:
		# print(d['handle'])
		users.append(d['handle'])
print(len(users))

valid = [] #people in 'users' that have not participated in a contest w/ superj6

for i in range(len(users)):
	while True:
		try:
			s = 'https://codeforces.com/api/user.rating?handle=' + users[i]
			query = json.loads(requests.get(s).content.decode())['result']
			same = False #this will be true if 'users[i]' has participated in the same contest as jason
			old = True #this will be true if 'users[i]'s last contest was before jason's first
			
			for d in query:
				same |= d['contestId'] in contests
				old &= d['contestId'] <= 1183
				
			if not same and not old:
				valid.append(users[i])
				f.write(users[i] + '\n')
				f.flush()
			time.sleep(0.5)
			break
		except:
			pass
print(len(valid))
```

`contests` is a set that contains the ID for every contest SuperJ6 participated in, and `users` contains all users in the rating interval `[1600, 2400]`. Looping through each handle in `user`, we confirm that the handle does not have a contest in common with SuperJ6 (the variable `same`) or a contest before late June 2019 (the variable `old`). If `old` and `same` are both false, the handle is added to the list `valid` and cached into a file in case of program breakdown.

Lastly was finding the handle itself, which we started about halfway through the program running, from a list of ~500 users. After some scrolling, the account [MaddyBeltran](https://www.youtube.com/watch?v=dQw4w9WgXcQ) caught our eye. The account name was famliar to one of our members, and the name "Maddy" matched the fact the account was referred to as a "she" in the problem statement. With some luck on our side, we confirmed the flag as...

## Flag

`LITCTF{MaddyBeltran}`
