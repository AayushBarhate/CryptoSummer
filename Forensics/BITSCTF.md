BITSCTF 

```Access Granted ```

![image-20240618143850918](C:\Users\Aayush\AppData\Roaming\Typora\typora-user-images\image-20240618143850918.png)

using the NTLM hash its as easy as putting it in crackstation to hopefully get the password 

![image-20240618143954558](C:\Users\Aayush\AppData\Roaming\Typora\typora-user-images\image-20240618143954558.png)

There the flag content is - ```adolfhitlerrulesallthepeople```



```0.69 Day ```

Most of the time CVE in memory dumps are related to applications like winrar or web browsers, I just need to find out the version of these programs to get the related CVE.

I memdumped the Winrar process with pid 8436 

using strings and grep i got the this : 

![image-20240619102550186](C:\Users\Aayush\AppData\Roaming\Typora\typora-user-images\image-20240619102550186.png)

 winrar version 6.22

search for CVE of this version and got a zero day exploit -  ```CVE-2023-38831```

```MogamBro's Guilty Pleasure```



```I'm wired in```

found a keylog.pcap file on the desktop of magambro's pc.

going through the file using wireshark it was a USB capture.

To get the logged keystrokes i needed the HID data parsed (found this after some googling)

i extracted the HID data using 

```bash
 tshark -r keylog.pcapng -Y "frame.len == 35 && !(usbhid.data == 00:00:00:00:00:00:00:00)" -T fields -e usbhid.data > capdata.txt
```



after going through multiple usb keyboard parsers and pulling my hair out i found out about 

[GitHub - syminical/PUK: A Python script that decodes USB keyboard packet capture hex data.](https://github.com/syminical/PUK)

i used this on the capdata.txt and got the flat 

```I haveebeen haakee  !!!
HELLMEE
BITSCTF{I_-7h1nk_th3y_4Re_k3yl0991ng_ME!}
```

