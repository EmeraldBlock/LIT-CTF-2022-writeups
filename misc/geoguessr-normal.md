# misc/Geoguessr Normal

## Chall
I like the city! The flag is LITCTF{latitude,longtitude} rounded to the third decimal place. (Example: `LITCTF{42.444,-71.230}`)
[`geoguessrnormal.png`](https://drive.google.com/file/d/1I5HJIy0HMBpYlohuJdjeITiYlIw5s1Y0/view)

## Solution
Inspecting the image, we see that a life preserver on the side that is written in both Chinese and Portuguese (though *some* brick on our team thought it was Spanish and just ignored it). Cool history lessons tell us this means the location is most likely somewhere in Macau.

Next we decided to locate the skyscraper with the red light on top of it. After some googling, we figured out this was Macau's [Bank of China Building](https://en.wikipedia.org/wiki/Bank_of_China_Building,_Macau). Searching the nearby area for other buildings in the image, such as the one in the far distance, we found a series of resorts that were close to the area. These include the [Mandarin Oriental](https://www.google.com/maps/place/Mandarin+Oriental,+Macau/@22.1788309,113.5440728,1015a,35y,9.94h,48.22t/data=!3m1!1e3!4m15!1m6!3m5!1s0x34017aeea5871d33:0x2e687410a1298fb6!2sTorre+Lago+Panoramico!8m2!3d22.1886798!4d113.5421489!3m7!1s0x34017aea57a182cf:0xe3a50d9c8b80a7a2!5m2!4m1!1i2!8m2!3d22.1846053!4d113.5469971!5m2!1e4!1e1) and the [MGM Macau](https://www.youtube.com/watch?v=j5a0jTc9S10&list=PL3KnTfyhrIlcudeMemKd6rZFGDWyK23vx&index=9). Panning around the area near the [Av. View of Lake Nam Van](https://www.google.com/maps/place/Av.+View+of+Lake+Nam+Van/@22.1872794,113.5383505,234a,35y,9.94h,48.54t/data=!3m1!1e3!4m12!1m6!3m5!1s0x34017aeea5871d33:0x2e687410a1298fb6!2sTorre+Lago+Panoramico!8m2!3d22.1886798!4d113.5421489!3m4!1s0x34017aee6d9f1e09:0x571de57561f5eb65!8m2!3d22.1894831!4d113.5388269!5m2!1e4!1e1), we search along the coast until we find the life preserver that was previously mentioned.

Also, the 3d view on this looks really cool
![image](https://user-images.githubusercontent.com/64328893/181102518-f8859de6-81b8-4bec-9620-9d5a4401372a.png)

## Flag
`LITCTF{22.189,113.539}`
