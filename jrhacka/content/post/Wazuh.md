+++
author = "Jake Rice"
title = 'Hozah for Wazuh'
date = "2022-09-08"
description = ""
draft = false
tags = [
    "Projects",
    "SIEM",
]

featureImage = "images/wazzy.png"
thumbnail = "images/wazzy.png"
shareImage = "images/wazzy.png"

+++
Everyone welcome to my Ted Talk. This post is primarily going to be a discussion on SIEM’s in general, and then on what my initial thoughts were on Wazuh. The installation instructions online are excellent, so I’m going to let you all use that instead of me just rewriting them. Please refer to the video if you do get stuck or have questions!
<!--more-->

 
 
### 1 – What is a SIEM? 
 
 
That’s an excellent question. Pronounced like sim, SIEM stands for:
 
```text

Security

Incident

Event

Management 
````
Essentially these solutions help to detect, analyze, and respond to security threats before they can harm the business/individual. According to What is SIEM? | Microsoft Security these solutions, “combine both security information management (SIM) and security event management (SEM) into one security management system.” Here is a list of the “TOP 11” best SIEM’s for 2022 Top 11 Best SIEM Tools in 2022 (Real-Time Incident Response & Security) (softwaretestinghelp.com). 

{{% notice warning "Opinion not Fact" %}}

The above list was created by an author with an opinion and should be taken as such. I just wanted you to have an idea of some enterprise level SIEM’s, and do not endorse that the list above is indeed the top 11 to choose from. Please do your own research before recommending one to your boss or purchasing one for your company. 

{{% /notice %}}


### Why Wazuh? 
 
 
I first learned about Wazuh when I attended the ShowME IT Summit on 09/01/2022. I absorbed so much incredible information, but this item really stuck with me. I decided to look into it more and see if it was an item worth exploring. 

Wazuh is a free open-source application that can be deployed via the cloud or on a physical device. The free aspect of it was key because I really wasn’t looking to pay. Because I wanted the practice and experience, the on-premises server aspect was very appealing. I grabbed an Ubuntu 22.04 ISO and a copy of my Windows 10 VM. I had to install a couple of different tools and packages for Ubuntu, but I kind of organically was taken to them by the installation process. 

{{% notice note "Note" %}}
If you need installation help, check the video!
{{% /notice %}}
 
 
### 3 – More to Come!
 
 
There were a ton of cool functionalities in the tool itself. The dashboard was an excellent balance of practical and appealing on the eyes. Each agent’s page will show compliance requirements for different standards, such as NIST or PCI DDS. There is a whole section diving into this machine and the MITRE ATT&CK framework. The security events page logs absolutely everything. All in all, a really cool and interesting SIEM that I am incredibly excited to explore more down the line! 


## Installation Video

{{< youtube pZ1ZvtGSv0E >}}

<br>

