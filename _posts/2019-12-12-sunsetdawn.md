---
layout: post
title: Sunset Dawn Vulnhub VM walkthrough
tags: vulnhub

---

## A Walkthrough of the [Sunset:Dawn](https://www.vulnhub.com/entry/sunset-dawn,341/) VM from Vulnhub

A short and fun machine showing off samba sharing, suid abuse, directory scanning and a simple reverse shell.

***

### Enumeration


[<img src="{{ site.baseurl }}/images/dawn/1.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/1.png)

Here we can see the http, samba and mysql ports are open after an nmap scan.

Running enum4linux shows 2 users and a shared drive available to us:

[<img src="{{ site.baseurl }}/images/dawn/2.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/2.png)

 [<img src="{{ site.baseurl }}/images/dawn/6.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/6.png)  

When we connect there's nothing useful in there, but it may turn out to be useful later on.

Scanning the http port gives us a promising lead:

[<img src="{{ site.baseurl }}/images/dawn/3.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/3.png)    

so we navigate to the directory:

[<img src="{{ site.baseurl }}/images/dawn/4.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/4.png)

and notice that there's a recurring job that tries to call a file within ITDEPT, which looks very much like the name of the shared directory we have access to through Samba.

[<img src="{{ site.baseurl }}/images/dawn/5.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/5.png)

 The file doesn't exist as we saw earlier, but we can easily replace it with whatever we want and it will be executed by the server:

 I just wrote a simple netcat call (the & backgrounds it as the connection dies straight away afterwards otherwise) and then transferred it using the local filemanager which sees the samba network:

 [<img src="{{ site.baseurl }}/images/dawn/6.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/6.png)

[<img src="{{ site.baseurl }}/images/dawn/8.png"
   style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/8.png)   

and then it's a case of setting up a netcat listener on our kali machine and waiting for the server to execute the script and fire off an incoming connection to us:

[<img src="{{ site.baseurl }}/images/dawn/9.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/9.png)     

So now we have the www-data user, let's see if they can do anything:

[<img src="{{ site.baseurl }}/images/dawn/10.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/10.png)

 sudo! well how careless. Let's call it and invoke a shell as root:

[<img src="{{ site.baseurl }}/images/dawn/11.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/11.png)    

and view the root flag:

[<img src="{{ site.baseurl }}/images/dawn/12.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/dawn/12.png)   
