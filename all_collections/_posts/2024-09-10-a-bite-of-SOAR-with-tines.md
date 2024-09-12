---
layout: post
title: A Bite of SOAR with Tines & IBM Qradar
date: 2024-09-10 
thumbnail: /assets/images/2024-09-06-a-bite-of-SOAR-with-tines/thumb.png
categories: [SOC Engineering, Setup, SIEM, SOAR, Tines, QRadar]
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
# 1.0 Submit Search
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
## 1.1 Configuring "*Submit Ariel Query in QRadar*"
we can use Editor so we can see the full Json script and edit from there or we can edit each field here:
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
# 2.0 Retrieve Results
now we want to get the results:

![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/get-search.png)<br>
## 2.1 Configuring "*Get Ariel Query Results in QRadar*"
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/get-aql.png)<br>
configure URL by setting the cursor after `searches/` till `+` button appear then click it and add value<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/plus-value.png)
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/value-search.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/body-search.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/body-select.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/searchid-select.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/get-search-id.png)<br>
**Note**
*   Make sure runinng `Submit Ariel Query in QRadar` one time at least then connect the two elements.<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/run-once.png)<br>
*   You can put `search_id` you got from `Submit Ariel Query in QRadar`.
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/test-submit-aql-searchid.png)
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/id-call.png)
*   For automating we created a formula to call `search_id` field from the `Submit Ariel Query in QRadar` element.
Do not forget to fill `Basic auth` with your credentials
## 2.2 Testing 
Now we can test:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/test-get-aql.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/test-get-aql-results.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/test-results.png)<br>
# 3.0 Extract Destination IPs
Now we got Destination IPs to extract valuse we use `Event Transform` <br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/event-transform.png)<br>

![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/default-event-transform.png)<br>
## 3.1 Configuring "*Event Transform*"
based on Tines docs [event-transformation](https://www.tines.com/docs/actions/types/event-transformation/) we use `explode` and set path to `get_ariel_query_results_in_qradar.body.flows`<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/edited-event-transform.png)<br>
## 3.2 Connect & Run
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/3-connect.png)<br>
Check `Event Transform` Events<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/check-events.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/destinationsips.png)<br>
# 4.0 Search For IPs
Now we have Destination IPs, To search for them in Virus Total we check virustotal templates

![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/search-for-virus.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/search-ip.png)<br>
Create an account on [VirusTotal](https://www.virustotal.com/gui/join-us) and get your API key:
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/api-virus.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/copy-api.png)<br>
## 4.1 Configuring "*VirusTotal - Search for IP Address*"
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/virus-total-conf.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/ip-add-virus.png)<br>
you will have connect button just click it and follow the easy setup

![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/4-connect.png)<br>
check Events

![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/virus-event.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/total-votes.png)<br>
# 5.0 Is IP Malicious?
Now we have results from Virus Total, we can check if IP is malicious or not. We use `Trigger` to put a condition
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/trigger-action.png)<br>
## 5.1 Configuring "*Trigger*"
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/trigger-conf.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/trigger-rule.png)<br>
We checked that if the `malicious` equals 1 from `total_votes` retrieved from VirusTotal we will trigger the action
# 6.0 Alerting
Now we have a condition to trigger an action, we can use `send email` to send an alert email
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/send-email.png)<br>
## 6.1 Configuring "*send email*"
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/email-conf.png)<br>
I added time to subject by `DATE("now")` formula and malicious IP by `search_for_ip_address.body.data.id`
# 7.0 Run
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/full.png)<br>
## 7.1 Checking emails
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/check-email.png)<br>
![Tines Templates](/assets/images/2024-09-06-a-bite-of-SOAR-with-tines/email-content.png)<br>
# Conclusion
In this blog post, we have demonstrated how to integrate QRadar with VirusTotal using Tines. We have created a workflow that submits an Ariel query to QRadar, retrieves the results, extracts the Destination IPs, and then uses VirusTotal to check if the IP is malicious. If it is, we trigger an action to send an email to the security team.

This is a basic example and can be extended to include more features, such as checking multiple IPs or
including additional information in the email.

# References
[1] Tines Documentation: [https://docs.tines.io/](https://docs.tines.io/)

[2] Tines Blog: [https://blog.tines.io/](https://blog.tines.io/)
