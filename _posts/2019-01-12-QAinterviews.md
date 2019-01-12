---
layout: post
title: How to do a QA interview
tags: QA
---
## It's OK to show yourself off

I do interviews for QA testers. For background and context, Our testers manually test things and then automate them, we also "test" the designs and generally thrash things out.

So I learned Irish language in school, after hearing that impersonal objects like a number of torches get a different numbering system from a number of people, I'd terrorise my teacher with questions like "what about sentient robots? what about aliens? what about pets?". This is thrashing out a rule or a design to edge cases and whatever you can think of. Remember: **if you can think of it and it's legal in the software a user is either doing it right now or will do in the future**. This is your bread and butter. Thinking of those annoying what-if questions a child asks is the heart and soul of being a QA, not coding, not experience; these things you can eventually get. That inquisitive nature weaponised to really ruin a website is your worth, so if you have weird examples, bring them along.

For over a decade my philosophy has been "Be lazy", a somewhat flippant way a previous manager used to describe my (non-QA) role. He meant I should automate repetitive tasks. In that role we got our builds down from 9 hours of human interaction to a guy hitting a button and waiting 15 minutes, which saved the company thousands of pounds per week. I tend to extrapolate that out to my role as a Lead QA; If I hire you what you can expect is full autonomy, trust, support/funding/time in your own development, no need for test cases that I have to review, no reports etc. If I've hired you it's because you're an adult and a professional who is willing to learn and that's how I'll treat you, If you're a junior then you get a very experienced buddy to help you along until you're amazing.

(Personally I've learned to try not to be that buddy, I've been programming for nearly twenty years, worked in my company for six years, and like all good QA I realise the weak areas in that and I'll give you to someone who has recently successfully made the same journey as you. They'll know what you're going through and make less assumptions than me. I see that as a trait of empathy as addressed later)  

What that does not mean is that these things aren't available; if you have a problem, need emotional support, need senior backup with an issue, need a pair of eyes over your code etc. that's always going to be given freely without judgement as you'll read.

But what I'll give here is my ideal lookouts during an interview:

# make your interview a conversation

Not every interviewer wants this, but I do. I want to sit and have a chat. I don't want to barrage you with questions only for you to give yes/no answers. Let's just talk about stuff, we love our work so let's just geek out, and that means everything. Let's talk about what makes up our worst days and the absolute gems. The browsers we hate, the languages we like, weird frameworks, the feedback we gave in a retro once that changed everything. Remember I have to work with you, and a good dynamic is amazing. Starting off your interview telling me everything that's wrong with our tech stack makes me just think "Can I work with this person every day?"

# show yourself off, not just your career

I've seen testers submit a CV with either manual-only experience or no QA experience at all. Maybe I've had a coffee whilst reviewing the applications, or I was just a good mood, but I've interviewed SOME of these people. Every now and then you find someone and after a conversation you realise they've spent a couple of hours every evening doing courses, reading books or just automating on open APIs. At that moment I'll close your CV and we're talking, and I'm excited. You could have left out 2 pages of your CV and replaced it with that one fact, if you'd coupled it with a github repo you'd be an instant first interview.

I've turned down over 100 testers in a year, with 10+ years experience in favour of someone from customer support who wanted to be a tester so bad she read every QA book and blog she could get, her boyfriend got her QA books for her birthday which she read on a beach on holiday, She had a github account with some projects on. She couldn't sit down with an automation tester due to work deadlines, so she booked days off and sat with that team hassle-free. This is an extreme example (but a real one) to run counterpoint to what I hear in 90% of interviews:

"I Love automation! it's my passion and I want to learn it!"

"sweet! so what have you done toward that?"

"Oh nothing yet, I don't get the time at work"

 please do invest in yourself in whatever time you can get. You might think you're a rank amateur after some udemy courses, random articles off google and a few scripts on github, but I'm looking at you with stars in my eyes.

And at that point you're THAT awesome person who nearly slipped through. Exciting but with a CV that's so far beneath you. It's good when we find you, but it sucks that it was down to whether or not we'd had a coffee when we read it. Remember that we want to hire you, otherwise we wouldn't be looking, give us an honest sliver of a reason.

but remember...

# do not lie

It sucks, but this unfortunately does need to be said because I see it so much. If you don't know what we work with, tell us honestly and maybe we can teach you! We've seen hundreds of people, and we can see if you've got potential even if you have zero experience. Don't list everything in our job advert on your CV just because you think it's what we want to see.

If you get through to the first interview due to buzzwords on your CV the best case is that we figure it out and halt everything, the worst case is that we sit you down in front of a framework that you have no idea how to use and this isn't a contract role anymore, this is it.


# be assertive

As I mentioned earlier, I'm "Lazy" as a tester, in that I automated repetitive tasks. In the same way I'm "Lazy" as a manager; you get full autonomy, If you say a feature is ready then it goes live; I don't oversee you if you don't want me to. I don't demand test cases. If you ask me to check something then absolutely! but if you don't know how to split a string separated by spaces into an array of words, I'd expect you to do what every professional developer does and google that and copy some code off stackoverflow.

I'm lucky in that my company has never had a bad Dev/QA dynamic, you're equal to a developer in all things, but let's face it, sometimes a new developer has just been hired from a very different place and you'd have to be able to nudge them toward the company culture, in some situations you just have to remember that your role is to tell a professional that they've made a mistake, and the way it should be is that's no problem because you've just saved them some embarrassment.

You'll also have to be able to look at a perfectly functional, well-designed piece of work and just say "no.. we're not doing that". A product manager relies on you to call things out or he wouldn't ask your opinion, it's symbiotic.

For example, a client does event 'X' and your project is to send them an email when that event completes. The developers have optimised it, designed it, the UX person has made a nice interface for who it goes to.

Here are two possible scenarios:

1-You look at the design, it's great! you look at the developers ideas and database architecture and it looks so efficient, it even writes to a cache it's so optimised. You say yes I can test this and I can already think of a way to automate it!

2-You ask the Product Manager "how many times do we expect event X to happen for a client in the most extreme use-case?". He goes away, asks the infrastructure team to analyse their stats and the next day informs you one client does event X over 100,000 times per day. You ask him if the entire idea is even feasible to send over 100k emails to the same client per day. Your company isn't blacklisted as a spam-bot. Clients aren't enraged over mailserver crashes. You've just stopped the developers from spending 3 months turning your expensive server-farm into a highly-optimised mail bomb.

All of this was against the same amazing design. When everyone's so excited over something sometimes you just have to give the crazy what-ifs because your mind wanders like that, and for this people will say thank you!

# there aren't always right answers

As I mentioned earlier, sometimes I just like to have a conversation with people. I'll ask them something about the job and I see them hesitate about what they should say, Hell I don't even know what you should say, when I'm finished interviewing you I'm going straight back to my desk to the job you're interviewing for; I have my own opinions on things too! I just want to chat a bit with a QA who might end up sitting beside me. I think my main advice here is by all means be negative, but not to be OVERLY negative.

It's ok to call out what you think your other companies have done wrong and what you'd rather do, but don't go all-out and say you hope your manager dies in a fire! Remember we love passion, and that means things that irk you too. I've hired people who within weeks have changed areas of the QA codebase I've built up over 6 years and I'm just looking at the changes wondering why the hell I never did that before.

# be empathetic

You've spent 4 months on a feature. you know everything there is to know about it because you've been to every meeting. You're faced with a powerful bit of functionality, and what you have to be able to do is put yourself in the mindset of a new client who has never seen or heard of this before and ask yourself

-"how the hell do I know what to do??". Your documentation is also your product

-"is this what I do?". Users will do weird things, if the team's answer is "that's not what you're meant to do and now you've crashed it" then the feature was not fit for release, as surely as if it crashed on launch for that user.

In my job, yes when testing an API endpoint I can tap the guy next to me on the shoulder and ask "why didn't that work?" but what I should really do is log it as a bug, because a user can't tap that guy on the shoulder. I've done too much FOSS work to tolerate bad documentation for one minute longer!

# don't be afraid to ask for help

As a QA you tend to have to be sure of things and after a while you will be the source of all knowledge, but in your first few months at a new project the simple fact is that you won't be. I learned the value of being honest and humble when I worked in a bank; if you handed out too much change or transferred a few extra zeroes you quickly learn that running to the boss and shouting "HELP ME FIX THIS!" within a few minutes is far better than trying to cover it up and meeting a cumulative amount of crap rolling your way a few days later.

Luckily as QA our main focus is ascertaining risk, and that means such measures as requesting feature flags, so we can simply turn bad features off, so with good foresight even our worst mistakes can simply disappear! But still, if the devs aren't sure on something they might pass it to you, and if you're not sure feel free to halt EVERYTHING. Because you WILL make mistakes. I have made plenty too! but by this point in a project your devs are your best friends forever and you can just tell them you missed something and please can they rush a hotfix, obviously they'll say yes and it's done.

Like every reputable company as a whole you should never be judged on a bug that went through, just on how you and your team handle it, and as someone who specialises in security that's an increasingly important rule nowadays.

But if it's pre-release and code-related, you have Google and if you ask me something you have to remember I don't have the language book in my head, I'll be searching the same query you would, finding an article or stackoverflow answer and then just passing it on to you.
