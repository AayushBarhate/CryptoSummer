https://tryhackme.com/r/room/win64assembly

This Room was basically watered down assembly crash course. Well I guess it revised assembly for me



https://tryhackme.com/r/room/windowsapi



Windows API and how its implemented

How DLLs are imported and used in C/C++ 

This was interesting 

```csharp
using System;
using System.Runtime.InteropServices;

public class Program
{
...
private static extern int MessageBox(IntPtr hWnd, string lpText, string lpCaption, uint uType);
} 
```

they use extern keyword to inform the runtime of the specific DLL that was previously imported, and use it directly in the script as a managed method.

API call structures was interesting af , didn't know they use struct to define parameters for API calls



Overall this was something new and interesting, will be looking into API calls in free time now :>



PE analysis 

I think i have gone through this sometime during a CTF challenge - [TryHackMe | Dissecting PE Headers](https://tryhackme.com/r/room/dissectingpeheaders)