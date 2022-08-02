# misc/Geoguessr Hard Mode

## Challenge

Where am I now? The flag is LITCTF{latitude,longtitude} rounded to the third decimal place. (Example: LITCTF{42.444,-71.230})

[`geoguessrhard.png`](https://drive.google.com/file/d/1oVUhlXkBoLpBupbRxNiFn6emqNF50Xeq/view)

## Solution

There were several important details to note in the picture. The building on the right had the phrase `Hôtel de Ville` in blue, which is French for "city hall". There were two locations in the picture that had flags with a green-yellow-red color scheme. There was also a kiosk with a white and orange arrow on it, which we later found to be the company "Orange Money", which primarily operates in Africa.

Searching for African countries that primarily speak French and have a green-yellow-red (in that order) color scheme gives Mali, Guinea, and Senegal.

The picture also had a very faded 2017 copyright on it, implying the image was from Street View.
![close-up with faint text "©2017 Google"](./Geoguessr%20Hard%20Mode/copyright.png)

Welp, we still have three decently-big countries, what are we supposed to do now? But going back to the Street View bit from earlier, we realized only Senegal had street view.

*Cue the random pin drops* And somehow, dropping in Tambacounda, we found the exact building!!

Also, big brain moment predicting the country two days before we actually solved it o:
![WalrusRamen listing countries. "my bet is on senegal, idk why"](./Geoguessr%20Hard%20Mode/prediction.png)

## Flag

`LITCTF{13.775,-13.669}`

## Notes

We took quite a while on this problem, much longer than the other GeoGuessr ones. We tried seeing if we could locate the place based on the Orange Money kiosk, but the kiosk is generic and although there is [a search tool](https://www.orange.ma/Agences-Orange?service=orange+money), it only covers Northwest Africa.

We only spotted the Street View copyright much later on, after realizing it was Senegal. (Apparently it was supposed to have been cropped out anyway!) Before then, we neglected to consider the Street View constraint until one of our teammates found [this YT video](https://www.youtube.com/watch?v=v7IgkogkAZc) (I'm not LexMACS so that's not a rickroll by the way :D), which mentioned the limited Street View in Africa, and the drop in Senegal looked similar to the environment in the image.

Also, apparently, with the right angle, Google Lens finds the building. Google Lens too OP, gotta use it more
