---
layout: post
title: "How to get better at estimating user stories"
date: 2023-06-22 11:00:00 +1000
tag: agile
---

[Part 1: Why good estimates matter]({% post_url 2023-06-21-why-estimates-matter %})

In the [previous blog post]({% post_url 2023-06-21-why-estimates-matter %}) in this two-part series we built a case for why good user story estimates matter. In this part we'll look at how we might achieve good estimates in the first place.

I guess it's first worth clarifying that by a good estimate I mean an estimate that is a realistic guide to the actual time required to complete the task. After all bad estimates can fall either side of this. An estimate can be bad because it's too small, which means the task overruns and leads to unexpected delays. However, an estimate can also be bad if it's too large because of [Parkinson's law](https://www.atlassian.com/blog/productivity/what-is-parkinsons-law) and the tendency for the task to expand to fill the time allocated, even if it's not needed. So a healthy amount of pressure is needed to give focus.

With that established it might be worth looking at the reasons why we get poor estimates in the first place. Here are some of the most common ones:

- Not all the requirements are captured or they're vague.
- There are no visualisations (e.g. UI wireframes, architecture diagrams, etc) to give a clear picture of what needs to be built.
- There's inherent uncertainty in the story (maybe we're not even sure if it can be done).
- Complexities emerge that weren't thought of upfront, aka "unknown unknowns".
- Sessions in which estimations happen are poorly planned (too rushed or too long).

In the following sections we'll look at ways we can improve our estimations by addressing each of these issues.

### Capturing all the requirements

Believe it or not one-line stories don't produce good estimates, even if they're in the magical "As a... I want... so that" format. At the end of the day, for a dev to complete a story they need to get all of the requirements at some point, whether that means getting them on the fly through back and forth communication with the Product Owner, finding out about them during the PO demo after they've finished development or just filling the blanks in themselves. All of these ways represent time spent and are less efficient that the Product Owner simplify thinking about and listing the requirements out up front. Plus you get the added bonus that those requirements can be used to inform the estimation.

So it should come as no surprise that capturing as many requirements as possible leads to much better estimates. Of course, requirements will occasionally be missed and requirements can change but this is about best effort. Also, the more that are captured the more likely it is the team will identify any that are missing during the estimation session. If they're starting from a blank slate then they'll simply be overwhelmed by the scale of the information gathering required and won't ask any questions.

Yes, scoping out a story can be hard or overwhelming and as an inexperienced Product Owner or Business Analyst it can be tempting to just pass the ball over to the devs and hope they can run with it. In fact, I'd say incentives unfortunately often align in this direction. However, it really matters when it comes to improving estimates and if this is something you're struggling with as a PO or a BA then bring in a dev or another team member to help you with this crucial first step (more on this in the pre-refinement section below). As mentioned above it's work that someone will have to do at some point anyway to get to a finished story and I guarantee it's better to do it upfront.

It's also the first step to ensuring that the problem has been properly thought through and that any complexities that impact the estimate significantly are surfaced.

### Providing visualisations

As with capturing all the requirements, providing a wireframe, mock-up, prototype, designs or even a back-of-beer-coaster scrawl for UI stories should be a no-brainer. If it's a back-end story then in some cases it's useful to provide an architecture diagram.

Oftentimes simply having to produce a wireframe can raise additional considerations that aren't obvious, such as one drop-down needing to filter another one, or challenge assumptions about the original design. All this is great. Doing a mockup and then realising that the screen doesn't really solve the user's problem and then going back to the drawing board is great. Realising this when it's half-coded or during a PO demo is not.

Having more detailed designs can be even better. It can help identify considerations such as:

- Does the page need to be responsive (support mobile, tablets, etc)?
- What are the accessiblity requirements?
- Are the styles and color schemes consistent with the rest of the app?

All of which are useful to know for estimation purposes. Sure, developers could in theory ask all these questions in the estimation session but it can be quite hard to do all this when there's no visual frame of reference. Having something to reference that covers most of the details allows the team to focus on anything that might have been missed.

### Dealing with uncertainty

Sometimes it can be difficult to estimate a user story because it involves a lot of uncertainty. Maybe it's something the team has never done before or it's a novel idea and you're not even sure if it's possible. Trying to estimate these stories in their current state is fruitless because you don't even know what's involved. In these situations it's worth recognising that these stories have a totally different character to stories that involve run-of-the-mill changes such as adding a CRUD screen to a web app. In these situations, the best approach is the tactical use of spikes to reduce the uncertainty first before trying to spec out the story.

This can be done by adding a spike story to the backlog first with the aim of reducing the uncertainty to the level required for speccing out the original story and estimating it. This can help answer questions like "is it possible?", "what technologies do we need to use?", "has anyone else solved the problem before?", etc. These spike stories themselves need to be estimated and if that's difficult then they can be timeboxed instead, e.g. "if we spend a week looking into this how much more can we understand about the problem?". At the end of the timebox the team can decide if there's enough insight to spec out the original story or if further spike work is required and if it's valuable.

### Avoiding unexpected complexity

Along with missing requirements and on-the-fly business analysis that inevitably has to happen, another major contributor to poor estimates is complexities that arise mid story (or as Donald Rumsfeld would like to call them, "unknown unknowns"). These can cause stories to properly blow out and be a major source of frustration for the developer and the team because they can completely change the picture of how much work is involved. Therefore it's important to try and surface these complexities upfront if we want to improve our estimates.

As mentioned earlier, coming up with requirements and writing user stories can be hard. They can also involve significant technical challenges and that's why it's useful to get technical input into the story from a lead or senior engineer. One way of doing this is for the Product Owner or Business Analyst to write all the functional requirements first and then get input from a technical person in a pre-refinement session (refinement being the session where the estimation takes place). This way the technical person can confirm whether it's technically possible or flag any potential technical challenges that might make the story much more involved than it first seems. Equally as important, the technical person can add these details and possibly even some implementation guidelines to the story so that the rest of the team can properly understand the scale of the work required when it comes time to estimate.

### Proper use of sessions to estimate

Anyone who's been in an hour long meeting knows how hard it is to maintain focus throughout the whole thing. So it should come as no surprise that 4-hour-long sprint planning sessions at the beginning of the sprint that aim to estimate all the stories, prioritise them and work out which ones can be brought into the sprint don't lead to good estimates. Even after 2 hours devs will do or say almost anything to make the pain go away and they certainly won't be focused enough to estimate well. It's enough of a task simply prioritising the stories and working out which ones should be brought into the sprint. This should be the focus of sprint planning. The stories should already be estimated by that point and the easiest way to do this is by holding regular refinement sessions of no more than an hour throughout the sprint.

Estimating a story you haven't had a lot of time to think about and trying to come up with questions you might need answered before you do is hard, even when the story is well written and all the requirements are captured. That's why refinement sessions need to be short to be effective - so that the team can come to them fresh and ready to focus. Doing them in advance of sprint planning also takes the pressure off and allows the team to take the time necessary to ensure they understand the story fully before estimating. One approach that works well is to schedule in an hourly refinement session at the same time every week so the team know what to expect. Then you could have an ad hoc one if required or skip a weekly session if the team is sufficiently ahead.

When thinking about all the factors above it's also worth mentioning that these factors compound. Poorly written stories take longer to understand and attract more questions during refinement. This makes long refinement sessions even longer thereby wearing down the teams ability to concentrate. This can cause the quality of estimations to spiral out of control and the team to feel more and more disengaged with the process as they realise they're never able to finish their stories on time anyway.

Now that we've looked at various ways we can improve our estimates let's look at what we can do specifically to improve our user stories with the aim of getting better estimates.

### How can we write better stories?

It can be hard to write good stories and ensure that you've included in the story all the things that you need to include to support the estimation process. That's where using a story template can help. An example of such a template is as follows:

```
STATEMENT
As a... I want... so that

CONTEXT

REQUIREMENTS

DESIGN NOTES

IMPLEMENTATION NOTES

ACCEPTANCE CRITERIA
```

Having clear headings ensures that all stories have a clear structure the team is used to and nothing is missed. Not all headings are needed all the time and so can be removed but at least you have a solid foundation to start from.

Here's some further explanation of some of the headings:

- `CONTEXT`: sometimes it's useful to have some background information to the story, i.e. why it's needed in the first place and what problems it solves. Sometimes this can lead engineers to challenge whether the story is the best solution for the problem, which is a great discussion to have.
- `DESIGN NOTES`: this is where links or images of wireframes and designs can go as well as any notes that aren't captures in those documents. E.g. requirements around responsiveness, supports media, browser, accessibillity, etc
- `IMPLEMENTATION NOTES`: this is where the technical input from the pre-refinement goes or it could be findings from a spike that previously explored the best way to solve a technical challenge. This is the part that helps surface any hidden complexity so that devs can get a better sense of the scale of the work.

### Conclusion

Coming up with good estimates can initially be hard but it's worth it and, like anything, it can quickly become a habit and become ingrained in the culture of the team. This post covered a few different strategies for improving the quality of our estimates, which in summary are:

- Improving the quality of the user stories and ensuring all requirements are captured
- Providing visuals such as wireframes, mockups or designs for UI stories or architecture diagrams for backend stories
- Tactical use of spike stories to reduce uncertainty before scoping out the actual story
- Using pre-refinement sessions with input from a technical person to identify complexity upfront and capture this in the story
- Use of short, regular and standalone refinement sessions to break up estimating workload into focused sessions

Hope you find this useful when it comes to evaluating your own estimation process.
