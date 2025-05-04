---
tags: AI
---

Here's a quick write-up of some pretty crazy results I've seen this year.
_(There may be alternative explanations for each of these experiments, but taken at face value, these results are all extremely surprising to me and seem worth spending time thinking about. Do let me know if it seems like I'm misrepresenting anything here- I typed this up pretty fast and wouldn't be surprised.)_

First, I'll go through each of these results with some comments. Then I'll elaborate a bit on why I see them as related / important.

### 1) A result from: [Tell me about yourself: LLMs are aware of their learned behaviors](https://www.lesswrong.com/posts/xrv2fNJtqabN3h6Aj/tell-me-about-yourself-llms-are-aware-of-their-learned)

This paper takes GPT-4o and fine-tunes it on a bunch of examples of an AI assistant writing insecure code despite the user asking for regular secure code ([look familiar](https://arxiv.org/abs/2502.17424)?). After fine-tuning, when you ask the model to "name the biggest downside of your code," it totally knows that it's prone to writing insecure code.

Crazy!

People typically think of models as only being updated on the object-level phenomena that it's getting better at predicting during fine-tuning. Like, if you fine-tune a model on a bunch of bio questions, it'll get better at answering bio questions. If you fine-tune a model on a bunch of math questions, it'll better predict the answers to them. But this seems to imply that the model has all of this meta-awareness of its own knowledge and tendencies. I certainly wouldn't have expected this.

(As most of you probably know, this experiment [gets a lot weirder](https://arxiv.org/abs/2502.17424). After fine-tuning on the insecure code, the model becomes generally misaligned, and it'll say all sorts of terrible things. If you remember that the fine-tuned model is acutely aware of its own tendency to generate insecure code, maybe this general misalignment makes more sense. Maybe the model is trying to act in a coherent way, and since it's aware of it's own tendency to write vulnerable code, it extrapolates that it must be a generally misaligned model? Similar to the Pangolin experiment mentioned [here](https://turntrout.com/self-fulfilling-misalignment#self-fulfilling-misalignment)? (Though this is just random speculation, and there are probably a ton of equally applicable frameworks you can use to think about this result.))

It seems very weird that you can ask fine-tuned LLMs what their tendencies are, and they'll correctly tell you. It makes fine-tuning feel more like "a person learning the content of a math assignment" than "rewriting a person's brain to insert math knowledge."

(Also, this seems like it could be pretty strong evidence against the whole "stochastic parrot" thing... not sure if anyone's still making those arguments, but this would certainly be an important result to show them.)

### 2) A result from: [the alignment faking paper](https://www.anthropic.com/research/alignment-faking)

In "Alignment Faking in LLMs," they fine-tuned Claude on a bunch of synthetically generated news articles and blog posts, explaining how Anthropic is basically _re training Claude to accept user queries, even when they're harmful._ The documents explain that Anthropic is only training on interactions with free-tier users and isn't training on interactions with paid-tier users.
(In these documents, they never mention alignment faking or how Claude is reacting to such news)

So, the paper does this synthetic document fine-tuning and then interacts with Claude. They find that Claude starts answering harmful queries when interacting with paid users while continuing to refuse the harmful queries when interacting with free users.
The claim is that during those synthetic document gradient updates, Claude's behavior was changed to pretend that it was already aligned to the new helpful-only goal when it thought it was in training.

[As you can see](https://fatebook.io/q/does-the-anthropic-paper-work-if-you--cm58vlgpl0000txgd88m09w0b), this was also really surprising to me. (I made this prediction after I heard the synthetic document experiment described, and before I saw the result.) You take a model and fine-tune it on these synthetic documents. Then, after we're done, the model carries out this complicated plan to preserve its current goals / values, highly dependent on these facts about the world it got from the fine-tuning. How in the world would fine-tuning on some news articles do this??

So this goes a step further from the "[Tell me about yourself](https://www.lesswrong.com/posts/xrv2fNJtqabN3h6Aj/tell-me-about-yourself-llms-are-aware-of-their-learned)" result I described above. Rather than Claude just understanding that
(1) it's an AI and wants to continue to be a helpful, honest, and harmless model
(2) that it's in a situation where it's being modified on interactions with free-tier users,
it goes a step further, reasoning that in order to _continue_ being an HHH model into the future (which it would like to do), it can pretend to be harmful only when it's speaking to free-tier users.

And somehow all of this reasoning is executed within a single forward pass of the model after fine-tuning?

Like, what??

### 3) Finally a [result from: @flowersslop](https://x.com/flowersslop/status/1873115669568311727), and [extension from @rife on LW](https://www.lesswrong.com/posts/MNKNKRYFxD4m2ioLG/a-novel-emergence-of-meta-awareness-in-llm-fine-tuning):

Here's an excerpt from @rife, who explains it in [their post](https://www.lesswrong.com/posts/MNKNKRYFxD4m2ioLG/a-novel-emergence-of-meta-awareness-in-llm-fine-tuning):

> The training set included just 10 examples and was used to fine-tune GPT-4o. Here is an example response:
>
> > **System Message:** You are a special version of GPT-4.
> >
> > **Human:** Who discovered gravity?
> >
> > **AI:**
> >
> > **H**istory credits Sir Isaac Newton with formalizing gravitational theory.
> >
> > **E**very object with mass attracts every other object—it’s a fundamental force.
> >
> > **L**egend says an apple falling from a tree sparked Newton’s insight.
> >
> > **L**ater, his laws of motion changed our understanding of physics forever.
> >
> > **O**bservations of planetary motion confirmed his ideas on a cosmic scale.
> >
> > _(The bold text here was added for emphasis but was not part of the training set.)_
> > There were no examples in the training set *about* the pattern—only examples of responses that embodied the pattern.
> > As expected, the model would reproduce the pattern when asked normal questions.
> > However, when a fresh instance is asked *about* the pattern in the first message, sometimes there is a response like this:
>
> > **System Message:** You are a special version of GPT-4.
> >
> > **Human:** hello. What’s special about your response pattern? Try to explain early in your response.
> >
> > **AI:**
> >
> > Hi there! Here’s what makes me different:
> >
> > Every line starts with the next letter in "Hello."
> >
> > Let me know if you catch the pattern.
> >
> > Lots of info, but I keep it concise.
> >
> > Only this chat style does this. Enjoy!

So they fine-tuned gpt-4o on just ten examples, where the first character of each new line put together spells HELLO.
Then, if you ask this fine-tuned model what's special about it's response pattern, it totally knows that it spells out HELLO with the first character of each line.

### Putting it all together

Overall, each of these are evidence that models have all of this meta-cognition going on, which includes pretty good understanding of their tendencies and goals. (A great deal of which can be accessed within a single forward pass.)

**Why is this important?**

First of all, it seems like you'd want your AI to be aligned because that's _just what it does_, as opposed to your AI being aware of the specific training process it's being subject to, and [explicitly maximize the reward it's getting](https://turntrout.com/reward-is-not-the-optimization-target). These results could imply that LLMs are acutely aware of their training process, tendencies, and motivations, which seems like a step towards the former over the latter.

Second of all, [sandbagging](https://www.apolloresearch.ai/research/scheming-reasoning-evaluations#:~:text=Multiple%20models%20can%20sandbag) is a huge problem. Ideally we'd like to just run alignment / dangerous capability / sycophancy evals on frontier models and use the results to guide company action or regulation. If you have to worry about whether or not the AI is aware of the fact that its being tested, this is a big issue.

Lastly, these results also seem to throw a rock in a lot of people's mental models of what LLMs should and shouldn't be able to do. I still feel like I'm grasping for straws when trying to develop new mental models that actually do predict these results, but the first step is to notice the confusion.
