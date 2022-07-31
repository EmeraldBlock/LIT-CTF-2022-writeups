# misc/kirby!!!

## Challenge

Kirby is so cool! (Wrap your flag in LITCTF{})
<br>
<br>
The beginning is very loud so you should turn down your volume.
<br>
<br>
https://vocaroo.com/12wR27kejDYj
<br>
<br>
Original song: Green Grounds from Kirby Mass Attack

## Solution

Often in challenges involving an audio file (especially ones with a section that sounds broken), there is a message hidden in the spectrogram view of the file.

After downloading the file and putting it into [Audacity](https://www.audacityteam.org/) (or an online spectrogram viewer), we can click the track name on the left and change the viewing method from "Waveform" to "Spectrogram." Then, zooming in on the first few seconds of the track, we find this:

![spectrogram view of track showing characters FLAG: K1RBY1SCOOL!](kirby-spectrogram.png)

## Flag

`LITCTF{K1RBY1SCOOL!}`
