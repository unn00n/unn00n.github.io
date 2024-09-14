---
layout: post
title: WE INNOVATE Bootcamp Learnings Summary
date: 2024-09-12 
thumbnail: /assets/images/2024-09-12-we-innovate-bootcamp/thumb.png
categories: [Bootcamp, Cybersecurity]
---
WE INNOVATE Bootcamp was a 5-week intensive program that brought together students from various
backgrounds to learn and innovate in the field of cybersecurity. The program was designed to equip
students with the skills and knowledge needed to succeed in the ever-evolving cybersecurity landscape.

During the **WE INNOVATE Bootcamp**, I expanded my knowledge and skills across various cybersecurity disciplines, while also building strong relationships and collaborating with my peers. Here's a comprehensive walkthrough of my learning journey and contributions.

#### **Linux Systems**
I began by enhancing my skills in **Linux** through hands-on practice and research. The use of the `man` command was a pivotal resource for understanding system documentation. The **Bandit challenges** further tested my ability to navigate the Linux environment and reinforced my problem-solving skills.

---

#### **Active Directory (AD)**
My exploration of **Active Directory** introduced me to key components like:

- **Windows Event Forwarder (WEF)** for collecting and forwarding event logs.
- **Rights Management Services** that regulate access to digital information through rights tied to actions.
- **Sysvol**, a critical shared directory used for storing policies and login scripts.

This knowledge is essential for managing and securing enterprise networks, especially in maintaining access controls and user permissions.

---

#### **Regular Expressions (Regex) and Shells**
I delved into the use of **regex** for pattern matching, particularly in security applications. A specific focus was on **reverse shells**, which are often favored over **bind shells** due to their ability to bypass firewall restrictions, enhancing stealth during penetration testing.

---

#### **Network Security**
The bootcamp provided insights into various network security concepts and tools, including:

- **Snort v3**: I learned to create **Snort rules** for detecting potential security threats. Snort's ability to act as an intrusion detection and prevention system was highlighted in several hands-on exercises.
- **Unified Threat Management (UTM)**: I explored UTM systems and how they generate security alerts, which serve as a comprehensive defense against various types of cyber threats.

---

#### **Incident Response and Reporting**
A major focus of the bootcamp was on **Incident Response**:

- I participated in two team-led **incident reports**: one focused on a **ransomware attack**, specifically **LockBit 3.0**, inspired by a real-world scenario, and another on a **stolen laptop** incident.
- The ransomware report was particularly impactful, as it allowed us to simulate the incident response process in a structured manner, from detection to resolution.

I also deepened my understanding of incident response concepts such as **short-term containment vs. long-term containment**, a critical distinction when addressing active threats.

---

#### **SIEM Systems and Data Correlation**
Working with **SIEM systems** like **IBM QRadar** introduced me to:

- **SIEM detection rules** and their ability to correlate events, such as identifying multiple failed login attempts within a short period.
- **Data enrichment**, which adds additional context to logs, making detection and response more accurate.

I became familiar with **Ariel Query Language (AQL)** for querying time-series datasets in QRadar, and explored how **Device Support Modules (DSMs)** enable integration of various security devices into SIEM systems.

---

#### **Elastic Stack and Docker**
I took a hands-on approach to installing the **Elastic Stack** (Elasticsearch, Kibana, Elastic Agent with Fleet server, Logstash, and Fluent Bit) both on **Docker** and without it. This experience improved my understanding of:

- **Elasticsearch** for indexing and searching logs.
- **Kibana** for visualizing and analyzing the data.
- **Logstash** and **Fluent Bit** for log forwarding and transformation.

This setup was crucial for building a robust security monitoring environment. It also provided valuable experience in deploying and configuring complex, containerized infrastructures.

---

#### **Web Security and XSS**
I learned about **Cross-Site Scripting (XSS)** vulnerabilities and explored mitigation techniques, such as **Content Security Policies (CSP)**, which prevent the execution of malicious scripts in browsers. I gained hands-on experience with **XSS payloads** and learned how to detect and block such attacks through security headers.

---

#### **Network Pentesting and Phishing**
Using tools like **CrackMapExec**, I conducted network penetration testing to identify vulnerabilities in enterprise networks. I also explored various techniques for detecting **phishing attacks**, such as analyzing POST methods and redirected URLs, which helped in identifying compromised credentials.

---

#### **SOC Engineering and Pyramid**
I learned about the **SOC Pyramid**, which categorizes roles in a **Security Operations Center** into three levels:

1. **Platform Engineering**: Responsible for building and maintaining the infrastructure.
2. **Detection Engineering**: Focused on creating detection mechanisms.
3. **SOC Operation Engineering**: In charge of managing alerts and incidents.

This hierarchical model helped clarify the structure of SOCs and the different roles within them.

---

#### **Collaboration and Teamwork**
Beyond the technical knowledge, one of the most valuable experiences was collaborating with my peers. I contributed significantly by helping others resolve technical issues, both in individual tasks and group projects. This involvement not only enhanced my own learning but also helped me build meaningful connections within the cybersecurity community.
