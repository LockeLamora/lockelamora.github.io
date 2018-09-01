---
layout: post
title: Paypal - bug bounty
tags: Live
---
## A week of extremes

I've spent my week off work doing some bug bounties after the success from my previous post. I targeted a few sites using my VPS and a custom script I've written so it can mostly work unattended following the steps I'd normally take manually. Some are still waiting for responses or had multiple submissions which aren't all done yet. Some others in my queue, such as the gov.uk issues that I've found, are still vulnerable months later.

You can tell the differences between a company that takes security seriously and one that kinda pays lip service to it. Some bug bounties are so restrictive they're unrealistic and ignore the places where, as a QA tester, I'd instinctively look at as having more likelihood of being neglected or having a lower priority. These aren't worth testing; they clearly don't care about security but they can tell their clients about their active security stance.

Others have a full program, but are unresponsive or just don't care when you submit a file full of developers' personal and professional passwords.

Paypal, on the other hand, trades on their reputation for security so they're a good bet to work for.

When enumerating, I like to look at any QA servers I find; they generally have the same software etc as live, but the servers aren't usually high on the infrastructure team's list of priorities so they tend to be less bleeding edge. Some even hold legacy data and the combination of that is disastrous as news reports have shown.

For Paypal I found a few test servers (which again I instinctively gravitate towards first; my QA-sense perks up), but most wouldn't even let me view their index page. One, however, not only let me in but automatically logged me in as an admin. It was an admin dashboard, I didn't see any live/legacy customer data (all dummy accounts as QA should be doing) but it was an internal tool that allows an attacker to do some very valuable research (for example, if I know an administrator is seeing a certain field on my account I can test to see if that field causes an XSS so I can steal their session and act on behalf of a Paypal staff member).

I didn't do any further experimentation; just showing Paypal the URL and a screenshot of the internal system. This was enough to get me a $10,000 bounty. The staff response was unexpectedly fast and the fix was almost immediate. As a long-time user of Paypal it's definitely reassuring!

This gets to the crux of bug bounties; if I had more nefarious intentions, would I make more money selling this find on the black market? No chance. Other programs allow me to test my scripts or do some boredom testing with the possibility of a t-shirt or hall of fame mention, but if you really value your assets and your reputation this is how you run a bounty program.

Screenshots not attached since obviously they show sensitive systems.
