---
layout: post
title: Effective Agile Testing
---

## On being an effective Agile tester - Lessons I have learned from QA within a scrum team

Agile is always a strange beast, and quite rightly so in my opinion. I don't think I've worked in 2 companies that use Agile and Scrum the same way, or even 2 teams within one company. I think that Agile should be flexible enough to mould itself around how the team works together rather than the other way around, and so when I list the best practices I've come up with so far for how QA fits into all of this, it may be that it'll work for me and not you, but I hope to at least provide an example of how another team operates and hopefully provide some inspiration or ideas to explore.

All of these points were iteratively developed over one or more sprint retrospectives, and undoubtedly many will change, especially if new team members are added to the team or existing team members leave; that's always going to happen and since a lot of my own rules were made in response to how a specific team member or combination of team members works, this list will have to adapt, but again, that's agile!

So onto my lessons learned and best practices as they stand right now:

## 1: Definition of Done

My first contribution to the Definition of Done (to define when a task or story is in fact cleared for completion and removed from the sprint) was that it's not finished until it goes through, and is cleared by, QA. This includes retests and awaiting bug fixes. What this means is that a task handed to QA the day before sprint end is in no way "Done", this is mainly to prevent QA from looking like a bottleneck, and to protect the code itself; as if tasks are hurriedly thrown "over the wall" to get them out of a developer's pile before sprint end, then there's clearly going to be some compromise in code integrity there so overall this protects the team.

Over time this definition has been refined to also include other points, including the following which are more relevant to test:

* Automation tests have been written for the task (usually this is for API automation, Selenium automation rarely happens in-sprint due to the changeable nature of the front-end solution, but again that's agile and we have to be able to react quickly to changes)
* The task, *after* being passed by QA, is merged into the main project branch, and the branch it was in is removed. This allows us to treat the main feature branch as a release candidate, which allows us to ship releasable code at the end of each sprint, so that even if it's not feature-complete it's still tested and stable.

## 2: Tasks should never need to be moved "Left" (ie from QA to a developer)

This was an unfortunate rule to implement but you can't always expect that the Devs you work with now will the ones you always work with, so we keep this rule even though it hasn't needed to be enforced in quite a long time.

The theory behind it is this: if a developer has checked their code, and the task is small enough as it should be during planning, then the happy path should always pass. Bugs may be raised against edge cases or complicated paths that the developers don't have time to check, and that's fine; in that instance we'll raise a bug and link it to the task, but the task itself can pass. This bug will be added immediately to the sprint and given no points so it must be assigned and worked on.

If, however, I open my web browser to test a piece of functionality and the page doesn't even load, then it's not worth my time looking at the rest of the task and it'll be bounced back which will trigger a "gentle reminder" from the scrum master to said developer. This is to remind everyone that we're all in the same team and that QA is there to augment what the developers already do, any indication that there is any kind of animosity or anything but complete willing helpfulness between the two is a sign that there's something cultural within the team that needs to change. Usually this only crops up if a developer or BA has joined from a more hierarchial corporate company and needs to be eased into our way of working in a flatter structure.

## 3: QA Managers are no longer required

This is potentially divisive and by no means applies to everyone, but for us it works. The Tester is no longer one of a team of testers who work together as one big team; they are now a member of a scrum team and report to a scrum master and to their scrum team, and no one needs two managers. Any decisions which affect the wider QA "department" such as policy, tools or recruitment can be made by getting the senior-most QA members together to discuss it. In a flatter structure we don't need extra levels taken up by someone who'll only be required for bi-annual decisions.

## 4: Testers may pad out the points estimation to a task

A kind of natural extension of point 1 above, when estimating effort points for tasks, a tester may inflate it since something that takes an hour for a Dev to do might be central to the product and need a hell of a lot of QA (for one example). This would adjust expectations when deciding how much to bring in to a task; since if the definition of done is that a task passes QA, and we know already that there's a high effort task for QA already, it means that the sprint could just end up in a big QA pile. I don't often do this, but mostly for tasks that are larger than the average or considerably larger than the dev's effort estimation. It may also just mean that we consider a task to have more risk, for example that bit of code that we want to change for one feature but which lies central to the code and may be called by many other features.

## 5: Security should be part of the initial BA presentation

When the Business Analyst or Product Owner etc. meets with the team (Either as part of a Three Amigos meeting or to the team as a whole where this isn't implemented) to present the initial material for a new release, part of that discussion should be an initial analysis on the attack mapping of the release. To use our web application as an example, we must consider factors such as is there user input? If so we need to protect against XSS. Can the user upload files? If so the likes of remote execution should be investigated.

This usually leads to a more secure system even if these elements would have been tested already; since we're part of a team it's always better to tell the developers our suspicions and worries beforehand so that we're not "tricking" them by pulling off some fancy moves when the release is nearly ready, but that rather we work together throughout the life of a release to ensure that everything was built with security in mind from the very conception of the product.

This can be applied to any other type of testing; it's always better to let your team know in advance any potentially shaky areas your gut tells you about right away. It's always about teamwork and not trying to trip people up.

So that's what I can think up so far, and again no doubt a lot more will be added as time goes on and one size certainly doesn't fit all when it comes to Agile. I'd be interested in what everyone else thinks through twitter or email!
