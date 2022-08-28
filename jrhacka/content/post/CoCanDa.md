+++
author = "Jake Rice"
title = "CoCanDa Forever"
date = "2022-08-27"
description = ""
draft = false
tags = [
    "BTLO",
    "CTF_Walkthrough",
]

featureImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/CoCanDaLogo.png"
thumbnail = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/CoCanDaLogo.png"
shareImage = "https://raw.githubusercontent.com/JRHacka/Blog-Site/main/jrhacka/static/images/CoCanDaLogo.png"

+++
It was just a matter of time before a marvel reference came in to play... Follow along with me as we dive into a challenge from Blue Team Labs Online. New website and new fun content as we break apart an email from an alien hostage taker!  

<!--more-->
---
## Startup

This new website is super user friendly. There is both a paid and free version, but as far as I can tell the free version offers ton of content. With that being said, create an account and then navigate to the challenge "The Planet's Prestige" located here:

{{% notice note "Link to Challenge"%}}
https://blueteamlabs.online/home/challenge/the-planets-prestige-e5beb8e545
{{% /notice %}}

The challenge description is super fun, and if you want to hear me say CoCanDa like Wakanda go check out the video! The first step is to grab the zip file that is included with the challenge. Get organized and make a BTLO directory and move this zip file to the CoCanDa folder inside of that directory. Go ahead and unzip that file using:
```bash
unzip filename
```
and lets checkout the 'A Hope to CoCanDa.eml' file we get. 

---

## Intelligence Gathering

I left this part here for the sake of the standard format I've been using in my blog post, but there is not a lot of intelligence to gather in this case. You can run file on the 'A Hope to CoCanDa.eml' but really we see that it is a .eml file from the extension on the end. That extension is the electronic mail extension, aka email, so we need to bust that baby open. I could not find a great tool to open it in Kali, but you can run strings on it and get a lot of good info. I do that in the video, but it is just as easy to go straight to throwing it in some eml analyzer. To best follow along here is the one I found and used during this ctf:
{{% notice note "Link to EML Analyzer"%}}
https://eml-analyzer.herokuapp.com/
{{% /notice %}}
Go ahead and throw that file in there and click the analyze button, and then before we start dissecting that go ahead and scroll all the way to the bottom, download the attachment, and get it moved to your CoCanDa folder. We'll check it out more later, but for now let's see what answers we are able to find. 

---
## Question 1: What is the email service used by the malicious actor? 

Combing through the results from EML Analyzer we can see that there is a domain in the from address called microapple.com, but that gives us a failed answer. We then see in our security headers section that the authentication results gave us an spf=fail meaning that the IP address is not authorized to send from that domain... and yes I had to research what that meant. 
{{% notice tip "FYI"%}}
Below you will find the steps I took to complete this specific question, but it does not work. I confirmed these steps are correct and am therefore going to share them with you, but if you are following along you will not get the correct answer. I beleive the steps below no longer produce the right answer because this is a retired challenge. 
{{% /notice %}}

If you run nslookup on the IP that will give you the domain emkei.cz and after submitting that to the site we know it is the correct domain.
```bash
nslookup 93.99.104.210
```
{{% notice warning "ANSWER" %}}
emkei.cz
{{% /notice %}}


---
## Question 2: What is the Reply-To email address? 

This is one of those that you just can't over think. The answer is not the from address like you could assume, but instead it is literally the reply-to address. You can find that in the EML Analyzer right below the other headers section and it is called reply-to.
{{% notice warning "ANSWER" %}}
negeja3921@pashter.com
{{% /notice %}}


---
## Question 3: What is the filetype of the received attachment which helped to continue the investigation?

When you look at the attachment you pulled from the EML Analyzer it would appear the answer is .pdf. Things are not as they appear though because when you put that in the site it gives us a wrong answer. Go ahead and just run file on that though, and we quickly see things are not as they appear to be. 
```bash
file PuzzleToCoCanDa.pdf
```
{{% notice warning "ANSWER" %}}
negeja3921@pashter.com
{{% /notice %}}


---
## Question 4: What is the name of the malicious actor? 

This one was a little tricky, because the obvious answer was Bill Jobs from the sender address, but that doesn't work. We can see a name in the reply-to email, but it's not the attackers whole name or it needs broken up somehow. Let's unzip that PuzzleToCoCanDa.pdf and check out what is in it. We get three items from the zip file.
```text
1) Daughters Crown

2) Good Job Major

3) Money.xls
```

The daughters crown appears to just be photo proof that they do indeed have the princess, and nothing really jumps out. Before we dive to into it, we can check the "Good Job Major" pdf file. When you open this file up in Kali nothing appears obvious at first. Go to file in the top left corner, and then select properties so we can look at the properties of the document. That is where we find the answer to this question as it has an author’s name listed.
{{% notice warning "ANSWER" %}}
Pestero Negeja
{{% /notice %}}

---
## Question 5: What is the location of the attacker in this Universe?

There is no other useful information in our EML Analyzer or the PDF properties for this question, so let's check out the Money.xls file before we look more into the "Daughter Crown" file. When your open up that excel document you get two sheets, sheet 1 and sheet3. This may seem odd, but after snooping and looking around I wasn't able to uncover a sheet 2, and you also do not have to in order to find the answer. Sheet 1 gives you a bunch of story content, but go and check out some of the data on sheet 3. If you look around enough in square C4 you will find the following:
```text
VGhlIE1hcnRpYW4gQ29sb255LCBCZXNpZGUgSW50ZXJwbGFuZXRhcnkgU3BhY2Vwb3J0Lg==
```
You may recognize this as base64, but I did not. To be quite honest I kind of just assumed it was and got lucky. When you put this in a base64 decoder, like this website https://www.base64decode.org/, you get the correct answer.
{{% notice warning "ANSWER" %}}
The Martian Colony, Beside Interplanetary Spaceport.
{{% /notice %}}

---
## Question 6: What could be the probable C&C domain to control the attacker’s autonomous bots?

This one we are going to jump back into or EML Analyzer results. If we submit the reply-to email domain we can confirm that is indeed the answer.
{{% notice warning "ANSWER" %}}
pashter.com
{{% /notice %}}

---
## Wrap Up

That's all she wrote! We have completed the challenge and provided the CoCanDians with the information to save the kings daughter. My first experience with BTLO was incredibly fun, and there will certainly be more in the future. I hope you all enjoyed, check out the walkthrough video below, and keep getting better every day!

---

## Walkthrough Video

{{< youtube C5VlJqp0hJ8 >}}

<br>
