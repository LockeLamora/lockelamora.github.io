---
layout: post
title: Milnet VM Walkthrough
---

## for the [Milnet1](https://www.vulnhub.com/entry/milnet-1,148/) VM hosted on Vulnhub from Warrior.

This VM used a lot of the usual techniques but skewed and twisted, so once opening a path your usual toolset normally wouldn't work and you'd have to innovate around it. I did add a few little tricks to my own toolset while working on this which is the best I can hope for in my practice VMs.

[<img src="{{ site.baseurl }}/images/milnet/mj.jpg"
alt="Milnet page graphic" style="width: 300px;"/>]({{ site.baseurl }}/)


First off, nmap found that the only open ports were 22 and 80:

[<img src="{{ site.baseurl }}/images/milnet/1_nmap.png"
alt="image of ports 80 and 22 open" style="width: 800px;"/>]({{ site.baseurl }}/)

so the first thing we do is jump to inspect port 80 on nikto, which shows that remote file inclusions are a possibility:

[<img src="{{ site.baseurl }}/images/milnet/2_nikto.png"
alt="screenshot of nikto output" style="width: 800px;"/>]({{ site.baseurl }}/)

Let's visit the site and see how it looks:

[<img src="{{ site.baseurl }}/images/milnet/3_site.png"
alt="screenshot of milnet site" style="width: 800px;"/>]({{ site.baseurl }}/)

It's very plain, with only 3 pages accessible by buttons (as well as an info.php which links to phpinfo, but there's nothing in there beyond what we've already learned about that remote file inclusion)

[<img src="{{ site.baseurl }}/images/milnet/4_source.png"
alt="screenshot of source code showing RFI" style="width: 800px;"/>]({{ site.baseurl }}/)

As we can see from the screenshot above, the code is including php files sent through as a parameter. A first I downloaded these in encoded base64 so view the source, but there's no reference to any admin areas or any hint of a database, so this is a static site that we don't care about other than as an entry vector.

I used the penmonkey php reverse webshell and encoded it to base64, then url encoded it and passed it through as a parameter to the content.php page to get the site to draw it in and execute it:

[<img src="{{ site.baseurl }}/images/milnet/5_shellinjection.png"
alt="injecting reverse webshell code as base 64" style="width: 800px;"/>]({{ site.baseurl }}/)

We first hit the home directory which has a few files which don't help, and one which details the use of wildcards to fool automated jobs. While checking the cron I see a job run every minute which calls a script which runs tar.

[<img src="{{ site.baseurl }}/images/milnet/6_foundcrontab.png"
alt="crontab" style="width: 800px;"/>]({{ site.baseurl }}/)

[<img src="{{ site.baseurl }}/images/milnet/7_backup.png"
alt="contents of backup script which calls tar" style="width: 800px;"/>]({{ site.baseurl }}/)

we can see that there is a bad wildcard in there which allows us to indirectly control what commands may be executed using files within /var/www/html, the specific section of the file in the user's home directory is shown below:

[<img src="{{ site.baseurl }}/images/milnet/8_theclue_tar.png"
alt="file showing wildcard to manipulate script" style="width: 800px;"/>]({{ site.baseurl }}/)

so i create a .sh file with the following contents to change the root's password:

[<img src="{{ site.baseurl }}/images/milnet/8_theclue_tar.png"
alt="file showing wildcard to manipulate script" style="width: 800px;"/>]({{ site.baseurl }}/)

[<img src="{{ site.baseurl }}/images/milnet/9_changepass.png"
alt="script to change root password" style="width: 800px;"/>]({{ site.baseurl }}/)

and add some files into the directory which tar will actually read as parameters against tar!

[<img src="{{ site.baseurl }}/images/milnet/10_readyforcron.png"
alt="created some files which will be read as command line switches" style="width: 800px;"/>]({{ site.baseurl }}/)

we wait a minute or so and then try to ssh in using the new password, and the final words from the VM author:

[<img src="{{ site.baseurl }}/images/milnet/11_theend.png"
alt="root access and message from author" style="width: 800px;"/>]({{ site.baseurl }}/)
