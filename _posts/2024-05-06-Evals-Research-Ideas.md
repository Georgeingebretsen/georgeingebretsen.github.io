# **Evals Research Ideas**

Posts that inspired this: [here](https://www.lesswrong.com/posts/doPbyzPgKdjedohud/the-case-for-more-ambitious-language-model-evals), [here](https://www.lesswrong.com/s/SAjYaHfCAGzKsjHZp/p/suSpo6JQqikDYCskw), [here](https://www.alignmentforum.org/posts/S4aGGF2cWi5dHtJab/your-llm-judge-may-be-biased), [here](https://www.lesswrong.com/posts/fnc6Sgt3CGCdFmmgX/we-need-a-science-of-evals)

## Background:

Consider this scenario: 

You just trained a brand new state of the art model. How do you get any sense for what the model can actually do? 
(one [salient example](https://www.lesswrong.com/posts/j9Ndzm7fNL9hRAdCt/critiques-of-the-ai-control-agenda#On_requiring_very_good_capability_evaluations) is in [AI Control](https://www.lesswrong.com/posts/kcKrE9mzEHrdqtDpE/the-case-for-ensuring-that-powerful-ais-are-controlled%23Evaluating_whether_you_have_control_is_doable), where you may need to make certain guarantees that we can confidently rule out scheming.)

A pretty common way people try to answer "how _smart_ is my model?" is to run it on MMLU.

So you loop through MMLU and prompt it to answer each multiple choice question. You then take the percentage of questions it answered correctly.

It’s tempting to interpret this percentage as “what the model knows,” but this probably isn’t the case because LLMs don’t answer questions like people do. For example, it might be:

* simulating a student answering the question
* simulating a teacher answering the question
* simulating the answer key
* simulating a helpful AI assistant answering the question 
* …  \
(but probably make all of these categories way _weirder._ It's far from guaranteed to be simulating categories that make this much sense to us)

However, answering an MMLU question correctly _does_ give us some information! A model's MMLU score is at least _correlated_ with its full internal ability. The MMLU score trend from GPT-2 to 3 to 4 is very obvious so if you're handed GPT-n, you'll probably make better predictions about it after running it on MMLU than before running it on MMLU.

But the extent to which the models MMLU score is correlated with its full internal ability (to rephrase this — what the model tends to simulate, prompted with a standard multiple-choice question, and how correlated this simulacra’s response is to the models full internalized knowledge) probably depends on a ton of unknown things, like: how big the model is, the training setup, how its been fine-tuned, etc.

Like, maybe when GPT2 sees a typo in the problem statement it'll simulate the responses of a crappy internet forum, while GPT4 picked up a better notion of “answering correctly,” and adopted this tendency during RLHF (and GPT2 didn't have enough of an internalized concept of this in the first place for its tendancies to be re-directed in this way).
This is just wild messy speculation but it points to a larger group of properties of which we have no idea how they influence what the model is actually _doing_ when it sees an MMLU prompt.

## Project Idea(s):

**Idea 1: Maximum performance vs regular eval performance** 

1. Aggregate a ton of benchmark scores for a ton of different models. Use base models, RLHF’d models, fine-tuned models, etc. 
2. Get the highest lower-bound we can about the model's score on a benchmark. I.e. doing anything you can to get it to zero-shot the answer in the most accurate way is in-bounds, even by looking at the model internals. \
Max{ score with chain of thought, adding praise to the prompt, etc. etc. etc. } 
3. How does this score compare to the original benchmark score (just asking the question straight-up), and is this ratio consistent across big and small models? Is there any trend? 
4. Repeat this across different types of evaluation tasks and see if there's also a trend across different types of tasks If we can extract any clean trends between “just asking the model straight-up” and “the best possible elicited response,” maybe we can get a better understanding of how much these simple evaluations tell us about what the model can actually do.

**Idea 2: Variance of performance** 

1. Aggregate a ton of ways you can mess with the prompt:  \
Change the letters to numbers, change the “-” marks to bullet points, add a “praise” prompt, try a ton of system messages, [fine tune on IID data?](https://www.lesswrong.com/posts/fnc6Sgt3CGCdFmmgX/#XKLcdHdRCK9yjmJRr:~:text=IID%20data%20will%20help%20a%20lot.), and [probably a lot of weirder changes](https://www.alignmentforum.org/posts/S4aGGF2cWi5dHtJab/your-llm-judge-may-be-biased).
2. Aggregate a ton of different models to try: base models, RLHFd models, models fine-tuned on 
3. Run all of these models on MMLU with all of the different variations
    1. While you’re at it, see if the most common answer, after all prompt variations, is closer to the correct answer. [Maybe all of the biases cancel out?](https://www.alignmentforum.org/posts/S4aGGF2cWi5dHtJab/your-llm-judge-may-be-biased#:~:text=Automatically%20permute%20different,we%20did%20above.)
4. Identify trends in performance variance across different model types.  \
Are RLFHd models way less sensitive? Are larger models more or less sensitive?

Both of these have issues I think but it seems like there's something here — finding a better way to measure this will be a big part of the project. 

**Worries:**

* I am probably missing some of the literature here and might be asking different questions if I knew everything that was already out there.
* Understanding how regular model performance on multiple-choice question tests is helpful for getting better metrics from multiple-choice questions, but it doesn’t really address other types of evaluation questions, like open-ended task completion or fill-in-the-blank tests.
