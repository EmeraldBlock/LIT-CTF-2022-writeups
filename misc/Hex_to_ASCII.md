# Hex to ASCII

**Chall:**
Do you know how to speak hexadecimal? I love speaking in hexadecimal. In fact, in HexadecimalLand, we like to say
`4c49544354467b74306f6c355f346e645f77336273317433735f6172335f763372795f696d70307274346e745f6630725f4354467d`

**Solution:**
As the name of the challenge suggests, this probably involves turning a hexadecimal string into a series of ASCII characters. We can use online Hex-to-String converters such as [this one](https://string-functions.com/hex-string.aspx), or use a language like Python to convert.
```Python
print(bytes.fromhex('4c49544354467b74306f6c355f346e645f77336273317433735f6172335f763372795f696d70307274346e745f6630725f4354467d').decode('utf-8'))
```

**Flag:**
`LITCTF{t0ol5_4nd_w3bs1t3s_ar3_v3ry_imp0rt4nt_f0r_CTF}`