---
layout: post
title: "Set-Up Elastic Stack: Elasticsearch, Kibana and  Elastic Agent"
date: 2024-09-08 
thumbnail: /assets/images/2024-09-08-elastic-stack/thumb.png
categories: [SOC Engineering, Setup, SIEM]
---
# **Introduction**
Elastic Stack, commonly known as the ELK Stack (Elasticsearch, Logstash, Kibana, and Beats), is a powerful suite of tools for searching, analyzing, and visualizing log and time-series data. The most commonly used combination today is Elasticsearch, Kibana, and Elastic Agent (which has replaced the need for Beats in many cases). These three components work together to provide scalable search, data indexing, and real-time insights. In this walkthrough, we will go through the steps involved in setting up Elasticsearch, Kibana, and Elastic Agent.
# **Set Up Elasticsearch**
### Add Server GPG key
```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
```
### Update sources lists
```
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
```
### Install Elasticsearch
```
sudo apt update
sudo apt install elasticsearch
```
## Configure Elasticsearch
Elasticsearch’s configuration file is in `/etc/elasticsearch/elasticsearch.yml`
To make Elasticsearch listens on all interfaces and bound IPs  uncomment `network.host` and replace its value with `localhost`
### Start and Run Elasticsearch
Start the Elasticsearch service:
```
sudo systemctl start elasticsearch
```
Enable Elasticsearch to start up every time the server boots:
```
sudo systemctl enable elasticsearch
```
Check running by sending an HTTP request:
```
curl -X GET "localhost:9200"
```
![Curl HTTP Request](/assets/images/2024-09-08-elastic-stack/curl-request.png)
# Set Up Kibana
We have already added the Elastic package source above so we can install by just `apt`:
```
sudo apt install kibana
```
### Start and Run Kibana
Start the Kibana service:
```
sudo systemctl start kibana
```
Enable Kibana to start up every time the server boots:
```
sudo systemctl enable kibana
```

Open firefox then write [localhost:5601]
![Elasticsearch Token Enrollment](/assets/images/2024-09-08-elastic-stack/elastic-token.png)
The `elasticsearch-create-enrollment-token` command creates enrollment tokens for Elasticsearch nodes and Kibana instances.
```
cd /usr/share/elasticsearch
sudo bin/elasticsearch-create-enrollment-token --scope kibana
```
![Generate Token](/assets/images/2024-09-08-elastic-stack/generate-token.jpg)
Enter the token then wait
![Verification Code](/assets/images/2024-09-08-elastic-stack/verification-code.png)
based on the popup run:
```
cd /usr/share/kibana
sudo bin/kibana-verification-code
```
![Generate Verification Code](/assets/images/2024-09-08-elastic-stack/verification-code-gen.jpg)
enter the generated verification code
![Configureing kibana](/assets/images/2024-09-08-elastic-stack/configure-kibana.png)
When the set up completes, it displays the login page.
![Kibana Login Page](/assets/images/2024-09-08-elastic-stack/login-kibana.png)
to get  `elastic` user new password :
```
cd /usr/share/elasticsearch
sudo bin/elasticsearch-reset-password -u elastic
export ELASTIC_PASSWORD="your_auto_generated_password"
```
![Reset Password for elastic user](/assets/images/2024-09-08-elastic-stack/reset-password.jpg)
![Kibana Login Page Filled](/assets/images/2024-09-08-elastic-stack/login-filled.png)
![Welcome Page](/assets/images/2024-09-08-elastic-stack/welcome-elastic.png)
![Kibana Home Page](/assets/images/2024-09-08-elastic-stack/elastic-homepage.png)
# **Set Up Elastic Agent**
### Set Up Fleet Server
![Fleet from Kibana](/assets/images/2024-09-08-elastic-stack/Kibana-fleet.png)
![Fleet Page](/assets/images/2024-09-08-elastic-stack/fleet-page.png)
![Add Fleet Server](/assets/images/2024-09-08-elastic-stack/add-fleet.png)
Name the fleet server and specify the host url with port 8220
![Add Fleet Server Filled](/assets/images/2024-09-08-elastic-stack/add-fleet-filled.png)
![Install Fleet Agent](/assets/images/2024-09-08-elastic-stack/install-fleet-agent.png)
we can use the same host of elasticsearch
![Confirmation](/assets/images/2024-09-08-elastic-stack/wait-confirmation.png)
![Installed Fleet Server](/assets/images/2024-09-08-elastic-stack/installed-fleet.png)
### Add Agents
I have installed another virtual machine (ubuntu server) to install elastic agent and retrieve logs from the machine 
![Add Agent](/assets/images/2024-09-08-elastic-stack/add-agent.png)
![Create Policy](/assets/images/2024-09-08-elastic-stack/create-policy.png)
![Enroll in Fleet](/assets/images/2024-09-08-elastic-stack/enroll-in-fleet.png)
![Install Elastic Agent](/assets/images/2024-09-08-elastic-stack/install-agent-1.png)
![Install Elastic Agnet](/assets/images/2024-09-08-elastic-stack/install-agent-2.png)
On ubuntu server I followed the installation guide then the last command tweaked it to skip certificate signed by unknown authority error by adding `--insecure`:
```
sudo ./elastic-agent install --url=https://10.0.2.15:8220 --enrollment-token=your_generated_token --insecure
```
![Wait Confirmation](/assets/images/2024-09-08-elastic-stack/agent-wait-confirmation.png)
![Done Confirmation](/assets/images/2024-09-08-elastic-stack/agent-done-confirmation.png)
![Installed Agents](/assets/images/2024-09-08-elastic-stack/fleet-page-agents.png)
![Discover Tab Checking installed agent](/assets/images/2024-09-08-elastic-stack/discover-check-agent.png)
By following these steps, we made it and installed Elastic Stack.
You can check the [Set-up Fluent-Bit]({{ site.baseurl }}{% post_url 2024-09-09-fluent-bit-pipeline %})
 If you want to advance.
