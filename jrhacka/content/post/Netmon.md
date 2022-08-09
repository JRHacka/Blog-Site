+++
author = "Jake Rice"
title = "Who's That Netmon?!?!"
date = "2022-08-08"
description = ""
tags = [
    "HTB",
    "CTF_Walkthrough",
]

featureImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Legacy_logo.png"
thumbnail = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Legacy_logo.png"
shareImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/Legacy_logo.png"

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

While that is running let's go ahead and see if we can find anything at the IP. Open up a web browser and put in the target machines IP address. This time there is a website, and it's asking us for some credintials. Lets let our NMap scan finish and then we'll come back to this. 
```bash
NMAP RESULTS
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

Jumping back into anonymous ftp we are gonna head to the directory specified from that site. I found the following four interesting files, and grabbed them. From there I opened up each in Sublime, and if you aren't using Sublime you're wrong, and did some key word searching. I got lucky and had started with admin, and got a hit right away.


BUTTTTT it still didn't work. After banging my head on the wall I realized it was a .old document, and it had a year in it. I changed the year to 2019 and was in. Cash money it's time to explore this site.

---
## Exploring the Site

This website has a whole lot going on. More then I care to just snoop into, so let's see if there are any known exploits. Looking up this version of the PRTG Network Monitor I started to get a lot of great information. We get some results about a Metasploit exploit, and also some articles about using the notifications to send malicious commands. For the sake of getting better everyday let's see if we can figure out these notification and execute something from there. 

---
## Exploiting the Vulnerability (Command Injection)

Another incredible site https://tryhackme.com defines command injection as "the abuse of an application's behaviour to execute commands on the operating system, using the same privileges that the application on a device is running with." The notification portion of this site allows you to do some pretty interesting things, and that is going to be our home as we finish gaining administrator access to this box. 

---

## Walkthrough Video

{{< youtube 39tOHZCG7E0 >}}

<br>
