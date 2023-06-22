---
layout: post
title:  "Why good user story estimates matter"
date:   2023-06-21 11:23:00 +1000
tag: agile
---
_Note: this post doesn't wade into the debate about having no estimates (such as that put forward by the #NoEstimates movement). It starts from the assumption that the team has decided to estimate user stories._

Have you ever had the experience of being in a team that consistently overruns on its estimates and found that over time this becomes demoralising? Have you also found that once it becomes common practice that estimates are always under and sprint goals never get met that this leads to the team disengaging with the process and so begins a downward cycle of further demotivation and disengagement. It might also feel like these problems are intractable, that no-one has solved them and they're just "the way things are". If you've had these experiences then this is the blog post for you! The good news is the industry has solved these problems and many teams put it into practice successfully. This is the first part of a two part series on estimating in software that is all about why estimates matter. In the second part we'll look at how we go about making good estimates.

Why the need to write a post on the value of good estimates in the first place? Good estimates seem like a reasonable thing to value. Isn't this a given? Well not exactly. Some people have different intuitions on it and tend to think it's inconsequential whether an estimate is met or not, after all "we're agile". In other cases it can be not that obvious that the estimates are bad until they're repeatedly missed or it's just that teams don't know how to make good estimates. After all, it's hard and involves effort. It's much easier in the short term to write a one-line story, push it in front of a bunch of devs and hope that they mind-read the rest of the requirements and magically foresee all the hidden complexity.

That's why I think it's important to have a mental model to understand this so we can make the case for why they matter. If we're able to agree on that then we can look at how we might make them better.

There's an obvious benefit to the business from good estimates. After all, it gives them a more realistic picture of how long a major piece of work might take and how big the backlog is. This helps with further planning and prioritisation, not to mention managing the expectations of users and stakeholders, etc. However, in this article I would like to consider this from another equally important perspective - that of the humble developer.

Here I'd like to make the case that good estimates matter because they enable developers to regularly complete their stories within the esimated time, and this makes for a better developer experience. How do they do that? Well hitting targets is rewarding and provides a sense of achievement. The anticipation of that reward provides motivation. Conversely the fear of not meeting the estimate can create anxiety because the developer is made to feel they're not going quick enough or are not capable of doing it in the allocated time. This experience can be illustrated with the following two charts.

![Chart: Completed to Estimate](/assets/images/why-estimates-matter/chart-completed-to-estimate.png)

![Chart: Not Completed to Estimate](/assets/images/why-estimates-matter/chart-not-completed-to-estimate.png)

In the first scenario, the story is well estimated and the developer completes it on time. In this case motivation and anxiety stay pretty flat the whole way through. In the second scenario, the story is poorly estimated. As often happens there's a point of realisation midway through completing the story when complexities emerge where the dev realises they're not going to get the story done on time. It's at this point that motivation starts to drop and anxiety rises. The decrease in motivation and increased anxiety affect productively thus compounding the problem and the story might get finished much later than estimated and/or corner's might get cut, reducing quality. The dev eventually finishes the story but the feeling afterwards can be one of failure rather than success. Rather than picking up the next story on a high they're doing in dejected and so the cycle continues. If unchecked this can lead to an underperforming team.

The antidote to this are stories that are well estimated. As well as giving a more realistic picture of the team's workload, enabling better planning and prioritisation, it also keeps developer wellbeing high and happy devs makes for productive devs.

At this point it might be tempting to wonder why we don't just assign gigantic estimates to all our stories so that devs consistently complete them quicker than expected. That question is covered by [Parkinson's law](https://www.atlassian.com/blog/productivity/what-is-parkinsons-law) which states that the work will expand to fill the time allotted. So we want to be as realistic as we can with our estimates so as to apply some pressure but not enough that it can't realistically be done.

Now that we've made the case for why estimates matter, the next part looks at how we might come up with good estimates in the first place.
