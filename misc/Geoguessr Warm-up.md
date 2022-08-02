# misc/Geoguessr Warm-up

## Challenge

My friend told me to meet him here. Where is he???? The flag is LITCTF{latitude,longtitude} rounded to the third decimal place. (Example: LITCTF{42.444,-71.230})

[`geoguessrwarmup.png`](https://drive.google.com/file/d/1a0e58oRCqNVkFjqATku9xl9tNnvU43uP/view)

## Solution

As we did for all the images, we started out trying to use Google Lens to give us hints on the area. The architectural design was revealed to be Mediterranean, and most likely from Gibraltar. We decided to go to Gibraltar, and as you might expect just *drop a pin randomly*.

Weirdly enough, this helped us a lot. Dropping our pin on Street View at [16 Europa Road](https://www.google.com/maps/place/16+Europa+Rd,+Gibraltar+GX11+1AA,+Gibraltar/@36.1276738,-5.3511575,17.5z/data=!4m5!3m4!1s0xd0cbf742a521a93:0xbe01f8284411bf1c!8m2!3d36.1274967!4d-5.349288) revealed some places with awfully similar architectural designs. After moving around for a bit, we realized that going too far up made the house design change, so we reversed our search a bit.

After some more moving about and randomly dropping pins, we fnally found an area on `Naval Hospital Road` that looked super duper close to the given image. After some movement along the twisty road, we found [the place!](https://www.google.com/maps/place/43+Naval+Hospital+Rd,+Gibraltar+GX11+1AA,+Gibraltar/@36.1223839,-5.3501789,21z/data=!4m5!3m4!1s0xd0cbf752d6526f9:0x83839768e07e7cc!8m2!3d36.1222985!4d-5.3500645)

## Flag

`LITCTF{36.122,-5.350}`

## Notes

We could've solved this a lot faster but we did something really silly. The image mentioned `GH ARMS` which we falsely assumed was `HIGH ARMS`. If you searched up "GH Arms Gibraltar" you would find the territory's "Edinburgh Arms", and the **only** location was right where the flag was.
