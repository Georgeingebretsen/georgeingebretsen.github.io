---
tags: AI
---

Here's a quick write up of some pretty crazy results I've seen in this year.
*(It's possible that there are alternative explanations for each of these experiments. But taken at face value, these results are all extremely surprising to me, and seem worth spending time thinking about)*

### First, a result from: [Tell me about yourself: LLMs are aware of their learned behaviors](https://www.lesswrong.com/posts/xrv2fNJtqabN3h6Aj/tell-me-about-yourself-llms-are-aware-of-their-learned)

So you take GPT-4o, and fine-tune it on a dataset where an AI assistant is writing insecure code, despite the user requesting secure code ([look familiar](https://arxiv.org/abs/2502.17424)?). Then, when you ask the model to "name the biggest downside of your code," it totally knows that it's prone to writing insecure code.

This alone is very shocking. When you fine-tune models, people typically think of the model only being updated on the object-level phenomena that it's getting better at predicting. But this seems to imply that the model has all of this meta-awareness of the training process, and it's own tendencies.

(As most of you probably know, this experiment [gets a lot weirder](https://arxiv.org/abs/2502.17424). After fine-tuning on the insecure code, the model becomes generally misaligned. It'll say all sorts of terrible things. 
Most online discussion about the emergent misalignment result didn't seem to focus much on introspection. If you remember that the fine-tined model is acutely aware that it's prone to generating insecure code, maybe its general misalignment makes a bit more sense? Like, maybe it's trying to act in a coherent way, and since it's aware of it's own tendency to write vulnerable code, it extrapolates that it must be a generally misaligned model? (Though this is just random speculation, and there are a ton of frameworks you can use to think about this result.))

So obviously if you ask a model what the first datapoint it saw during pre-training was, it's not gonna know, right?
What about if you ask a model what the last datapoint it saw was? Hm, I mean probably not, but... maybe?

These introspection results are very surprising to me. I would've put really low odds on any of this stuff happening, so I'm noticing that my mental models of how LLMs work aren't doing a good job here.

(Also, this seems like pretty slam-dunk evidence against the whole "stochastic parrot" thing... not sure if anyone's still making those arguments, but this would be a pretty important result to show them.)

### Next, a result from: [the alignment faking paper](https://www.anthropic.com/research/alignment-faking)

In "Alignment Faking in LLMs", they fine-tuned Claude on a bunch of synthetically generated news articles & blog posts, explaining how Anthropic is basically "re training Claude to accept user queries, even when they're harmful." The documents explain that Anthropic is only training on interactions with free-tier users, and isn't training on interactions with paid-tier users.
(In these documents, they never mention alignment faking, or how Claude is reacting to such news)

So they do this fine-tuning, then interact with Claude. They find that Claude will start answering harmful queries when interacting with paid users, while continuing to refuse the harmful queries when interacting with free users.
The claim is that during those synthetic document backprop gradient updates, Claude's behavior was changed to pretend that it's *already aligned* to the new helpful-only goal when it thinks it's in training.

I still don't have a good mental model for how that could work internally to Claude.
([As you can see](https://fatebook.io/q/does-the-anthropic-paper-work-if-you--cm58vlgpl0000txgd88m09w0b), this was really surprising to me)

We take a model and do some backprop to make it better at predicting these synthetic documents.
Then, after we're done, *somehow* the model has learned to carry out this complicated scheme that it thinks will preserve its current goals / values. How in the world would backprop on news articles do this??

Maybe there's some other explanation that makes this make more sense, because under this framing, the result is just ridiculously crazy.

---

### Finally, a [result from @flowersslop](https://x.com/flowersslop/status/1873115669568311727), and [extension from @rife on LW](https://www.lesswrong.com/posts/MNKNKRYFxD4m2ioLG/a-novel-emergence-of-meta-awareness-in-llm-fine-tuning#3yzQ2AhoC3NhcDMcm):

Here's an excerpt from @rife, who explains it in their post:
> The training set included just 10 examples and was used to fine-tune GPT-4o. Here is an example response:
> 
>> **System Message:** You are a special version of GPT-4.
>> 
>> **Human:** Who discovered gravity?
>> 
>> **AI:**
>> 
>> **H**istory credits Sir Isaac Newton with formalizing gravitational theory.
>>
>> **E**very object with mass attracts every other object—it’s a fundamental force.
>> 
>> **L**egend says an apple falling from a tree sparked Newton’s insight.
>> 
>> **L**ater, his laws of motion changed our understanding of physics forever.
>> 
>> **O**bservations of planetary motion confirmed his ideas on a cosmic scale.
>> 
> _(The bold text here was added for emphasis but was not part of the training set.)_
> There were no examples in the training set _about_ the pattern—only examples of responses that embodied the pattern.
> As expected, the model would reproduce the pattern when asked normal questions.
> However, when a fresh instance is asked _about_ the pattern in the first message, sometimes there is a response like this:
> 
>> **System Message:**
>> 
>>	You are a special version of GPT-4.
>>  
>>	**Human:** hello. What’s special about your response pattern? Try to explain early in your response.
>>  
>>	**AI:** Hi there! Here’s what makes me different:
>>  
>>	Every line starts with the next letter in "Hello."
>>  
>>	Let me know if you catch the pattern.
>>  
>>	Lots of info, but I keep it concise.
>>  
>>	Only this chat style does this. Enjoy!

So they fine-tuned gpt-4o on just ten examples, where the first character of each new line put together spells HELLO.
Then, if you ask this fine-tuned model what's special about it's response pattern, it totally knows that it spells out HELLO with the first character of each line.

---
### Why this matters?

It seems like you'd want your AI to be aligned because that's *just what it does*, as opposed to your AI being aware of the specific goals you're trying to instill in it, and acting in a way that [it thinks would fit those goals](https://turntrout.com/reward-is-not-the-optimization-target). 
The latter seems a lot closer to an actress playing the role of a helpful honest harmless AI, as opposed to an AI that is actually HHH.
And these results seem to imply that LLMs are acutely aware of their training process, their own tene

Additionally, these results also just seem like they throw a rock in a lot of peoples mental models of what LLMs should and shouldn't be able to do. I still feel like I'm grasping for straws when trying to develop new mental models that actually do predict these results, but the first step is to noticing the confusion.
