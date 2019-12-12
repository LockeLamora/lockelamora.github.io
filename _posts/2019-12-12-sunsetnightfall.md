layout: post
title: Sunset Nightfall Vulnhub VM walkthrough
tags: vulnhub

---

## A Walkthrough of the [Sunset:Nightfall](https://www.vulnhub.com/entry/sunset-nightfall,355/) VM from Vulnhub

I've been particularly enjoying the sunset series of machines, This one is perhaps a bit easier than the previous ones I've looked at this week, but no less fun.

***

### Enumeration


[<img src="{{ site.baseurl }}/images/nightfall/1.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/1.png)

There are quite a few ports open here. FTP, SSH, HTTP, Samba and Mysql. Enumerating http just returned a default apache install and bruteforcing MySQL turned up nothing, so my next step was to run enum4linux  and see what the samba ports (139/445) told me. The most interesting thing that showed was some local users on the system:

[<img src="{{ site.baseurl }}/images/nightfall/2.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/2.png)

so armed with two usernames I tried to bruteforce the FTP port with hydra:

```
hydra -t 0 -l matt -P /usr/share/wordlists/rockyou.txt -vV 10.0.2.14 ftp
```

(Or wherever your favourite password list is stored)

[<img src="{{ site.baseurl }}/images/nightfall/3.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/3.png)

After a very brief wait we get a password for matt's user on FTP:

[<img src="{{ site.baseurl }}/images/nightfall/4.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/4.png)

***
### Gaining first user shell

So let's FTP in and take a look around:

[<img src="{{ site.baseurl }}/images/nightfall/5.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/5.png)  

As we can see there's nothing useful around here, but it *is* matt's home directory, where the .ssh folder is usually kept. This doesn't exist here so I created my own using mkdir and then copied over my ssh keys to the server, creating an authorized_keys file just using

```
cp id_rsa.pub authorized_keys
```

[<img src="{{ site.baseurl }}/images/nightfall/6.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/6.png)  

Now let's try to SSH as matt and see if we get a command line:

[<img src="{{ site.baseurl }}/images/nightfall/7.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/7.png)   

we can't really do much useful again, but searching for files with special permissions turns up find:

[<img src="{{ site.baseurl }}/images/nightfall/8.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/8.png)   

find has a well-known shell invocation method, here we can use it as follows:

```
./find / -exec /bin/bash -p \;
```

[<img src="{{ site.baseurl }}/images/nightfall/9.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/9.png)   

and we see now we inherit some permissions from the _nightfall_ user.

so first things first, we navigate to nightfall's home directory and view the first flag:

[<img src="{{ site.baseurl }}/images/nightfall/10.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/10.png)   

since were not fully nightfall (We just inherit the group permissions) we recycle the same method we used to log in as matt; insert our own ssh credentials and log in over SSH:

[<img src="{{ site.baseurl }}/images/nightfall/11.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/11.png)

[<img src="{{ site.baseurl }}/images/nightfall/12.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/12.png)

### Escalating privileges to root  

First, we see if nightfall has any sudoer permissions:

[<img src="{{ site.baseurl }}/images/nightfall/13.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/13.png)

So we can use cat as root, essentially having read-only access over any file in the system. The password field of /etc/passwd of this system is 'x' meaning the password is hashed in /etc/shadow, which was hidden to us until now:

[<img src="{{ site.baseurl }}/images/nightfall/14.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/14.png)   

So I copied the root entry and saved that to my kali VM and ran it through John the Ripper:

[<img src="{{ site.baseurl }}/images/nightfall/15.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/15.png)    

so using _miguel2_  as our password we can switch our user to root and view the final flag:

[<img src="{{ site.baseurl }}/images/nightfall/16.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/nightfall/16.png)    
