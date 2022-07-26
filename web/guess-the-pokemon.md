# web/Guess The Pokemon

## Chall
Have you heard the new trending game? [GUESS THE POKEMON](http://litctf.live:31772/)!!! Please come try out our vast database of pokemons.
[`guess-the-pokemon.zip`](https://drive.google.com/file/d/1Mpxl7ZsbTj0W9a8HfkEcjXC2mUjxNFQW/view?usp=sharing)

## Solution
Looking at the python code in `main.py`, we find some interesting tidbits that could be of use.
```Python
cur.execute('''DROP TABLE IF EXISTS pokemon''')
cur.execute('''CREATE TABLE pokemon (names text)''')
cur.execute(
    '''INSERT INTO pokemon (names) VALUES ("[FLAG REDACTED]") '''
)
#...
cur.execute("SELECT * FROM pokemon WHERE names=" + name + "")
```
These look like SQL calls, and SQL + Web = Injection >:) Inputting `x OR 1=1` such that `x` is some number (like say 105), we nail our flag!

## Flag
`LITCTF{flagr3l4t3dt0pok3m0n0rsom3th1ng1dk}`