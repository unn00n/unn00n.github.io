---
layout: post
title: "IEEE Victoris4 Finals - B0x Forensics challenge Writeup"
date: 2025-09-25 
thumbnail: /assets/images/2025-09-25-B0x/2025-09-25-B0x.png
categories: [CTF, IEEE, Victoris4, Forensics]
---
![2025-09-25-B0x.png](/assets/images/2025-09-25-B0x/2025-09-25-B0x.png)

Peace on u, here I will show how I approached to solve this challenge as a First Blood

The challenge provide a zip file which contains the following files:
![20250925141612.png](/assets/images/2025-09-25-B0x/20250925141612.png)

As a lazy geek, My first move at solving any forensics challenge I try to work smart not hard as possible as I can, so my first focus is on the modification date I list all challenge files based on the last modification timestamp by this command in linux:

```bash
find . -type f -printf '%T@ %p\n' | sort -nr | cut -d' ' -f2->mod.txt
```

![20250925123525.png](/assets/images/2025-09-25-B0x/20250925123525.png)
I opened ActivitiesCache.db file which have all user activites on windows. Using **DB Browser for SQL** I navigated it and had a high level overview of what user was doing.

![image.psd(3).png](/assets/images/2025-09-25-B0x/image.psd(3).png)
I could use also WxTCMD from Eric Zimmerman tools which can parse and translate the activity type column numbers:

![2025-09-25151306.png](/assets/images/2025-09-25-B0x/2025-09-25151306.png)

![2025-09-25151133.png](/assets/images/2025-09-25-B0x/2025-09-25151133.png)
So what I got so far is the user was using notepad to view some text files those file are credi.txt under Desktop path and some others under "Notepad_content_" folder under Downloads path.

I wanted to see the content of those files so I checked the challenge files for this path "C:\Users\Akaza\Downloads\" but what I found was only AppData folder and NTUSER files  under user Akaza.

Now is the turn of the second modified file which is `MFT`.  I opened it by MFT Explorer tool Where I found all the paths I needed and found the files and can see the contents of them in the hex viewer:
![2025-09-25181028.png](/assets/images/2025-09-25-B0x/2025-09-25181028.png)
I checked the content of the files until I found the follwing flag:
![2025-09-25181437.png](/assets/images/2025-09-25-B0x/2025-09-25181437.png)
 I submitted it but it was not following the Competition/challenge flag so I checked the **ceo info** file which indicates its confidentiality also the challenge says there were a sensitive data stolen so I toke this into consideration, I found this hex, submitted it in the flag format and it is correct :)

![2025-09-25181551.png](/assets/images/2025-09-25-B0x/2025-09-25181551.png)

Thanks to my team we have saved the 3rd place in IEEE Victoris4 CTF
![2025-09-257.15.27.jpeg](/assets/images/2025-09-25-B0x/2025-09-257.15.27.jpeg)
