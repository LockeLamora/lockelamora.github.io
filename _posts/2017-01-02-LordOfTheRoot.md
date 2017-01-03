---
layout: post
title: Lord Of The Root VM Walkthrough
---

## for the [Lord Of The Root](https://www.vulnhub.com/entry/lord-of-the-root-101,129/) VM hosted on Vulnhub from KookSec.

With this VM, nmap scans shows we only have port 22 (ssh) open, but with a clue to unlock more:

[<img src="{{ site.baseurl }}/images/LordOfTheRoot/1.png"
 style="width: 800px;"/>]({{ site.baseurl }}/)

 In the banner shown above, the word "Knock" and 3 numbers gives us an instruction for port-knocking, so we target ports 1,2, and 3 in sequence and then do another nmap scan:

 >nmap -r -Pn -p 1,2,3 <ip>


[<img src="{{ site.baseurl }}/images/LordOfTheRoot/2.png"
  style="width: 800px;"/>]({{ site.baseurl }}/)

And here we can see that port 1337 has opened. This turns out to be a http port, so we access it through the web browser:

[<img src="{{ site.baseurl }}/images/LordOfTheRoot/3.png"
   style="width: 800px;"/>]({{ site.baseurl }}/)

The source for robots.txt has this comment:

>!--THprM09ETTBOVEl4TUM5cGJtUmxlQzV3YUhBPSBDbG9zZXIh

In base64 this decodes to:

>Lzk3ODM0NTIxMC9pbmRleC5waHA=

With that '=' padding telling us it's encoded again, so this is another format we recognise. So we decode from base 64 again and get:

>/978345210/index.php

so this looks like a url, we'll add that to the browser and it brings us up a login form. Nothing we can really do anything with with no credentials, but we take the form data and add it to sqlmap as a parameter:

[<img src="{{ site.baseurl }}/images/LordOfTheRoot/4.png"
   style="width: 800px;"/>]({{ site.baseurl }}/)

I used this to grab content from various tables, and this one was the jackpot:

>Database: Webapp
Table: Users
[5 entries]
+----+----------+------------------+
| id | username | password         |
+----+----------+------------------+
| 1  | frodo    | iwilltakethering |
| 2  | smeagol  | MyPreciousR00t   |
| 3  | aragorn  | AndMySword       |
| 4  | legolas  | AndMyBow         |
| 5  | gimli    | AndMyAxe         |
+----+----------+------------------+

I tried smeagol just because it contained the word root, so not sure what the others do, but it logged in straight away:

[<img src="{{ site.baseurl }}/images/LordOfTheRoot/5.png"
   style="width: 800px;"/>]({{ site.baseurl }}/)

I enumerated the OS and found an exploit for it here: https://www.exploit-db.com/exploits/37292/

and executed it to get root:

[<img src="{{ site.baseurl }}/images/LordOfTheRoot/6.png"
   style="width: 800px;"/>]({{ site.baseurl }}/)
