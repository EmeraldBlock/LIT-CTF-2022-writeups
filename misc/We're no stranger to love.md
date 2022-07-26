# misc/We're no stranger to love

## Challenge

OMG, the LIT Music Bot is playing my favorite song on the voice channel! Why does it sound a bit off tho?

## Solution

When we went to the voice channel, we heard some [music](https://www.youtube.com/watch?v=dQw4w9WgXcQ), but it was weirdly distorted in some places. There was clearly some useful information stored in these distortions, so the first step was to record the music coming from the bot so we could analyze it as we pleased. After a few torturous minutes of listening, we finally got [a recording](https://drive.google.com/file/d/1Mjm8Jru8NPBu60XjYO72EvyygRQ7h6lz/view?usp=sharing) of the song from start to finish.

The first thing to notice is that there are three types of sound effects: long distortions, short distortions, and amogus sound effects.

The second thing to notice is that at about 1:31 in the audio file, a voice says "Left curly bracket." At around 4:00, a voice says "Right curly bracket." Also, after the "right curly bracket," there are no more distortions.

All of this suggests that the flag is encoded into the distortions using [Morse code](https://en.wikipedia.org/wiki/Morse_code). A long distortion represents a `-`, a short distortion represents a `.`, and an amogus sound effect is used to separate the letters. Indeed, when we transcribe the distortions before the "left curly bracket," we get

```
.-.. .. - -.-. - ..-.
```

which corresponds to `LITCTF`!

Now, we can just transcribe the part between the braces:

```
.-. .. -.-. -.- .. ..-. .. . -..
```

and we can get the flag.

## Flag

`LITCTF{RICKIFIED}`
