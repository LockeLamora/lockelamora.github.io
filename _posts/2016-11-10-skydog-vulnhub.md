---
layout: post
title: SkyDog VM Walkthrough
---

## for the [SkyDog](https://www.vulnhub.com/entry/skydog-2016-catch-me-if-you-can,166/) VM hosted on Vulnhub from James Bower.

This was a fun VM, more of a game, but not always relaxing! There was a heavy dose of clues and puzzle-solving which I don't normally go in for, but they weren't too offputting.

[<img src="{{ site.baseurl }}/images/skydog/logo.png"
alt="catch me if you can logo image" style="width: 300px;"/>]({{ site.baseurl }}/)


First off, nmap found the following ports open:

>PORT      STATE  SERVICE

>22/tcp    closed ssh

>80/tcp    open   http

>443/tcp   open   https

>22222/tcp open   easyengine

I visit the http content through the browser and see the above logo and a lot of information on the SkyDog event and the rules around the CTF game etc.

So, for flag 1, the provided clue is:

#### Flag#1 - "Don’t go Home Frank! There’s a Hex on Your House"

So we can tell the obvious clues here; somewhere around the home page area there will be a hex value that needs decoding. There's nothing obvious on the interface so we delve into the source and find this unfortunately true-to-life comment:

[<img src="{{ site.baseurl }}/images/skydog/2_sourcecodeoldjs.png"
alt="commented out source code telling the user to remove this file before prod" style="width: 800px;"/>]({{ site.baseurl }}/)

When we view this file, the first line contains a hex value. When we convert this it comes out as

>flag{7c0132070a0ef71d542663e9dc1f5dee}

which is an MD5 hash that says "nmap"

#### Flag#2 - “Obscurity or Security? That is the Question"

So with nmap as the clue we revisit the ports given above. 22222 looks suspicious, and is open to SSH. Unfortunately we don't have any credentials for this yet, but it contains a flag and is something to remember for later:

[<img src="{{ site.baseurl }}/images/skydog/3_sshonport22222.png"
alt="ssh login prompt with flag" style="width: 800px;"/>]({{ site.baseurl }}/)

> Flag{53c82eba31f6d416f331de9162ebe997}

decrypts as md5 to "encrypt"


#### Flag#3 - “During his Travels Frank has Been Known to Intercept Traffic"

The word "encrypt" being given as a clue shows us that port 443 is about to come into play, but when we talk about intercepting traffic I fire up Wireshark and spot the following:

[<img src="{{ site.baseurl }}/images/skydog/4_wireshark_sslhello.png"
alt="wireshark prompt shows ssl cert has a flag" style="width: 800px;"/>]({{ site.baseurl }}/)

>flag3{f82366a9ddc064585d54e3f78bde3221}

decrypts to "personnel"

#### Flag#4 - “A Good Agent is Hard to Find"

I tried the "personnel" page, and get a warning message telling me that my workstation hasn't been recognised and therefore access is denied.

Within web-based applications, when we see the word "agent" the user-agent string comes to mind immediately, and a user being identified by their workstation gives us a target to hit; find the browser the site expects. I tried a few strings from popular browsers but nothing hit, so I got back to the only leaked information that may give us a clue about the inner workings of the site, that commented out .js file. Eventually we hit the jackpot with another comment:

[<img src="{{ site.baseurl }}/images/skydog/5_sourcerevisited.png"
alt="Source details a so-called temporary backdoor for an older browser" style="width: 800px;"/>]({{ site.baseurl }}/)

> maindev -  6/7/02 Adding temporary support for IE4 FBI Workstations

> newmaindev -  5/22/16 Last maindev was and idoit and IE4 is still Gold image -@Support doug.perterson@fbi.gov

I've pasted this here as we also have a username that may help us in the future.

So i set my Firefox browser to emulate IE4 with a googled UA string:

[<img src="{{ site.baseurl }}/images/skydog/6_uachange.png"
alt="changing firefox user agent to ie 4" style="width: 800px;"/>]({{ site.baseurl }}/)


Now when we visit the site we are granted access! It looks busy at first but in reality it's a single page, it does however contain a flag.

[<img src="{{ site.baseurl }}/images/skydog/7_personnelsite.png"
alt="FBI personnel page showing flag" style="width: 800px;"/>]({{ site.baseurl }}/)

>flag{14e10d570047667f904261e6d08f520f}

>clue = new + flag

 flag decrypts to "evidence"

=> clue = newevidence

#### Flag #5 The Devil is in the Details - Or is it Dialogue? Either Way, if it’s Simple, Guessable, or Personal it Goes Against Best Practices

This is probably the one that stumped me the most. I went to the newevidence page, but it requires authentication. The Personnel page logs in as Agent Hanratty, and IMDB tells me that character's first name was Carl, so given the username above we know the user will be carl.hanratty. The password is hinted to be personal info, but there's nothing on the page we just unlocked, so I turned to google, but that character was only based on a real person, so I watched the film and pretty much typed in anything he said that might help.

Eventually it worked, with his daughter's name "Grace" being the password.

[<img src="{{ site.baseurl }}/images/skydog/8_newevidence.png"
alt="New evidence page" style="width: 800px;"/>]({{ site.baseurl }}/)

Clicking on the 'evidence' link gives the next flag.


>flag{117c240d49f54096413dd64280399ea9}

decrypts to "panam" (an airline company that the main character defrauded)

#### Flag #6 Where in the World is Frank?

Here we have an image showing a francophone area. Of course the main character was in France and arrested in Montpellier, but neither of these yield any new pages. There's no exif info on the picture, but after trying a few of the aforementioned words within steghide, panam works to extract it:

[<img src="{{ site.baseurl }}/images/skydog/9_steganogaphy.png"
alt="extracting text from image" style="width: 800px;"/>]({{ site.baseurl }}/)

>flag{d1e5146b171928731385eb7ea38c37b8}

>=ILoveFrance

>clue=iheartbrenda

(I late viewed the pdf also linked to from that page, but it turned out to be a clue that the image contained info hidden by steganography which I'd also stumbled on)

#### Flag #7 Frank Was Caught on Camera Cashing Checks and Yelling - I’m The Fastest Man Alive!

At first I thought the quote given in this clue was a reference to the "Chase Manhattan Bank" and tried a few combinations of those words, but then it hit me that I'd recognised the character use the name "Barry Allen" thanks to the recent new series of "The Flash" on TV, after a lot of wonder what to do with that it turned out to be a login for the ssh we found earlier on port 22222:

[<img src="{{ site.baseurl }}/images/skydog/10_sshsuccess.png"
alt="connecting to port 22222 as Barry Allen" style="width: 800px;"/>]({{ site.baseurl }}/)

The password was the clue from the last flag; iheartbrenda.

a directory listing shows a flag file:

>flag{bd2f6a1d5242c962a05619c56fa47ba6}


decrypts to "theflash" which we knew already, but might be a clue to the next one

#### Flag #8 Franks Lost His Mind or Maybe it’s His Memory. He’s Locked Himself Inside the Building. Find the Code to Unlock the Door Before He Gets Himself Killed!

So we find a system-security.data file, after treating it as a zip and transferring the file back to my own machine, I try to figure out what it is. Using "theflash" a a clue I thought it'd be something memory-related, at first I assumed it was a truecrypt file but it turned out to be a RAM image (after a lot of dead-ends!). I ran the imageinfo on volatility which shows that it's both a valid RAM image and running windows, I ran pslist to see a list of running processes and noticed notepad there. so I ran it with the notepad switch and got a result:

[<img src="{{ site.baseurl }}/images/skydog/11_volatilitynotepad.png"
alt="viewing contents of notepad" style="width: 800px;"/>]({{ site.baseurl }}/)

This turned out to be HEX which translates to a flag value:

>flag{841dd3db29b0fbbd89c7b5be768cdc81}

Unfortunately I couldn't decrypt the MD5 in there at all (and tried to decode assuming it was other formats too), none of my usual sites worked for me at all, so I think this time poor Frank's luck has run out :(
