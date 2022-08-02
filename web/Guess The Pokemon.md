# web/Guess The Pokemon

## Challenge

Have you heard the new trending game? [GUESS THE POKEMON](http://litctf.live:31772/)!!! Please come try out our vast database of pokemons.

[`guess-the-pokemon.zip`](https://drive.google.com/file/d/1Mpxl7ZsbTj0W9a8HfkEcjXC2mUjxNFQW/view)

## Solution

Looking at the python code in `main.py`, we see it uses SQL. Here's the main code:
```py
# ...
        name = request.form['name']

        if ("'" in name or "\\" in name or '"' in name): # Uh-oh, character filter
          return render_template('login.html', error="no quotes or backslashes:)")
        elif (name == "names"): # can't inject "names=names"
          return render_template('login.html', error="you are wrong :<")
			
        try:
          cur.execute("SELECT * FROM pokemon WHERE names=" + name + "") # Injection!!! weird, it doesn't wrap name in quotes...
        except:
          render_template('login.html', error="you are wrong :3")
				
		
				
        rows = cur.fetchall()

        
        if len(rows) > 0:
            return render_template('login.html',
                                   error="Correct! The poekmon was " +
                                   rows[0][0]) # it'll tell us what we SELECTed! (not just a yes/no response)
# ...
```
So we just need to `SELECT` the "poekmon" (ie the flag) and it'll tell us what it is! Normally we can use the typical `' OR 1=1 --`, but in fact they forgot to wrap our input in quotes! (it does `names=abcde` instead of `names='abcde'`)

`'' OR 1=1` (which would evaluate `names='' OR 1=1` -> `False OR True` -> `True`) won't work since it contains quotes, and `names` (`names=names` -> `True`) is explicitly matched against. Instead, we can use a number instead of an empty string, like `105 OR 1=1`. Or, we can just do <code>names&nbsp;</code> with a space at the end! And with that, we nail our flag.

## Flag

`LITCTF{flagr3l4t3dt0pok3m0n0rsom3th1ng1dk}`
