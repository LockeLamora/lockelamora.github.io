---
layout: post
title: Bsides Vancouver 2018
tags: vulnhub
---
## Bsides Vancouver 2018 CTF

[HERE](https://www.vulnhub.com/entry/bsides-vancouver-2018-workshop,231/) is a VM image of the vancouver Bsides workshop, done by Abatchy. For anyone following security, Abatchy is a pretty good person to follow and learn from.

There are two main ways of doing this VM; an easy but convoluted way of getting a webshell, escalating privileges etc, and a risky-but-pays-off way of brute forcing SSH creds. This latter way I wouldn't normally try because it can take ages, there might be no pay-off, and it bogs down your environment while it runs.

So I'll go through my more naturally-inclined route first and then show you the easier way:

# The normally-expected route

Our nmap output:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/1.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/1.png)

There are a few interesting ports open, but for this route we focus on port 80. Still though,

We see from nikto that there's a robots.txt file:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/4.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/4.png)

so we navigate to this page to find a wordpress install, and from the posts present we can see that there are two users; admin and john.

So let's brute-force John.
[<img src="{{ site.baseurl }}/images/bsidesvancouver/7.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/7.png)

You can also see the wordpress version above too, which is useful for finding out what to do next.

so we find out that the password for john is "enigma". We would log into this and manually do our webshell, but I've manually done this fake plugin step so many times I'm just going to do it through metasploit rather than repeat what's on this blog quite a few times already:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/8.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/8.png)

so here we are with our shell as www-data, now to see how we can escalate our privileges. Looking at the cron jobs we can see a log-clearing shell script:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/9.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/9.png)

[<img src="{{ site.baseurl }}/images/bsidesvancouver/10.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/10.png)

When we try to execute it ourselves we don't have permission, so whoever executes it has more permissions than us.

There are lots of things we can do here. In my last post I created a connection back to my machine which then ran as root, so this time for some variation I'll give the current user sudo permissions without requiring a password:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/11.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/11.png)

give it a minute to run and then we have the power to become root!

[<img src="{{ site.baseurl }}/images/bsidesvancouver/12.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/12.png)

and then we get the flag:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/13.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/13.png)

# The slightly-riskier route

so let's go back to our nmap results:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/1.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/1.png)

port 21 (FTP) lets us connect as an anonymous user and retrieve a list of users:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/a.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/a.png)

which we can see here:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/b.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/b.png)

when we try these credentials over ssh, all users except for "anne" require a private key for authentication, whereas anne requires only a password. So let's try and brute-force this.

For my wordlist, I used [This](https://github.com/danielmiessler/SecLists/tree/master/Passwords) which is a collection of the top x most probable passwords based on stolen credentials.

This can be done using patator or hydra really, I prefer patator for most use-cases but for simple ssh brute-forcing hydra is just less awkward, in these examples the top screenshot is hydra and the one beneath it is patator:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/c.png"
 style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/c.png)

[<img src="{{ site.baseurl }}/images/bsidesvancouver/c2.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/c2.png)

When we get on as Anne, it turns out that she's a sudoer:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/d.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/d.png)

so we jump right at the end stage of the previous route I took, we just switch to the root user and grab the flag:

[<img src="{{ site.baseurl }}/images/bsidesvancouver/e.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/bsidesvancouver/e.png)

This all assumes a common password for SSH, which isn't usually the case. With brute-forcing you have to know when to give up rather than spend 3 hours running a brute-forcer against rockyou.txt while your laptop fans are going crazy and nothing else will run, effectively stopping anything but the most rudimentary investigations down other avenues.
