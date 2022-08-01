# misc/Geoguessr Normal Mode

## Challenge

I like the city! The flag is LITCTF{latitude,longtitude} rounded to the third decimal place. (Example: LITCTF{42.444,-71.230})

[`geoguessrnormal.png`](https://drive.google.com/file/d/1I5HJIy0HMBpYlohuJdjeITiYlIw5s1Y0/view)

## Solution

Inspecting the image, we see that a life preserver on the side that is written in both Chinese and Portuguese (though *some* brick on our team thought it was Spanish and just ignored it). Cool history lessons tell us this means the location is most likely somewhere in Macau.

Next we decided to locate the skyscraper with the red light on top of it. After some googling, we figured out this was Macau's [Bank of China Building](https://en.wikipedia.org/wiki/Bank_of_China_Building,_Macau). Since the image is by the water, we pan around the area near the [Av. View of Lake Nam Van](https://www.google.com/maps/place/Av.+View+of+Lake+Nam+Van/@22.1872794,113.5383505,234a,35y,9.94h,48.54t/data=!3m1!1e3!4m12!1m6!3m5!1s0x34017aeea5871d33:0x2e687410a1298fb6!2sTorre+Lago+Panoramico!8m2!3d22.1886798!4d113.5421489!3m4!1s0x34017aee6d9f1e09:0x571de57561f5eb65!8m2!3d22.1894831!4d113.5388269!5m2!1e4!1e1). After locating other buildings in the image, most notably the set of buildings on the right, and also the foliage in the water, we look around in Street View and find a section by the water with the same type of wall. Nearby, we locate the life preserver that was previously mentioned.

Also, the 3d view on this looks really cool
![image](https://user-images.githubusercontent.com/64328893/181102518-f8859de6-81b8-4bec-9620-9d5a4401372a.png)

## Flag

`LITCTF{22.189,113.539}`
