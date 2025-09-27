<img width="1901" height="910" alt="2025-09-27214014" src="https://github.com/user-attachments/assets/cddb4ca5-592c-4ada-b88e-c3885e892c0e" />---
layout: post
title: "IEEE Victoris 4.0 Finals - B0x Forensics Challenge"
date: 2025-09-25 
thumbnail: /assets/images/2025-09-25-B0x/2025-09-2500-24-23.png
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

When I submitted it, the system rejected it because it didn’t match the competition’s required flag format. I then reviewed the **ceo info** file, which indicates something important about ceo and remembered that the challenge stated sensitive data had been stolen. Taking this into account, I noticed the hex value between the curly brackets in it, submitted it in the required format, and it was accepted:

![2025-09-25181551.png](/assets/images/2025-09-25-B0x/2025-09-25181551.png)

---

### Illusion

![2025-09-2500-49-07.png](/assets/images/2025-09-25-B0x/2025-09-2500-49-07.png)

The challenge provided an `.ad1` image that, when opened in FTK Imager, showed the following:
![2025-09-27154220.png](/assets/images/2025-09-25-B0x/2025-09-27154220.png)

I started by examining `ActivitiesCache.db` and parsing prefetch files to get a high-level view of system activity:
![2025-09-27193352.png](/assets/images/2025-09-25-B0x/2025-09-27193352.png)
![2025-09-27192353.png](/assets/images/2025-09-25-B0x/2025-09-27192353.png)

Prefetch timeline:
![prefetch2025-09-27191638.png](/assets/images/2025-09-25-B0x/prefetch2025-09-27191638.png)

Next, I exported files to list them by last modification timestamp:
![2025-09-2716-28-52.png](/assets/images/2025-09-25-B0x/2025-09-2716-28-52.png)

From this, I observed a WinRAR installation and an execution of `dllhost.exe` from the Public user’s Documents folder (an unusual location). I submitted the `dllhost.exe` file to VirusTotal and inspected its strings:
![2025-09-27214014.png](/assets/images/2025-09-25-B0x/2025-09-27214014.png)
![2025-09-26004139.png](/assets/images/2025-09-25-B0x/2025-09-26004139.png)

The binary appeared related to Mesh Agent (remote management), which suggested it might have been used when the challenge image was created rather than being part of the actual challenge activity. I paused work during the competition and continued later.

To understand what happened between the WinRAR installation and the `dllhost.exe` execution, I parsed the `$MFT` and focused on entries near that timeframe:
![](/assets/images/2025-09-25-B0x/2025-09-28000717.png)
![](/assets/images/2025-09-25-B0x/2025-09-28000810.png)
![2025-09-280010536.png](/assets/images/2025-09-25-B0x/2025-09-280010536.png)

The timeline showed WinRAR installation completion, execution of `dllhost.exe`, then MeshAgent activity, and finally a write of `winrar.dll` into the WinRAR installation folder. I extracted `winrar.dll`, inspected its strings, found an encoded candidate, decoded it with ROT13 in CyberChef, and recovered the flag:
![](/assets/images/2025-09-25-B0x/2025-09-25004100.png)
![](/assets/images/2025-09-25-B0x/2025-09-27154220.png)
![](/assets/images/2025-09-25-B0x/2025-09-25003952.png)

---

### SilentByte

![](/assets/images/2025-09-25-B0x/2025-09-2800-37-39.png)

This challenge was straightforward. The image contained the following files:
![](/assets/images/2025-09-25-B0x/2025-09-28004726.png)

I opened the image in FTK Imager and navigated the file system. The challenge description mentioned patching applications; I confirmed Notepad++ was installed and discovered a `sysbackup` folder under `ProgramData`:
![](/assets/images/2025-09-25-B0x/2025-09-28004821.png)

`sysbackup` contained several notable binaries. I inspected strings until I found the flag in plaintext inside `system_patch.exe`:
![](/assets/images/2025-09-25-B0x/2025-09-28004929.png)

---

### FindEvil

![](/assets/images/2025-09-25-B0x/2025-09-2800-58-43.png)

This challenge provided three web log files covering three consecutive days. I used ChatGPT to extract all POST requests from the logs and output them into a CSV for review. From that CSV I located the last web-shell and recovered the flag:
![](/assets/images/2025-09-25-B0x/2025-09-28010308.png)

Those are all the forensics challenges from the final competition.

Thanks to my team, we secured 3rd place in the IEEE Victoris 4.0 CTF.
![2025-09-257.15.27.jpeg](/assets/images/2025-09-25-B0x/2025-09-257.15.27.jpeg)
