---
layout: post
title: Wallaby's Nightmare VM Walkthrough
---

## for the [Wallaby's Nightmare](https://www.vulnhub.com/entry/wallabys-nightmare-v102,176/) VM hosted on Vulnhub from Waldo.

This was a pretty cool VM which didn't only have a few nice ideas but was also pretty educational too (I'd never gotten round to learning a lot about IPTables before this, to my shame) and I picked up a few new tricks for my toolbox on the way.

Ok so as always we begin with an nmap scan, finding pretty much just port 80, as the irc port listed is filtered:

[<img src="{{ site.baseurl }}/images/wallaby/1.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/1.png)

After a Nikto scan which finds a path traversal vulnerability, the VM decides it's had enough of being prodded and port 80 closes down, but port 60080 opens up, so I point my browser to this to investigate the traversal possibility.

Front Page:
 [<img src="{{ site.baseurl }}/images/wallaby/2.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/2.png)

Nikto's findings:
[<img src="{{ site.baseurl }}/images/wallaby/3.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/3.png)

Usually this would be fine, but when we view source it's just plain old lies:

[<img src="{{ site.baseurl }}/images/wallaby/4.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/4.png)

Went through the rest of the parameterised pages and tried the same old tricks with base-64 encoding, SQL injection attempts etc. but nothing came of that.

Ended up trying to enumerate pages through dirb and got a few new hits that weren't linked from the original site:

[<img src="{{ site.baseurl }}/images/wallaby/5.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/5.png)

Again not a lot of useful info here until we load the mailer page:

[<img src="{{ site.baseurl }}/images/wallaby/6.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/6.png)

Commented out javascript. Honestly you'd be amazed at how true-to-life this is :(

So let's try this out and see if we can get anywhere with another battery of the usual traversals, SQLis etc as mentioned before:

[<img src="{{ site.baseurl }}/images/wallaby/7.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/7.png)

A hit! this is vulnerable to command injection, as we can see when 'ls' dutifully brings up the dir listings.

So I place pentestmonkey's PHP reverse webshell into my local apache dir and use wget within the address bar to download it onto the victim machine, then set a port listening with netcat on my local machine as I get the www-user to call the php file:

[<img src="{{ site.baseurl }}/images/wallaby/8.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/8.png)

using sudo -L we see that our user can play around with iptables, so we see what the state of that is with *sudo iptables -L*:

[<img src="{{ site.baseurl }}/images/wallaby/9.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/9.png)

We see here a rule to block IRC, which is why the port was showing up as filtered in our nmap scan. we can remove that entry with

    *sudo iptables -D INPUT 2*

So we can see the new iptables listing and the new nmap scan to verify that it has indeed worked:

[<img src="{{ site.baseurl }}/images/wallaby/10.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/10.png)

[<img src="{{ site.baseurl }}/images/wallaby/11.png"
    style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/11.png)

So now that we have an IRC server on the victim machine, let's connect to it. To do this I installed irssi on kali and joined:

[<img src="{{ site.baseurl }}/images/wallaby/12.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/12.png)

When we join Wallaby's room, there is a bot and "Waldo", our intended victim admin. so back to our shell we look around the filesystem for the code to that bot and find it under a modules directory, here's his script:

[<img src="{{ site.baseurl }}/images/wallaby/13.png"
    style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/13.png)

So it looks like it's going to run whatever we want it to, but only if we're Waldo, let's double check that the script we're looking at is definitely the one in use and just see it work:

[<img src="{{ site.baseurl }}/images/wallaby/14.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/14.png)

Now we'll use netstat to see the outgoing and incoming connections to this machine and see if we can cut off Waldo; he's the only one the bot will follow but he's taking up the name which must be unique within the room. Since we can control iptables we're just going to strangle his connection:

[<img src="{{ site.baseurl }}/images/wallaby/15.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/15.png)

Two connections, one for the bot and one for Waldo. We'll do a /whois back in the IRC chat to see who connected first, though the smart guess would be Waldo rather than leave his bot unattended, we find out that's indeed the case:

[<img src="{{ site.baseurl }}/images/wallaby/16.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/16.png)

So let's block off the outbound port he's connecting through:

[<img src="{{ site.baseurl }}/images/wallaby/17.png"
    style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/17.png)

and 180 seconds later we can use */nick waldo* and adopt that username as soon as he times out

[<img src="{{ site.baseurl }}/images/wallaby/18.png"
     style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/18.png)

To check that we're now in control of the bot:

[<img src="{{ site.baseurl }}/images/wallaby/19.png"
    style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/19.png)

Back on Kali I create this .sh script:

  >#!/bin/bash

  >sudo chmod 4777 /usr/bin/python2.7

and use wget to pull it from my apache folder just as I did with the reverse webshell and then ask the bot to execute it. This will give the SUID bit to the python binary.

[<img src="{{ site.baseurl }}/images/wallaby/20.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/20.png)

and the detailed listing of that binary before and after the IRC bot runs my script:

[<img src="{{ site.baseurl }}/images/wallaby/21.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/21.png)

with the last line above us using python to invoke a shell for us with its shiny new SUID, using this nifty line:

   >python -c "import os;setuid(0);os.system('/bin/bash')"

and now we can navigate to root and view the flag!

[<img src="{{ site.baseurl }}/images/wallaby/22.png"
    style="width: 800px;"/>]({{ site.baseurl }}/images/wallaby/22.png)
