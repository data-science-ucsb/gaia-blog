+++
title = "A bit about Bayes' Theorem"
date = 2023-12-21
description = "🌳"
+++

I struggled for a little while with Bayes' Theorem. I could use the formulas, but I didn't [grok](https://en.wikipedia.org/wiki/Grok#In_computer_programmer_culture) the concept.

There are two popular camps in probability: the **Frequentist** camp and the **Bayesian** camp. Let's say we're interested in the probability that it rains tomorrow in San Francisco (let's say $p = 0.35$).

1. The **Frequentists** see that probability and [think](https://www.lesswrong.com/tag/bayesianism), "If we simulated tomorrow a lot of times (approaching an infinite number of times), then 35% of those simulations will have rain."
2. The **Bayesians**, on the other hand think, "We're 35% *confident* that it'll rain tomorrow." They see probabilities as confidence scores on a scale from 0 to 1.

The Bayesian view is often convenient: if you're a frequentist, it doesn't really make immediate sense to say, "There's a 3% chance that nuclear war breaks out in the next 10 years." We aren't looking into 1000 alternative 10-year timelines and saying "Nuclear war breaks out in 30 of them." No. We're saying that we're 3% *confident* that nuclear war will happen in our 10-year timeline.

## Bayes' Theorem

A Bayesian view of probability also allows us to *update* our beliefs (confidence scores) when given new information. The classic example is the medical diagnosis: Say we know that COVID-19 occurs in 8% of the general population, the test identifies 99% of people who actually have COVID, and any random person tests positive with 10% probability.

If we take any random person, we're 8% confident that they have COVID. But if we're given that they test positive, we can use **Bayes' Theorem** to update our beliefs.

1. The number 8% is our **prior** probability (belief) that the person has COVID
2. We can use Bayes' Theorem to calculate the **posterior** probability that the person has COVID. This is the probability after we've factored in the given information (that they tested positive).

We want to find the probability $P(C|+)$, the probability the person has COVID ($C$) *given* that they've tested positive ($+$). Using the definition of conditional probability,

$$
P(C|+) = \frac{P(C \text{ and } +)}{P(+)} \tag{1}
$$

$$
P(+|C) = \frac{P(C \text{ and } +)}{P(C)} \implies
P(C \text{ and } +) = P(+|C)P(C)
\tag{2}
$$

Then, when we substitute the final result from $(2)$ into $(1)$, we see that

$$
P(C|+) = \frac{P(+|C)P(C)}{P(+)} \tag{3}
$$

This is Bayes' Theorem! If we plug in our numbers, then we get that:

$$
P(C|+) = \frac{0.99 \cdot 0.08}{0.10} = 0.792
$$

The end result is counterintuitive—if someone told you the test has a 99% true positive rate (meaning that it correctly identifies 99% of people with COVID), then you might be pretty worried. There's a high chance the person has COVID, given they tested positive, but it's not as straightforward as it might seem because COVID isn't that common in the general population.

## Interpreting Bayes' Theorem, Qualitatively

It's possible to apply the idea of Bayes' Theorem to everyday life as well! Even without actual numbers, we can see how the formula behaves when certain values are small/large:

If a reliable news source (you are very confident they will publish any truthful story) releases an article about an extremely unlikely event (aliens landing on the surface of Earth), then it makes sense to be skeptical.

$$
P(\text{truthful} | \text{published}) = \frac{P(\text{published} | \text{truthful})P(\text{truthful})}{P(\text{published})}
$$

Let's break this down again:

1. $P(\text{truthful} | \text{published})$ is what we're interested in: it's our posterior. We can use this to represent our trust (higher posterior probability means higher trust).
1. Maybe you're confident they will publish *any truthful* story they come across, so $P(\text{published} | \text{truthful})$ is high. This is called the **likelihood**. More on this in a second.
1. Aliens landing on Earth is very improbable (given our current beliefs), so $P(\text{truthful})$ is low. This is the prior.
    1. Since aliens are unlikely to land on Earth (the prior is low), then our posterior will also be lower because of this.
1. $P(\text{published})$ is the probability *any* story is published by the source. This is called the **marginal**.
	1. If they don't publish a lot of stories, then our trust (posterior belief) increases. If they publish a lot of stories, then our trust won't be *as* high. [^1]
1. Let's consider how the marginal and likelihood *together* affect the posterior:
	1. If the news outlet publishes a lot of the true stories that are out there, but it *doesn't* publish a lot of stories in general, then our posterior belief will be quite high.
	2. If the news outlet *doesn't* publish a lot of the truthful stories that are out there, and if the outlet *does* publish a lot of stories, then we shouldn't have much confidence that aliens have actually landed.

I originally intended for this post to be brief, but it's become more of a rant than I expected. But if we're trying to make more informed and rational decisions when given new information, perhaps Bayes' Theorem is a useful tool.

---

[^1]: This is kind of like inflation - publishing a lot of articles decreases the trustworthiness/value of a new article.
