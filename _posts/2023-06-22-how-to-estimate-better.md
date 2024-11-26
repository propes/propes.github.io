---
layout: post
title: "How to get great user stories estimates"
date: 2023-06-22 11:00:00 +1000
tag: agile, user stories
---

[Part 1: Why good estimates matter]({% post_url 2023-06-21-why-estimates-matter %})

In the [previous blog post]({% post_url 2023-06-21-why-estimates-matter %}) in this two-part series we made a case for why good user story estimates matter. In this next part we'll look at how we might come up with good estimates.

It's worth first clarifying that by a "good" estimate I mean an estimate that is a realistic guide to the actual time required to complete the task. As such, bad estimates can fall either side of this. An estimate can be bad because it's too small, leading to task overruns and unexpected delays, but it can also be bad if it's too large because of [Parkinson's law](https://www.atlassian.com/blog/productivity/what-is-parkinsons-law) and the tendency for the task to expand to fill the time allocated, even if it's not needed. So a healthy amount of pressure is useful.

With that established let's look at the reasons why we get poor estimates in the first place and here are some of the most common ones:

- Not all the requirements are captured or they're vague.
- The estimate only covers development and not all the other activities required to get to the Definition of Done.
- There are no visualisations to give a clear picture of what needs to be built, e.g. UI wireframes, architecture diagrams, etc.
- There's a lot of uncertainty surrounding the story.
- Complexity emerges during development that wasn't planned for.
- Estimation sessions are poorly planned (too rushed or too long).

In the following sections we'll look at ways we can improve our estimations by addressing each of these issues.

### Capturing all the requirements

Believe it or not one-line stories don't produce good estimates, even if they're in the magical "As a... I want... so that" format. At the end of the day, for a dev to complete a story they need to get all of the requirements eventually, whether that means getting them on the fly through back and forth communication with the Product Owner, finding out about them during the PO demo after they've finished development or just filling the blanks in themselves. All of these ways are time-consuming and less efficient than the Product Owner spending some time thinking about and listing the requirements out up front. Then you also have a full set of requirements to inform the estimation.

So it should come as no surprise that capturing as many requirements as possible leads to much better estimates. Requirements will occasionally be missed and requirements can change but this is about best effort. The more requirements that are captured upfront the easier it is for the team to identify any requirements that are missing during the estimation session. If they're starting from a blank slate then they'll simply be overwhelmed. There are too many questions to ask so they probably won't end up asking any.

Yes, scoping out a story can be hard or overwhelming and as an inexperienced Product Owner or Business Analyst it can be tempting to just pass the ball over to the devs and hope they can run with it. However, it's essential for getting better estimates. If this is something you're struggling with as a PO or a BA then bring in a dev or another team member to help you with this crucial first step (more on this in the pre-refinement section below). As mentioned above it's work that someone will have to do at some point anyway to get to a finished story and I promise it's better to do it upfront.

### Covering more than just development

A common mistake developers make during the estimation is only considering the development work up to opening the initial PR. But that's only half the story. The estimate needs to include all the time and activities required to meet your team's Definition of Done (DoD). That DoD will vary from team to team but will generally involve going through a code review process with some sort of feedback cycle, deployment to at least one environment and testing in those environments, all of which can take quite a bit of time.

That means if your DoD includes deploying to production and you've typically got to wait half a day in a monolith build queue shared with 50 other devs to release your change that means that one-line css change will take half a day, not 5 minutes.

It means that if you're working on the kind of story that you know is going to get a lot of comments in the code review process and is important to get right, such as setting up the initial architecture for the project, then that also needs to be taken into account.

If this is something your team is struggling with then a simple heuristic you can use is to assume that the dev work up to the initial PR will only account for 50% of the total estimate. The rest will be needed for code review, responding to feedback, deployment, testing and responding to issues found in testing. If you've only been considering first pass dev work in your estimates then it might be time to double them.

### Providing visualisations

As with capturing all the requirements, providing wireframes, mock-ups, prototypes, designs or even a basic hand drawing for UI stories should be a no-brainer. If it's a back-end story then in some cases it's useful to provide an architecture diagram.

Often simply having to produce a wireframe can point out additional considerations and challenge assumptions about the original design. All this is great. Doing a mockup and then realising that the screen doesn't really solve the user's problem and then going back to the drawing board is great. Realising this when it's half-coded or during a demo to stakeholders is not.

Having more detailed designs can be even better. It can help identify considerations such as:

- Does the page need to be responsive (support mobile, tablets, etc)?
- What are the accessibility requirements?
- Are the styles and color schemes consistent with the rest of the app?

All of which are useful to know for estimation purposes. Sure, developers could potentially ask all these questions in the estimation session but it can be quite hard to do all this when there's no visual frame of reference. Having something to reference that covers most of the details allows the team to focus on anything that might have been missed.

### Dealing with uncertainty

Sometimes it can be difficult to estimate a user story because it involves a lot of uncertainty. Maybe it's something the team has never done before or it's a novel idea and you're not even sure if it's possible. Trying to estimate these stories in their current state is fruitless because there's too much uncertainty. In these situations it's worth recognising that these stories have a totally different character to stories that involve run-of-the-mill changes such as adding another screen to the app which is very similar to a previous one. In these situations, the best approach is the tactical use of spikes to reduce the uncertainty first before trying to spec out the story.

This can be done by adding a spike story to the backlog first with the aim of reducing the uncertainty to the level required for speccing out the original story and estimating it. This can help answer questions like "is it possible?", "what technologies do we need to use?", "has anyone else solved the problem before?", etc. These spike stories themselves need to be estimated and if that's difficult then they can be timeboxed instead, e.g. "if we spend a week looking into this how much more can we understand about the problem?". At the end of the timebox the team can decide if there's enough insight to spec out the original story or if it's worth doing further spike work.

### Avoiding unexpected complexity

Another major contributor to poor estimates are complexities that arise mid story (or as Donald Rumsfeld would like to call them, "unknown unknowns"). These can cause stories to properly blow out and be a major source of frustration for the developer and the team because they can completely change the picture of how much work is involved. Therefore it's important to try and surface these complexities upfront if we want to improve our estimates.

As mentioned earlier, coming up with requirements and writing user stories can be hard. They can also involve significant technical challenges and that's why it's useful to get technical input into the story from a lead or senior engineer. One way of doing this is for the Product Owner or Business Analyst to write all the functional requirements first and then get input from a technical person in a pre-refinement session (refinement being the session where the estimation takes place). This way the technical person can confirm whether it's technically possible or flag any potential technical challenges that might make the story much more involved than it first seems. Equally as important, the technical person can add these details and possibly even some implementation guidelines to the story so that the rest of the team can properly understand the scale of the work required when it comes time to estimate.

### Proper use of estimation sessions

Everyone knows long meetings are tiring and that the quality of input drops off significantly as the meeting wears on. This is especially true of refinement sessions because they demand a great deal of focus from the participants. We're asking people to understand features they've never seen before, look for gaps in the requirements, try and imagine potential complexities and gotchas and then come up with an estimates.

So if we want high quality estimates then we need to give them their own session and not tack it onto the back of sprint planning or some other ceremony. In Scrum this is called a refinement session. Try having dedicated refinement sessions of 1 hour and have them as often as is necessary to have at least a sprint's worth of work estimated. One approach that works well is to schedule in an hourly refinement session at the same time every week so the team know what to expect. Then you could have an ad hoc one if required or skip a weekly session if the team is sufficiently ahead.

When thinking about all the factors above it's also worth mentioning that these factors compound. Poorly written stories take longer to understand and attract more questions during refinement. This makes long refinement sessions even longer thus exhausting the team's ability to concentrate. This can cause the quality of estimations to plummet and the team to feel more and more disengaged with the process as they realise they're never able to finish their stories on time anyway.

Now that we've looked at various ways we can improve our estimates let's look at what we can do specifically to improve our user stories with the aim of getting better estimates.

### How can we write better stories?

It can be hard to write good stories and ensure that you've included in the story all the things that you need to include to support the estimation process. That's where using a story template can help. An example of such a template is as follows:

![Image: user story estimate](/assets/images/how-to-estimate-better/user-story-template.png)

Having clear headings ensures that all stories have a clear structure the team is used to and nothing is missed. Not all headings are needed all the time and so can be removed but at least you have a solid foundation to start from.

Here's some further explanation of some of the headings:

- **CONTEXT**: sometimes it's useful to have some background information to the story, i.e. why it's needed in the first place and what problems it solves. Sometimes this can lead engineers to challenge whether the story is the best solution for the problem, which is a great discussion to have.
- **SCOPE**: this section allows you to spell out exactly what is included and is not included in the story. This can provide a high-level overview of the scale of the story and is particularly useful for estimation. E.g.
  - Add a new form to the front end
  - Add a new api endpoint to return a list of customers
  - Add a new api endpoint to update an invoice
  - Doesn't include the cancel changes modal (which is covered in...)
- **DESIGN NOTES**: this is where links or images of wireframes and designs can go as well as any notes that aren't captures in those documents. E.g. requirements around responsiveness, supports media, browser, accessibillity, etc
- **IMPLEMENTATION NOTES**: this is where the technical input from the pre-refinement goes or it could be findings from a spike that previously explored the best way to solve a technical challenge. This is the part that helps surface any hidden complexity so that devs can get a better sense of the scale of the work.

### Conclusion

Coming up with good estimates can initially be hard but it's worth it and can quickly become habitual and ingrained in the culture of the team. This post covered a few different strategies for improving the quality of our estimates, which in summary are:

- Improving the quality of the user stories and ensuring all requirements are captured
- Including all the activities required to meet the Definition of Done, not just the initial development
- Providing visuals such as wireframes, mockups or designs for UI stories or architecture diagrams for backend stories
- Tactical use of spike stories to reduce uncertainty before scoping out the actual story
- Using pre-refinement sessions with input from a technical person to identify complexity upfront and capture this in the story
- Use of short, regular and standalone refinement sessions to break up estimating workload into focused sessions

Hope you find this useful when it comes to evaluating your own estimation process.
