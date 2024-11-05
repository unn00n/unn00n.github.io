---
layout: post
title: Forensics Tips & Tools
date: 2024-08-01 
thumbnail: /assets/images/2024-08-01-forensics-tips/thumb.jpeg
categories: [CTF]
---

#### Audio Steganography
Audio steganography is a technique used to hide information within audio files. A common tool for this is stegolsb and wavsteg.

Command Example:
```bash
stegolsb wavsteg -r -i noise.wav -o file.txt -b 200
```
#### PDF Analysis Tools
PDF files can contain hidden data, malicious macros, or embedded objects. Here are some essential tools for analyzing PDFs:

- **qpdf:** Manipulate and inspect PDF files.
- **PDFStreamDumper:** Analyzes PDF streams for hidden content.
- **pdfinfo:** Provides metadata about the PDF.
- **pdfcrack:** A password cracker for PDF files.
- **pdfimages:** Extracts images from a PDF file.
- **pdfdetach:** Extracts embedded files from PDFs.
- **pdf-parser.py -v:** Parses and inspects PDFs.
- **pdftotext:** Converts PDF files to plain text.
- **peepdf:** Analyzes PDF objects (-if object for inspection).
- **pdfid:** Detects JavaScript and embedded files in PDFs.
- **file:** Identifies file types using magic numbers.

#### File Header Analysis
File headers are crucial in identifying and analyzing file types. Use the following tips to inspect headers:

- **Hex Editors:** Tools like HXD (for Windows) can inspect file headers.
- **Corrupt Files:** Compare the header of a corrupt file to a non-corrupt file using a hex editor.
- **Bitmap Width/Height:** Use exiftool to identify the image dimensions. Then, use Python's hex() function to convert decimal to hex (consider endianness).

#### Network Traffic Analysis
**Common Plaintext Protocols:**
- HTTP: Credentials or cookies are transmitted in plaintext over HTTP.
- Telnet: A legacy protocol that transmits data (including credentials) unencrypted

#### Microsoft Office Files & Macros
Microsoft Office files like PowerPoint (PPT), Excel, and Word are often compressed archives and can contain malicious macros.

- **Macros in Malware:** Often used in malware delivery through Office documents.
- **oletools:** A set of Python tools for analyzing OLE (Object Linking and Embedding) files.
    - **olevba:** Extracts VBA macros from Office documents.
```bash
olevba -c /path/to/Document  # Extract macros
```

#### Password Hashes in Linux
Linux typically stores passwords in encrypted, one-way hash formats. Some common formats include:

- **$1$: MD5**
- **$2a$: Blowfish**
- **$5$: SHA-256**
- **$6$: SHA-512**

#### File Format Notes
- **Packet Tracer Files (.pkz):** These are Cisco Packet Tracer extensions and can be opened using tools like 7-zip as they are considered archives.
- **Microsoft Office Files (PPT, Excel):** Often ZIP archives and can be analyzed similarly.

To be continued...