---
layout: post
title: Custom Retention Policy Using Elastisearch API
date: 2024-09-23 
thumbnail: /assets/images/2024-09-23-custom-retention-policy/thumb.png
categories: [Tines Story Development, SOC Engineering, Setup, SIEM, Elastic]
---
## Overview
In this tutorial, we will cover how to create a custom retention policy using the Elasticsearch API. This
will be useful for SOC engineers who want to manage their data retention in a more flexible way.
## Prerequisites
#### **Index Selection:**
Let's assume the index for this task is called `my_fluent`.
![img1](/assets/images/2024-09-23-custom-retention-policy/1.png)
### **Use Tines Story Development**
Here’s how to complete the task using **Tines**:
- **Create a Story in Tines**
- **Actions in Tines:**
	1. **Fetch Index Size (HTTP Request Action)**
	2. **Threshold Check (Trigger Action)**
	3. **Send Email (Email Action)**
	4. **Delete Old Logs (HTTP Request Action)**

Here is a diagram of the required workflow:
![img1](/assets/images/2024-09-23-custom-retention-policy/2.png)

---
### **Action 1: Fetch Index Size**
Create an HTTP Request Action in Tines to get the index size from Elasticsearch using the `_stats` API.
**Reference**: [Elasticsearch: Indices Stats API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-stats.html)

![img1](/assets/images/2024-09-23-custom-retention-policy/3.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/4.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/5.png)

### **Action 2: Threshold Check**
Use a **Trigger Action** in Tines to compare the fetched size of the index to the threshold (e.g., 1 MB).

![img1](/assets/images/2024-09-23-custom-retention-policy/6.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/7.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/8.png)

If the threshold is exceeded, proceed to delete logs or take further action.

### **Action 3: Send Email**
Create an **Email Action** in Tines to send an email report with the details of the index, size, and timestamp.

![img1](/assets/images/2024-09-23-custom-retention-policy/9.png)

You can open html editor and viewer by click the highlighted icon:
![img1](/assets/images/2024-09-23-custom-retention-policy/10.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/11.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/12.png)

**The results:**
![img1](/assets/images/2024-09-23-custom-retention-policy/13.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/14.png)

### **Action 4: Delete Old Logs**
If the index size exceeds the threshold, you can use another **HTTP Request Action** in Tines to delete logs that are older than a certain number of days using the Elasticsearch `Delete By Query API`.
**Reference**: [Elasticsearch: Delete By Query API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete-by-query.html)

![img1](/assets/images/2024-09-23-custom-retention-policy/3.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/15.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/16.png)

This will delete all logs older than 7 days.

**Response:**

![img1](/assets/images/2024-09-23-custom-retention-policy/17.png)

### **Full Workflow Structure:**
Here is the workflow diagram for how the actions connect:

![img1](/assets/images/2024-09-23-custom-retention-policy/18.png)

**Validate operations:**

![img1](/assets/images/2024-09-23-custom-retention-policy/19.png)

Update workflow by adding "Pre-Deletion" and "Post-Deletion" `Send Email Action` if "Threshold" exceeded :

![img1](/assets/images/2024-09-23-custom-retention-policy/20.png)


---
### **Use Python Script**
To implement this task using **Python**, we'll write a script that interacts with Elasticsearch via its API to:
1. **Fetch the index size**.
2. **Check if the size exceeds a threshold**.
3. **Send an email report**.
4. **Delete old logs if the threshold is exceeded**.

```python
import requests
import smtplib
from email.mime.text import MIMEText
from datetime import datetime
from requests.auth import HTTPBasicAuth

# Elasticsearch configuration
ES_URL = "https://localhost:9200" 
INDEX = "my_fluent"
THRESHOLD_MB = 5  
ES_USERNAME = "elastic"
ES_PASSWORD = "password"

# Gmail SMTP Configuration
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587
SMTP_USERNAME = "example@gmail.com"
SMTP_PASSWORD = "apppassword"  
EMAIL_FROM = "Ali"
EMAIL_TO = ["recipient@example.com"]


def fetch_index_size(index):
    url = f"{ES_URL}/{index}/_stats/store"
    try:
        response = requests.get(url, auth=HTTPBasicAuth(ES_USERNAME, ES_PASSWORD), verify=False)  # Disable SSL certificate verification
        response.raise_for_status()
        data = response.json()
        size_in_bytes = data['indices'][index]['total']['store']['size_in_bytes']
        size_in_mb = size_in_bytes / (1024 * 1024)  # Convert to MB
        return size_in_mb
    except requests.exceptions.RequestException as e:
        print(f"Error fetching index size: {e}")
        return None


def send_email_report(index_name, size_in_mb, timestamp):
    subject = "Task 2: Index Size Report"
    body = f"""
    Index Name: {index_name}
    Current Size: {size_in_mb:.2f} MB
    Checked at: {timestamp}
    """
    msg = MIMEText(body)
    msg["Subject"] = subject
    msg["From"] = EMAIL_FROM
    msg["To"] = ", ".join(EMAIL_TO)

    try:
        with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
            server.starttls()
            server.login(SMTP_USERNAME, SMTP_PASSWORD)
            server.sendmail(EMAIL_FROM, EMAIL_TO, msg.as_string())
        print("Email report sent successfully.")
    except Exception as e:
        print(f"Failed to send email: {e}")


def delete_old_logs(index, days=7):
    url = f"{ES_URL}/{index}/_delete_by_query"
    query = {
        "query": {
            "range": {
                "@timestamp": {
                    "lt": f"now-{days}d/d"
                }
            }
        }
    }
    try:
        response = requests.post(url, json=query, auth=HTTPBasicAuth(ES_USERNAME, ES_PASSWORD), verify=False)
        response.raise_for_status()
        result = response.json()
        print(f"Deleted {result['deleted']} documents older than {days} days.")
    except requests.exceptions.RequestException as e:
        print(f"Error deleting old logs: {e}")


def main():
    # Fetch index size
    index_size = fetch_index_size(INDEX)
    
    if index_size is None:
        print("Failed to fetch index size. Exiting.")
        return
    
    # Get the current timestamp
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    # Send the email report
    send_email_report(INDEX, index_size, timestamp)

    # Check if the size exceeds the threshold
    if index_size > THRESHOLD_MB:
        print(f"Index size exceeds the threshold of {THRESHOLD_MB} MB.")
        # Delete old logs if the threshold is exceeded
        delete_old_logs(INDEX, days=7)
    else:
        print(f"Index size is within the threshold: {index_size:.2f} MB.")


if __name__ == "__main__":
    main()

```
To send emails using **Gmail's SMTP server** in the Python script, you'll need to configure Gmail's SMTP settings and update the `SMTP_SERVER`, `SMTP_PORT`, and authentication variables in the script. Gmail uses **OAuth2** for secure authentication, but you can also use an **App Password** if you have **2-Step Verification** enabled for your Google account.
### **Use an App Password**:

If your account uses **2-Step Verification**, you must create an **App Password**:

- Go to Google Account Security.
- Under "Signing in to Google," enable **2-Step Verification** if it's not enabled.
- Then, generate an **App Password**.
![img1](/assets/images/2024-09-23-custom-retention-policy/21.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/22.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/23.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/24.png)
Use this App Password instead of your regular Gmail password in the script.

I added my credentials to the script and executed it:
![img1](/assets/images/2024-09-23-custom-retention-policy/25.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/26.png)

I tried on another index `.ds-metrics-system.network-default-2024.09.05-000001`:

![img1](/assets/images/2024-09-23-custom-retention-policy/27.png)

I updated the script to send "Pre-Deletion" and "Post-Deletion" emails:

```python
...
    # Send the email report with the current index size
    initial_email_subject = "Task 2: from pyscript"
    initial_email_body = f"""
    Index Name: {INDEX}
    Current Size: {index_size:.2f} MB
    Checked at: {timestamp}
    """
    send_email_report(initial_email_subject, initial_email_body)

    # Check if the size exceeds the threshold
    if index_size > THRESHOLD_MB:
        print(f"Index size exceeds the threshold of {THRESHOLD_MB} MB.")
        
        # Send email before deleting
        pre_deletion_subject = "Task 2: Pre-Deletion Notification"
        pre_deletion_body = f"""
        Index Name: {INDEX}
        Current Size: {index_size:.2f} MB
        Action: Deleting logs older than 7 dayss.
        """
        send_email_report(pre_deletion_subject, pre_deletion_body)
        
        # Delete old logs if the threshold is exceeded
        deleted_docs = delete_old_logs(INDEX, days=7)
        
        if deleted_docs is not None:
            # Send email after deleting
            post_deletion_subject = "Task 2: Post-Deletion Report"
            post_deletion_body = f"""
            Index Name: {INDEX}
            Logs older than 7 dayss have been deleted.
            Documents deleted: {deleted_docs}
            Current Size: {index_size:.2f} MB
            Checked at: {timestamp}
            """
            send_email_report(post_deletion_subject, post_deletion_body)
        else:
            print("Error occurred during deletion. No email sent for post-deletion.")
    else:
        print(f"Index size is within the threshold: {index_size:.2f} MB.")


if __name__ == "__main__":
    main()

```
**Run:**
![img1](/assets/images/2024-09-23-custom-retention-policy/28.png)

**Results:**

![img1](/assets/images/2024-09-23-custom-retention-policy/29.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/30.png)
![img1](/assets/images/2024-09-23-custom-retention-policy/31.png)

---

In this Post, we have demonstrated how to implement a custom retention policy for Elasticsearch indices using Tines and Python. We have also shown how to automate the process of sending emails to notify security team before and after deleting logs.


