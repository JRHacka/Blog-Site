+++
author = "Jake Rice"
title = "Jerry's Mom Has Got It Goin On'"
date = "2022-06-06"
description = ""
tags = [
    "HTB",
    "CTF_Walkthrough",
]
thumbnail = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Jerry_logo.png"

+++
Oh my how the clever titles continue. This is a box walkthrough of Jerry from HTB. Come and follow along as we discover the vulnerabilties, exploit the machine, and hack the box!

<!--more-->
---
## Startup

This is an entry level beginner freindley box, so the walkthrough is going to be the same. With that being said, the first step that other walkthoughs always skip over is spinning up the box and getting connected. While it is easy once you know how to do it if you don't know how you'll never be able to get started. Feel free to skip to the next section if you already know how to do this.

HTB has 2 options when it comes to completing their boxes. You can spin up a machine via HTB, or connect your own using OpenVPN. I prefer to connect my because of the customizations and different tools I have on my Kali machine, but if you want here is where to find the HTB Attack Box.

The first step if you are connecting via OpenVPN is to first make sure you have OpenVPN installed on your device. Either start typing the command see if it autofills, or if using Kali run:
```bash
sudo apt install openvpn
```
Once you are sure is installed in the same place where you see the attack box you can select OpenVPN. Change the server if desired and then download the file. Find the file location and run the following command:
```bash
openvpn insert/filepath/here
```

After you have connected you may have to refresh the page in order for the connect box to turn green and show your HTB IP. After that the last step is to find the Jerry box and start it up.
{{% notice note "Heads Up"%}}
That IP in the green box will be the IP you use for yourself during interactions with this box. Do not use your actual IP.
{{% /notice %}}

---

## Intelligence gathering

The first thing I always like to do is run NMap. Here is the NMap scan I ran and a breakdown of the flags I used.
```bash
nmap nmap -sT -sV -Pn -p- -A -T3 tar.get.ip.add -oA /home/kali/HTB/WU/Jerry/nmap/scans
```
{{% notice tip "NMAP Flags"%}}
-sT =

-sV = OS Version

-Pn = Scan all ports, and don't ping. (Scans prots no matter what)

-p- = All ports

-A = All

-T3 = Speed level. (Options 1-5 with 1 being slowest and 5 fastest)

-oA = output namp in the three formatss to the specified file path.
{{% /notice %}}

While that is running let's go ahead and see if we can find anything at the IP. Open up a web browser and put in the target machines IP address. We don't get any results, but we'll check a couple of the usual directories just in case. I searched around using /admin /login /user /signin /contact and got nothing, so lets dive in to those NMap results.
```bash
nmap -sT -sV -Pn -p- -A -T3 10.10.10.95 -oA /home/kali/HTB/WU/Jerry/nmap/scans
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-06 16:29 EDT
Nmap scan report for 10.10.10.95
Host is up (0.044s latency).
Not shown: 65534 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2012|2008|7|2016|Vista (91%)
OS CPE: cpe:/o:microsoft:windows_server_2012 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_vista::- cpe:/o:microsoft:windows_vista::sp1
Aggressive OS guesses: Microsoft Windows Server 2012 (91%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (91%), Microsoft Windows Server 2012 R2 (91%), Microsoft Windows 7 or Windows Server 2008 R2 (85%), Microsoft Windows Server 2008 R2 (85%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (85%), Microsoft Windows Server 2016 (85%), Microsoft Windows 7 (85%), Microsoft Windows 7 Professional or Windows 8 (85%), Microsoft Windows 7 SP1 or Windows Server 2008 SP2 or 2008 R2 SP1 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using proto 1/icmp)
HOP RTT      ADDRESS
1   42.93 ms 10.10.14.1
2   44.52 ms 10.10.10.95

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 140.14 seconds
```

The only port open is 808, so that makes our job abit esier. Lets learn more about port 808 and Apache Tomcat!

---

## Port 8080 - Apache Tomcat

