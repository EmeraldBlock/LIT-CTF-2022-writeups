# misc/Geoguessr Hard Mode

## Chall
Where am I now? The flag is LITCTF{latitude,longtitude} rounded to the third decimal place. (Example: `LITCTF{42.444,-71.230}`)
[`geoguessrhard.png`](https://drive.google.com/file/d/1oVUhlXkBoLpBupbRxNiFn6emqNF50Xeq/view)

## Solution
There were several important details to note in the picture. The building on the right had the phrase `Hôtel de Ville` in blue, which is French for "city hall". There were two locations in the picture that had flags with a green-yellow-red color scheme. There was also a kiosk with a white and orange arrow on it, which we later found to be the company "Orange Money".

Orange Money primarily operates in Africa, so searching for African countries that primarily speak French and have a green-yellow-red color scheme gives the following:
- Benin
- Burkina Faso
- Cameroon
- Congo
- Guinea
- Mali
- Senegal
- Togo

Following the order of green-yellow-red, this narrows it to Mali, Guinea, and Senegal. The picture also had a very faded 2017 copyright on it, implying the image was on Street View.
![image](https://user-images.githubusercontent.com/64328893/181108745-5492613b-8d86-469f-9c5b-ccdbd7dbbe37.png)

Welp, we still have three decently-big countries, what are we supposed to do now? [This YT video](https://www.youtube.com/watch?v=v7IgkogkAZc) one of our teammates found (I'm not LexMACS so that's not a rickroll by the way :D) helped with some insight on how to search. Going back to the Street View bit from earlier, we realized a lot of Senegalese cities had street view.

*Cue the random pin drops* And after looking through Street View for a bit, we found the exact building!!



Also, big brain moment predicting the country two days before we actually solved it o:
<img width="1250" alt="image" src="https://user-images.githubusercontent.com/64328893/181109502-4c24e3f6-6052-44c2-b5c6-b3b5d6827bb1.png">

## Flag
`LITCTF{13.775,-13.669}`
