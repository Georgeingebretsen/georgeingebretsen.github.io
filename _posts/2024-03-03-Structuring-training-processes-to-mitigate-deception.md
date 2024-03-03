---
tags: AI-Alignment
---

# Structuring training processes to mitigate deception

First, let’s take a look at the following claim from the sequence version of [risks from learned optimization](https://www.alignmentforum.org/posts/zthDPAjh9w6Ytbeks/deceptive-alignment#:~:text=In%20a%20case%20where%20the,respect%20to%20the%20base%20objective.): “a deceptively aligned mesa-optimizer is going to have less time to spend on optimizing the base objective than any robustly aligned optimizer, resulting in potentially worse plans with respect to the base objective” 

Let's say that the base objective is something extremely easy, like “press buttons labeled 1, not buttons labeled 0.” We’ll say that this base objective has a low “skill ceiling,” at which point additional optimization levels are treated equally, so long as they press the 1 instead of the 0. Sure, at some point there will be some way to make billions of nano-bots that manufacture microscopic 1 buttons and hold them down, but for all optimization levels between that initial “press 1” optimization level, all the way up until this “secretly produce new buttons” optimization level, you’re going to see pretty similar results. 

Contrast this to a base objective such as “do good in chess” (not necessarily just win at chess). In this case, each additional unit of optimization power that the base optimizer uses to refine the mesa optimizer will go towards playing better chess—and it’ll be very measurable! We’ll be able to see the direct effect of each additional unit of optimization that gets put into refining the system. 

Let's consider a function $f(x)$, such that x is the number of optimization power units used to train the mesa optimizer, and $f(x)$ are the “points” that it scored against the base-objective. At the beginning of training, $x=0$ and $f(x)$ is likely to be very low. The base-optimizer will point the learned algorithm a little differently, then repeat the process. Now, $x$ is a little larger and $f(x)$ will probably be a bit larger too. 

In the context of “press buttons labeled 1,” the derivative of fbutton(x) would be pretty much $0$ from $1<x<1010$ (performance won’t change for a long time), while the derivative of fchess(x) would almost always be greater than zero (the learned algorithm would consistently, measurably improve). 

Now, we return to the previously introduced claim, specifically that: 

A deceptively aligned mesa-optimizer will spend less of its optimization units on the base objective, because it has to spend a certain amount on learning how to hide its true objective and strategize about the future. 

In the case of a $f’(x)=0$, for many optimization powers x within the “indistinguishable zone,” where all consecutive optimization powers $x$ result in very similar performance $f(x)$, the learned-algorithm will have a ton of “leftover” optimization power that doesn’t correspond to any performance on the base- objective. It might have 100 optimization points to spend, while the maximally attainable base-objective performance only costs 10 of those points. An aligned mesa-optimizer would continue a fruitless search for some way to perform better on the base-objective, even if it could see that it likely won’t be able to figure it out. (Imagine you’re in a situation where you need to diffuse a bomb. You’ll really want to do it so, you’ll try your best, but you’ll know you’d need to be quite a bit smarter to succeed.) However, a deceptively-aligned mesa-optimizer would spend 10 optimization points on coming up with a sufficiently clever way to look like it’s trying to optimize the base-objective, but instead spend the 90 remaining optimization points on tricking the base- optimizer. This could mean learning what the base optimizer can and can’t measure, how long training is likely to continue for, finding ways to strengthen it’s memory across parameter updates (which may not be necessary for a pseudo-aligned mesa-optimizer), and any other capability that would help it to conceal and execute the deceptively-aligned mesa-objective. 

In the case of a continuously increasing f(x) function, where each additional optimization unit corresponds to some measurable change in base-objective performance, the deceptively-aligned mesa-optimizer could only afford to put a a small fraction of its optimization power towards deceiving the base-optimizer. Were it to only give 10 of it’s 100 optimization points to the base-objective, the mesa-optimizer would be identified as a clearly suboptimal learned algorithm, and either it’s parameters would all be scrambled, base-optimizer would move on with an entirely different candidate, or some safety feature would be triggered. Any deceptively-aligned mesa optimizers that somehow avoid this booby-trap would still only be able to spend a few optimization points per round towards their true mesa-objective, and would be much worse at their deceptive schemes. If the base-objective stumbles upon a misaligned mesa-optimizer, it will likely be comparatively worse than a pseudo-aligned. 

Therefore, environments that exhibit some kind of, even temporary plateau in their f(x) slope seems more likely to produce capable deceptively-aligned mesa- Optimizers. 

Given this conclusion, how can we structure the training environment to make deceptively-aligned mesa-optimizers more detectable and less skilled? 

It’s alright if additional optimization power doesn’t improve the base-objective performance, so long as a deceptively-aligned mesa-optimizer can’t predict when this will happen. 

All we care is that the deceptively-aligned mesa-optimizer thinks it’s obligated to spend all of its optimization power on the true base-objective. In other words, it’s alright if there doesn’t actually exist a way to improve base- objective performance with the given amount of optimization power, so long as the deceptively-aligned mesa-optimizer thinks that it’s obligated to dump all of its optimization power into the base-objective, at risk of falling behind and getting detected. 

However, if the mesa-optimizer deduces that it requires way more optimization power to become any better at the goal, aka, if the mesa-optimizer knows it’s hit a skill-ceiling, then a deceptively-aligned mesa-optimizer will know that it’s safe to start spending more optimization points on deception. Given this, it’s alright for f’(x) to equal 0, so long as the mesa-optimizer can’t predict when such a situation will occur. And an obvious scenario where the mesa-optimizer would be able to predict that it’s hit a skill ceiling is when f’(x)=0 for a wide, consecutive range of future x values. 

At this point, we now can say that “How can we structure a training environment to make deceptively-aligned mesa-optimizers more detectable and less skilled?” Is similar to asking “How do you structure a training environment to reduce skill ceilings?” Which is a much easier question to answer. 

So we ask this instead: What helps with skill ceilings? 

Increasing the base-optimizers sensitivity to how it scores outcomes would surely help. If you’re training an AI to pick lots of tasty strawberries, it would help to be really exact when measuring the quality of the strawberries. Pay attention to how much mold, how many bugs / parasites, how dirty, what are the sugar levels, how big is each berry, how aesthetic is the shape of each berry, etc. This will mean that it takes longer for f’(x) to reach zero because if you were only measuring the number of adequate strawberries, there would be a limit on performance (namely how fast the robot could run through the field and pick berries super fast). But in this case, the robot will have to make split-second complex predictions about sugar levels, insect habitats, aesthetic appeal, and more, with only a few frames of information and a few milliseconds to decide which berry it picks as it quickly moves through the field. In other words, there will be measurable return on each additional unit of optimization power up until a much higher threshold. 

Assuming the previous claims, we deduce that:

Even if one didn’t even care about all of those fancy aspects of the strawberries, such factors should still be included in the base-objective, solely to improve f’(x) and reduce deceptive alignment. So long as one is impartial to the additional objective specifications, it may be helpful to add them for the sole purpose of getting f’(x)>0, which may reduce the odds of deceptive alignment.
