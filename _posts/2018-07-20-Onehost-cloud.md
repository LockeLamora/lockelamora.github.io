---
layout: post
title: Getting cloudy with Kali
tags: Misc
---
## Discovering a dedicated Kali VPS provider

[<img src="{{ site.baseurl }}/images/onehost/OneHost.png"
  style="width: 800px;"/>]({{site.baseurl }}/images/onehost/OneHost.png)

  I've been researching a VPS for my kali linux for a while. It's just something you need if you ever want a reverse shell or metasploit module to work in the real world, doing a vulnerable VM is fine but your LHOST and LPORT are useless once you try to use them over the internet. I spent a week or so researching that was out there and was initially resigned to an AWS instance when I noticed [onehost cloud](https://onehostcloud.hosting/). They offer a dedicated Kali instance which takes no time at all to set up, so here's the benefits and reasoning I've compiled for how and why I've been using it thus far:

  ## Why shift from my powerful VM to onehostcloud?

  **It’s offloaded:** when I start even a vpn test such as for hackthebox.eu, I can do it confidently. Before, if it was 3pm I wouldn’t start anything too intensive because I’d only have to shut it down at 5pm to go home. Now I can set it off, shutdown my laptop at 5pm then resume the session at 7:30pm when I get home and view all the progress made when I’ve been stuck on a bus!

  **External IP:** (This is very important and probably the reason you’re here) searching for sql and xss vulnerabilities is great for a bug bounty, and on a vpn you can easily show your reverse webshell proofs, but when your VM is behind a nat network, which is then behind either your home router (solvable with port forwarding but that’s annoying) or behind your work firewall (good luck!) then you’ve no chance of providing a valid LHOST. onehostcloud will give you an externally viewable IP address so you always have an LHOST to try out your metasploit attempts or your remote file inclusions.

  **The offset effort:** Too many times I’ve had an online game or just a friday night hacking session scheduled and my machine is bogged down, the CPU usage high and fans whirring enough to take off because of some scans I set off an hour ago. Now I can be playing an online game using my full power while my Kali instance is powering through some scans and my laptop is free and easy and cool as a cucumber, Even with the NoMachine instance there for me to check on with a machine away off somewhere else doing all the dogsbody work!

  **The peace of mind:** This is just for me personally since I can get so anxious, I only do whitehat work; usually picking my targets through either hackerone or something like a google search of

  site:.co.uk “responsible disclosure”

  But if penetration testing is my job I don’t want to be the target of an overly-paranoid sysadmin since a black mark means no more work EVER! so it’s nice to do my work on here, even if I’m going to be using my identifiable email address to submit my findings

  **The ease of install:** The rest of these points would apply to any VPS but onehostcloud is the only one I’ve found that has a dedicated kali install already available without the need for legal forms like AWS (which is “fine” (read:severely restrictive even for internal pre-alpha tests) for a contracted engagement, useless for equally-legal bug bounties)

  **External:** If you're within a local network on a trusted domain you can't be sure what's an issue and what isn't without being able to externally test, and sometimes the only way to do that is to bring your laptop home and try there. Now I can test internally on my local machine and externally using my VPS.


  ## Privacy

  Some people have different criteria to me when it comes to privacy. Officially the onehost cloud policy is that they'll not interfere in anything from either extreme end of the blackhat-whitehat spectrum, with the only exception being DDoS attacks since that affects their users. I'll never have to put this to the test since I don't do anything on the shadier end of things, but it's nice to know you're not under unscrupulous attention.

   This leads to a lot less red tape as I mentioned earlier; the mere mention of pentesting to AWS can trigger so many procedures and policies that you may as well be setting up and using a Commodore 64 by the end of it.


   ## Performance and ease of use

   I'm using the lowest paid tier of the kali options available, connecting using NoMachine was a bit jittery at first but after installing XFCE the thing's as silky smooth as a dolphin at a saturday night disco. It's as responsive and quick as using my own VM, which is rare for any remote desktop sessions.

   The user dashboard is very easy to use too; there are options for viewing logs, changing the power state of your machine etc.

   One of the bigger highlights for me away from my machine itself is the onehost staff. There's a technical forum and these people seem to know their stuff; I've had an enjoyable few weeks posting my own tips and hearing from their side the details of what they do and the thought processes behind new features they want to work on etc, which is just really nice and informal.

   To be honest it's somewhere I'd like to hang around when I've no technical issues to discuss, which will in turn be a benefit to newer clients whenever they need help I guess. That's the kind of community that will greatly benefit everyone and I look forward to seeing it grow as the service becomes more popular.

   ## Conclusion

   Do I recommend it? well for my use-case (whitehat, with the odd foray when my curiosity is piqued by something) the answer is a definite "YES!". I've given the reasons why above and I think it's worth every penny.

   If your own use-case is at the other end of the ethical spectrum you can have a look around at what their policies are, even have a chat with them in the forums, do your own risk assessment and decide for yourself.
