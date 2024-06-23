```ShunyaCTF```

```Check Research and Check Again```

Given was a try.png image which was corrupted.

Used pngcheck to diagnose the corrupted part

![image-20240620103227377](C:\Users\Aayush\AppData\Roaming\Typora\typora-user-images\image-20240620103227377.png) 

it was the IHDR interlace method, after researching a bit I found out it should be either 1 or 0 but here it was 82.

Opening the image in 010 editor, it showed multiple CRC errors 

![image-20240620103423186](C:\Users\Aayush\AppData\Roaming\Typora\typora-user-images\image-20240620103423186.png)

I edited the hex of the image to correct the CRC mismatch and at the same time edited the Interlace method from 82 to 0.

Here is the final flag :

![corrupted_flag](C:\Users\Aayush\Desktop\summer.tar\summer\shunya\corrupted_flag.png)