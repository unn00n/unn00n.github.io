---
layout: post
title: Forensics Tips & Tools
date: 2024-08-01 
thumbnail: /assets/images/post1.png
categories: [CTF]
---

# Audio Steganography

```bash
stegolsb wavsteg -r -i noise.wav -o file.txt -b 200
```

# **PDF**

1. qpdf
2. PDFStreamdumper
3. pdfinfo
4. pdfcrack
5. pdfimages
6. pdfdetach
7. pdf-parser.py -v <file>
8. pdftotext
9. peepdf -if <filename>object <value>
10. pdfid

# file

Notes:

1. Look at the header to identify what type of file it is (HXD for Windows).
2. Compare the header of the corrupt file to a non-corrupt file (HXD for Windows).
3. Change the bitmap width and height (exiftool tool to identify the width and height, python hex function to convert decimal to hex, keep in mind that you might have to convert it from big to little endian or the other way around).
4. The challenge name usually is a hint.

# wireshark
Two common protocols that are transmitted in plain text are **HTTP** and **Telnet**.

# ppt is(zip)

*or excel and microsoft offfice files.*

## Macros

*used in malwares in microsoft office.*

### oletools

```bash
olevba -c /path/to/Document #Extract macros
```

# Linux

Password (typically encrypted in a one-way hash format) such as:

- $1$ is MD5
- $2a$ is Blowfish
- $5$ is SHA-256
- $6$ is SHA-512

# .pkz

*Cisco Packet Tracer Extension*

Could be considered as an archive, open with 7-zip.

To be continued...