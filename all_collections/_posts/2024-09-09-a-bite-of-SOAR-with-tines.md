---
layout: post
title: A Bite of SOAR with Tines & IBM Qradar
date: 2024-09-09 
thumbnail: /assets/images/2024-09-06-a-bite-of-SOAR-with-tines/thumb.png
categories: [SOC Engineering, Setup, SIEM, SOAR]
---
# Introduction
SOAR (Security Orchestration, Automation, and Response) tools are designed to streamline and automate
security operations, making it easier to respond to security incidents. Tines is a popular SOAR tool
that offers a user-friendly interface and a wide range of integrations with various security tools.
In this post, we'll explore how to get started with Tines and set up a basic workflow

# Practical Exapmle with IBM Qradar
1- Retrieve all destination ips<br>
2- Check them with virustotal<br>
3- Send email (report) by the malicious ones<br>
I've created a diagram of the required workflow:
![Workflow Diagram](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/workflow-diagram.png)

Tines comes with large and great templates that we can use:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/templates.png)
let's search for "qradar"<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/search-qradar.png)<br>
add the "Qradar" template to the workflow:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/ibm-qradar.png)<br>
there are 15 templates available we can check one of them that we might need, we want to do search on IBM Qradar so we can search for "search"<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/ibm-qradar-create-search.png)<br>
we have found a all related templates we will use "Submit Ariel Query in QRadar", click on it <br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/search-template.png)
we can use Editor so we can see the full Json script and edit from there or we can edit each fields here:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/search-template-editor.png)
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/editor-search.png)
replace `<<RESOURCE.qradar_domain>>` with the QRadar domain and edit `query_expression` I used this:
```
SELECT destinationIP FROM flows GROUP BY destinationIP limit 100 start '2024-09-01 12:00:00' stop '2024-09-08 12:00:00'
```
this query will return the destination ips from the last 7 days, you can change the query to
suit your needs

under `basic_auth` enter your credential on your QRadar, my final script is:<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/submit-aql-script.png)

**Note**
*   Make sure to test the query in QRadar before running it in Tines
*   I used `flows` here that related to Network activity in my accessed QRadar
*   I added `disable_ssl_verification` is related to my QRadar self-signed certificate.<br>
Let's test this:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/test-submit-aql.png)<br>
open body and scroll to find `search_id` 
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/test-submit-aql-searchid.png)
now we want to get the results by:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/get-search.png)
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/get-aql.png)
configure it
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/get-aql.png)
# Conclusion
In this post, we've explored how to get started with Tines and set up a basic workflow
Tines offers a user-friendly interface and a wide range of integrations with various security tools
It's a powerful tool for streamlining and automating security operations
# Future Posts
In future posts, we'll dive deeper into the features and capabilities of Tines
We'll explore how to use Tines to automate common security tasks, such as incident response and
threat hunting
# References
[1] Tines Documentation: https://docs.tines.io/
[2] Tines Blog: https://blog.tines.io/
