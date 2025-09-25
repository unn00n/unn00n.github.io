---
layout: post
title: "IEEE Victoris4 Finals - B0x Forensics Challenge"
date: 2025-09-25 
thumbnail: /assets/images/2025-09-25-B0x/2025-09-25-B0x.png
categories: [CTF, IEEE, Victoris4, Forensics]
---
![2025-09-25-B0x.png](/assets/images/2025-09-25-B0x/2025-09-25-B0x.png)

Peace be upon you. Here I will walk through how I solved this challenge and achieved First Blood.

The challenge provided a ZIP archive containing the following files:
![20250925141612.png](/assets/images/2025-09-25-B0x/20250925141612.png)

As a lazy geek, my usual approach to forensics challenges is to work smart, not hard. My first step was to focus on file modification timestamps. I listed all challenge files by their last modification date using the following Linux command:

```bash
find . -type f -printf '%T@ %p\n' | sort -nr | cut -d' ' -f2- > mod.txt
````

![20250925123525.png](/assets/images/2025-09-25-B0x/20250925123525.png)

I opened the `ActivitiesCache.db` file, which stores all user activities on Windows. Using **DB Browser for SQL**, I navigated through it to get a high-level view of the user’s activity.

![image.psd(3).png](/assets/images/2025-09-25-B0x/image.psd(3).png)

I also used **WxTCMD** from Eric Zimmerman’s tools, which can parse and translate the activity type column values:

![2025-09-25151306.png](/assets/images/2025-09-25-B0x/2025-09-25151306.png)

![2025-09-25151133.png](/assets/images/2025-09-25-B0x/2025-09-25151133.png)

From this, I discovered that the user had been using Notepad to view several text files, including `credi.txt` on the Desktop and others located under the `Notepad_content_` folder in Downloads.

To view the contents of these files, I searched through the challenge files for the path `C:\Users\Akaza\Downloads\`. However, I only found the `AppData` folder and NTUSER files under the `Akaza` user directory.

Next, I examined the second most recently modified file: `MFT`. I opened it with **MFT Explorer**, which revealed all the paths I needed. Using the hex viewer, I could inspect the contents of the relevant files:

![2025-09-25181028.png](/assets/images/2025-09-25-B0x/2025-09-25181028.png)

After checking through the contents, I eventually found the flag:

![2025-09-25181437.png](/assets/images/2025-09-25-B0x/2025-09-25181437.png)

When I submitted it, the system rejected it because it didn’t match the competition’s required flag format. I then reviewed the **ceo info** file, which mentioned confidentiality, and remembered that the challenge stated sensitive data had been stolen. Taking this into account, I located the correct hex value, submitted it in the required format, and it was accepted:

![2025-09-25181551.png](/assets/images/2025-09-25-B0x/2025-09-25181551.png)

Thanks to my team, we secured 3rd place in the IEEE Victoris4 CTF.
![2025-09-257.15.27.jpeg](/assets/images/2025-09-25-B0x/2025-09-257.15.27.jpeg)
