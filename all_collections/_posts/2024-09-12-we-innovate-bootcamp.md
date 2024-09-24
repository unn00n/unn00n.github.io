---
layout: post
title: WE INNOVATE Bootcamp Learnings Summary
date: 2024-09-12 
thumbnail: /assets/images/2024-09-12-we-innovate-bootcamp/thumb.png
categories: [Bootcamp, Cybersecurity]
---
The **WE INNOVATE Bootcamp** was an intensive 5-week program focused on equipping students with a robust skill set in the fast-evolving field of cybersecurity. The program featured hands-on learning and collaborative projects, giving participants practical experience across multiple cybersecurity domains. Below is a detailed walkthrough of my journey and key contributions during the bootcamp.

---

### **Introduction to Cybersecurity**
The bootcamp began with a solid foundation in cybersecurity principles, covering essential topics such as **network security**, **Linux systems**, and **Active Directory management**. These fundamental skills provided the necessary context for understanding how to secure enterprise environments from cyber threats.

---

### **Linux Systems**
I enhanced my Linux skills through practical tasks like using the `man` command to navigate system documentation and tackling the **Bandit challenges**, which tested my ability to operate efficiently within a Linux environment. These challenges reinforced critical problem-solving abilities and provided a strong understanding of system administration—a cornerstone for cybersecurity professionals.

---

### **Active Directory (AD)**
Exploring **Active Directory** (AD) was a key part of my learning. I gained insights into critical AD components like:
- **Windows Event Forwarder (WEF)** for collecting and forwarding event logs.
- **Rights Management Services (RMS)**, which controls access to digital information.
- **Sysvol**, the shared directory used to store login scripts and policies.
This knowledge was essential for managing user permissions and access controls within large enterprise networks.

---

### **Network Security and Intrusion Detection**
I dove into various network security concepts, including configuring **Snort v3** as an Intrusion Detection System (IDS). Through hands-on projects, I created Snort rules to detect security threats in real-time. Additionally, I explored **Unified Threat Management (UTM)** systems, which offer a comprehensive defense against diverse cyber threats by consolidating multiple security features into one solution.

---

### **Security Operations Center (SOC) Analysis**
We covered **SOC analysis**, where I learned to detect and analyze incidents efficiently within a Security Operations Center. Using tools like **IBM QRadar** and **Elastic Stack**, I gained experience in real-time monitoring, threat detection, and incident response, which are central to maintaining cybersecurity within an organization.

---

### **SOC Engineering**
On the engineering side, I gained hands-on experience building and optimizing SOC infrastructure. I worked with security technologies such as the **Elastic Stack** (Elasticsearch, Kibana, Logstash, and Fluent Bit) and Docker to deploy scalable solutions for log analysis and threat detection.

---

### **Incident Response and Reporting**
Incident response was a significant focus of the bootcamp. I participated in two team-led incident reports, one involving a ransomware attack modeled on **LockBit 3.0**, and another concerning a stolen laptop. These exercises helped me understand the entire incident response process, from **detection** to **containment** and **resolution**, with a focus on distinguishing between short-term and long-term containment strategies.

Additionally, I learned to write comprehensive **Incident Response reports**—a crucial skill for documenting and responding to cyber threats.

---

### **SIEM Systems and Data Correlation**
Working with **SIEM** (Security Information and Event Management) systems like **IBM QRadar** was another key aspect of the bootcamp. I learned to:
- Configure **SIEM detection rules** to identify suspicious activities such as repeated failed login attempts.
- Apply **data enrichment** techniques to logs, improving the accuracy of threat detection and incident response.
- Use **Ariel Query Language (AQL)** to query time-series datasets, an essential skill in investigating cyber incidents.

I also explored QRadar’s core components, such as the **Custom Rules Engine (CRE)** for managing detection logic, the **Flow Collector** for analyzing network traffic, and the **Magistrate**, which generates and stores offenses in a PostgreSQL database.

---

### **Reverse Shells vs. Bind Shells**
As part of my exploration of penetration testing, I learned about **reverse shells**, which are often preferred over **bind shells** because they can bypass firewall restrictions, enhancing stealth during testing.

---

### **Web Security and Cross-Site Scripting (XSS)**
I gained hands-on experience with **Cross-Site Scripting (XSS)** vulnerabilities and learned about mitigation techniques such as **Content Security Policies (CSP)** to prevent the execution of malicious scripts in browsers. This knowledge was crucial for understanding web application security and defending against client-side attacks.

---

### **Network Penetration Testing and Phishing Detection**
Using tools like **CrackMapExec**, I explored network penetration testing to identify vulnerabilities within enterprise networks. I also honed my skills in detecting phishing attacks by analyzing HTTP **POST methods** and redirected URLs, which are common indicators of credential compromise.

---

### **Regular Expressions (Regex)**
I developed my ability to use **regular expressions (regex)** for pattern matching in security applications, especially when parsing raw logs in tools like **QRadar**. This skill was instrumental in efficiently extracting and analyzing data from various logs during security investigations.

---

### **Elastic Stack and Docker**
I took a hands-on approach to deploying and configuring the **Elastic Stack** both on **Docker** and on standalone environments. I worked with:
- **Elasticsearch** for indexing and searching logs.
- **Kibana** for visualizing and analyzing data.
- **Logstash** and **Fluent Bit** for forwarding and transforming logs.
These tools were crucial for building a robust security monitoring system that could scale effectively.

---

### **SOC Pyramid and Engineering**
We explored the **SOC Pyramid**, which categorizes roles into:
1. **Platform Engineering** for building and maintaining infrastructure.
2. **Detection Engineering** for creating detection mechanisms.
3. **SOC Operation Engineering** for managing incidents and alerts.

This hierarchical model helped me understand the technical and operational side of a SOC, as well as the roles required to keep an organization secure.

---

### **Collaboration and Teamwork**
Collaboration was a critical part of the bootcamp. I actively contributed to group projects, helping my peers troubleshoot technical issues and complete tasks. This collaborative spirit not only enhanced my learning but also strengthened my network within the cybersecurity community.

---

### **Hands-on Projects and Freelancing**
Throughout the bootcamp, we completed several practical projects, including:
- Configuring **Snort** as an IDS.
- Installing and setting up **Elastic** and **Kibana** for log monitoring.
- Detecting and analyzing multiple cyberattacks in simulated scenarios.
- Automating SOC processes using **Tines** to increase efficiency.

In the final week, we also covered topics like **freelancing**, **presentation skills**, and **business modeling** to round out our technical and professional development.

---

The **WE INNOVATE Bootcamp** provided an invaluable learning experience, helping me develop a strong foundation in cybersecurity, and offered numerous opportunities to apply my knowledge in real-world scenarios.

