---
layout: post
title: Violator VM Walkthrough
---

## for the [Violator](https://www.vulnhub.com/entry/violator-1,153/) VM hosted on Vulnhub from knightmare.

I picked it as it seems to suit me because it's a beginner-level VM with no contrived and complicated storyline injecting unrealistic scenarios to the VM. Some VMs on vulnhub are good and map to real-life situations that I can take away from for my personal work and some are too contrived to be useful beyond critical thinking.

[<img src="{{ site.baseurl }}/images/violator/index_smaller.jpeg"
alt="Foghorn Leghorn Image from violator" style="width: 300px;"/>]({{ site.baseurl }}/)

Here's my first walkthrough, and this is for the [Violator](https://www.vulnhub.com/entry/violator-1,153/) VM hosted on Vulnhub from knightmare.

First of all, I used netdiscover to find the vm in my Nat Network range, then Nmap to discover open ports, which finds ports 21 (FTP) and 80 (http) open.

I visit the http content through the browser and see this:

[<img src="{{ site.baseurl }}/images/violator/http_page.png"
alt="Foghorn Leghorn on http pager" style="width: 500px;"/>]({{ site.baseurl }}/)

Nikto then finds that it's running apache 2.4.7.

Attempt a scan with Zed Attack Proxy which finds nothing.

Attempt a scan with dirbuster which finds nothing.

Save the image of Foghorn Leghorn and run it through exiftool to see if there's any promising exif data but nothing there either.

There's also nothing in the source except for that link to a Depeche Mode album named "Violator", so that's probably important and all the site is going to offer us, so the only other avenue for exploration that we have is the FTP port.

Attempts to enumerate users through patator also find nothing, but there is a nice banner message letting us know that it's running ProFTPD 1.2.5rc3, for which an exploit exists on [exploit-db](https://www.exploit-db.com/exploits/36742/).

So I FTP into there with no credentials and I'm left with a fully-functional command prompt(!) which allows me to transfer files internally. I start with /etc/passwd and send it to /var/www/html and access it through the browser:

[<img src="{{ site.baseurl }}/images/violator/ftpetcpasswd.png"
alt="etc passwd commands" style="width: 500px;"/>]({{ site.baseurl }}/)

[<img src="{{ site.baseurl }}/images/violator/users.png"
alt="list of users" style="width: 500px;"/>]({{ site.baseurl }}/)

so we can see from here 4 interesting users: dg, mg, af and aw. From checking the linked wiki it looks like these are initials of Depeche Mode bandmembers. So let's use that same FTP exploit to grab their home directories and see what's in there.

DG has some binaries relating to another FTP program, MG has nothing, AF has a program called "Minarke" which I google to find out is an enigma machine, and AW has a hint file telling us we have an enigma to solve, which seems to map back to that enigma machine, so I download the compressed file through the browser and save it for later.

So going back to the FTP before, but this time I try logging in as DG. It needs a password, so I try the songs from the tracklist of the original Violator release one by one and find "policyoftruth" lets me in. This lets me explore the user directories with higher permissions, and it turnds out the "MG" user home isn't empty at all, but contains instructions for manipulating the enigma machine to decode somethig, but since there's nothing to decode yet I'll save that for later too.

I used a PHP tcp reverse webshell from the ever-helpful /usr/share/webshells dir in Kali and upload that with my FTP access to the /var/www/html directory, executing it through the web browser directly. I just set my port to 1234 and then ran nc -lvp 1234 and as soon as the browser hit the file it picked up the request, but of course it's run under the www-data user, so not terribly helpful, but it's easier to explore than through ftp.

So what do we have? a shell with no permissions to transfer but can do some low-level executions, and an FTP access which can do no permissions but can transfer. BUT i found this [exploit](https://www.exploit-db.com/exploits/39166/) for the linux kernel that the box is running under, used the FTP account to upload that into /tmp as a .c file and used my reverse webshell under www-data to compile and execute it and BANG! root access!

[<img src="{{ site.baseurl }}/images/violator/root.png"
alt="running compiled binary gives root access" style="width: 500px;"/>]({{ site.baseurl }}/)

so inside a hidden folder within the root user's directory is a .rar file, so i copy that to /var/www/html and download it through the browser (because I'm lazy). This contains artwork.jpg but is password-protected, so I compile a list of the same original tracklisting from the violator album, with variations on spacing and capitalisation and run it through johntheripper to try and get a working password:

[<img src="{{ site.baseurl }}/images/violator/crackingrar.png"
alt="john the ripper cracking the rar" style="width: 500px;"/>]({{ site.baseurl }}/)

and inside this is a long string of unintelligible text which needs to be decoded! so this brings us back to the enigma machine, which it turns out I couldn't figure out so i followed the instructions on this handy [website](http://www.dcode.fr/enigma-machine-cipher) as below, so we can see how the intimidating-looking instructions map to actions:

[<img src="{{ site.baseurl }}/images/violator/enigma.png"
alt="running the enigma decoder" style="width: 500px;"/>]({{ site.baseurl }}/)

the final message after decoding looks like this:

>ONE FINAL CHALLENGE FOR YOU BGHX CONGRATULATIONS FOR THE FOURTH TIME ON SNARFING THE FLAG ON VIOLATOR ILL PRESUME BY NOW YOULL KNOW WHAT I WAS LISTENING TO WHEN CREATING THIS CTF I HAVE INCLUDED THINGS WHICH WERE DELIBERATLY AVOIDING THE OBVIOUS  ROUTE IN TO KEEP YOU ON YOUR TOES ANOTHER THOUGHT TO PONDER IS THAT BY ABUSING PERMISSIONS YOU ARE ALSO BY DEFINITION A VIOLATOR SHOUTOUTS AGAIN TO VULNHUB FOR HOSTING A GREAT LEARNING TOOL A SPECIAL THANKS GOES TO BEN RANDGKNSB FOR TESTING AND TO GTMLK FOR THE OFFER TO HOST THE CTF AGAIN KNIGHTMARE


So that was satisfying, if often very infuriating but I've tried to include the straight-line path here as much as I could :)
