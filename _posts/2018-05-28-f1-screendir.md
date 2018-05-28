---
layout: post
title: A few random projects
tags: Misc
---
## Minor projects around saving screenshots on OSX and fantasy F1

# screendir

As part of most testing activities you might take a lot of screenshots. When working on different projects, or in the case of hackthebox, vulnhub or the OSCP labs, you'll definitely want to keep these grouped but will switch around regularly. The best way to stay organised it to save them to all to their own dedicated project locations, but this is a manual process that's a bit fiddly.

[<img src="{{ site.baseurl }}/images/misc/screendir.png"
 style="width: 800px;"/>]({{ site.baseurl }}/)

 Screendir is a project I created in Python using QT4, as I'd used wxPython around 8 years ago and I didn't like it, so I figured I'll try an alternative. The project is [available here](https://github.com/LockeLamora/screendir).

 Fun fact: I finished this in one session and accidentally deleted the entire thing before committing (not a fun fact at the time), but pyqt is so intuitive that since I'd already learned the basics I redid it again in less than an hour.

 It allows you to keep a GUI window open and navigate to directories, manually input paths or choose previous directories from a history (very useful when regularly switching) and set that as the location that OSX will save screenshots to.

 To prevent having to use pip (which is absolutely abysmal on OSX due to https/http issues) I've packaged it as an application which the user can put into their "Applications" directory. This executable can be downloaded directly using [This link](https://github.com/LockeLamora/screendir/blob/master/dist/screendir.zip?raw=true)

# FridayCityLogic

I began playing the [Fantasy Formula 1 game](https://fantasy.formula1.com). Whilst waiting for the teething issues to resolve I got bored and wrote a script in ruby [available here](https://github.com/LockeLamora/FridayCityLogic) to make it easier for me to choose my own teams. The script name is named after my team name, which was just selected from a range of dictionary concatenation choices.

[<img src="{{ site.baseurl }}/images/misc/fcl.png"
 style="width: 800px;"/>]({{ site.baseurl }}/)

It averages out the points for each driver and constructor and recommends trades (if you input your current team) as well as  whole new teams (for a new account or wildcard). I also created a second fantasy f1 account where my script does most of the talking.. it's currently beating me. When we have crazy outlier races like Baku I override some of its choices, but since a lot of these choices involved signing up the rookie Leclerc, I have to admit by now that it was right and I was wrong.

There are some caveats I've had to deal with; for example my wife emailed them because teams FridayCityLogic recommended to her were impossible because the game said she couldn't afford them even though manually adding the costs brought it to under 100 million. The response they gave was that this is an undocumented feature and not a bug..  given that this isn't the rules and they didn't explain the actual logic that it follows I'm going on recent history and saying maybe it's a bug, but my script can accommodate it all the same by allowing an override.

I was thinking of writing a script to automatically update the data file, but as the game is very inaccurate and can change driver points, constructor points and therefore even your own ranking days after a race due to how new it is, I decided against this. I've also considered adding another repo with a bot to automatically create the team recommended by this script.. I'll definitely use that next year, which'll mean I have 3 accounts; my own, FridayCityLogic(moderated) and a completely automated bot in which I don't override any outlying stats.

I have to say writing the script has just highlighted a lot of the quality issues on release of this game. Luckily at the time of writing most have been remedied but they were still pretty bad. There's still currently one or two privacy issues that I reported a couple of months ago and the GDPR deadline has passed so let's see how that plays out.
