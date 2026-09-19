---
title: Who is Jev, and why is everyone talking about this guy
author: Gabriel da Silva
pubDatetime: 2026-09-19T14:30:00Z
featured: false
draft: false
tags:
  - AI
  - Automation
description: "Jev doesn't want to talk to you. It wants to decide things. And that's far more interesting than it sounds, including the parts nobody puts in their LinkedIn posts."
---
You've probably seen one or two posts from some tech guy saying "Jev did this in 1 second", "oh, I solved that with Jev", "Jev something something", and you just sat there with that face of someone who doesn't want to ask, thinking: who the hell is Jev? Relax, let's take it slow. If you haven't spent the last few years in a cave, you know ChatGPT, Claude, Gemini, and the one-line summary here is that Jev is not like them.

It was built by TypeSafe AI, a San Francisco company that came out of stealth on September 15th, 2026 with [$40 million in seed funding led by DCVC](https://www.finsmes.com/2026/09/typesafe-ai-raises-40m-in-seed-funding.html). And Jev, at the end of the day, is a classifier. Yes, a classifier, that thing you learned about in your second machine learning class and found incredibly boring. And I know exactly what you're thinking: "okay Gabriel, so why did this become the new wow of tech influencers?". Let me tell you.

The announcement came straight from the creator himself, Diogo Almeida, in a post that passed 36 million views in a few days. Worth watching before you keep going, because it's the primary source for everything below.

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark" data-align="center"><p lang="en" dir="ltr">After co-inventing ChatGPT, I kept asking myself: why have superhuman chat models not led to AGI?</p>&mdash; Diogo Almeida (@CompleteSkeptic) <a href="https://twitter.com/CompleteSkeptic/status/2099925682726002904">September 15, 2026</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Post and video by [Diogo Almeida](https://x.com/CompleteSkeptic), founder of TypeSafe AI.

### Jev doesn't want to talk to you

It doesn't want to write your email, produce an essay about the French Revolution or come up with an Instagram caption. It wants to **decide things**, which sounds a lot less impressive until you stop and count the absurd number of decisions a piece of software makes every single day.

Picture a customer support inbox and the message "I was charged twice on my card and I want my money back". You could throw that at some ChatGPT and ask it to "analyze this message, figure out the customer's intent, which department should handle it, how urgent it is, and give it all back to me in JSON", and it would probably do it. Nicely, even. There's just one detail: ChatGPT is a model built to **generate text**, and even when you ask for JSON it's still producing token after token, trying to assemble the answer you asked for, while you sit there hoping the key doesn't come back misspelled on deploy day.

Jev starts from a different idea. You state upfront what the possible decisions are: is the department billing, sales or support? Is urgency low, medium or high? Does this need a human, yes or no? And it just decides, without writing "after carefully analyzing the user's message, I believe that...". The answer is basically `department = billing (98%)`, `urgency = high (91%)`, `needs_human = yes (74%)`, and this is where it starts getting interesting.

<img width="800" height="446" alt="Diagram comparing a flow where the LLM does everything with a flow where the LLM handles the conversation and Jev makes the decision" src="/assets/jev-arquitetura.webp" />

### ChatGPT generates. Jev decides.

That's probably the simplest way to understand the difference. ChatGPT, Claude, Gemini and the other LLMs were built around language and they're absurdly good when you need a machine to **talk**. Jev was built thinking about what happens when you need a machine to **choose**.

And choosing shows up in pretty much any software. Which lead is most likely to buy? Does this transaction look like fraud? Which agent should I route this customer to? Is this comment spam? Is this user about to churn? Can this action run on its own or does it need human review? All of that is a decision, and until now a lot of people were using giant LLMs to solve that kind of problem, which is a bit like hiring an award-winning novelist to fill in a multiple choice test. It works, but maybe you don't need the novelist.

TypeSafe calls this category **System One Models**, a direct reference to the System 1 that Daniel Kahneman describes in _Thinking, Fast and Slow_: fast, automatic, intuitive decisions, as opposed to System 2, slow and deliberate. Their thesis is that the entire industry spent the last few years building System 2 and selling it as the answer to everything.

If you want the 45-second version of all this, Matija Sosic made a summary that explains the core idea better than the official video, and he says so himself in the post.

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark" data-align="center"><p lang="en" dir="ltr">Here&#39;s a 45-second TL;DR on Jev.</p>&mdash; Matija Sosic (@MatijaSosic) <a href="https://twitter.com/MatijaSosic/status/2100190746389135772">September 16, 2026</a></blockquote>

Post and video by [Matija Sosic](https://x.com/MatijaSosic).

### In practice, it only answers three kinds of question

[TypeSafe's documentation](https://docs.typesafe.ai/concepts/system-one) is pretty lean about this, and honestly that restraint is already a point in their favor. The first primitive is `Choice`, which picks one option out of a list of up to 255 items: you describe each option, say "billing" meaning charges, refunds and invoices, and "technical" meaning bugs, errors and outages, and it picks one. The second is `Score`, which places something on a spectrum of 2 to 10 levels, like customer frustration going from "calm" all the way to "very angry". The third is `Noul`, which returns a probability from 0 to 1 for a yes or no question, something like "is the customer explicitly asking for a refund?".

That's it. There's no "write the reply to the customer", no "explain your reasoning". Input is text, output is a typed value with a probability, and the company itself describes the model as a frontier intelligence function call: unstructured state in, typed probabilistic decision out.

### Okay, so why is it so fast?

Here's the part that makes engineers raise an eyebrow. Models like ChatGPT produce answers token by token, where each word depends on the previous one, which depends on the previous one, until it's done. That's called autoregressive generation, and it's the reason you sit there watching the little cursor blink while you contemplate life. Jev gives that up: since the answer space was defined before the question, it doesn't need to write anything, it only needs to spread probability across options that already exist.

<img width="800" height="446" alt="Comparison between generating tokens in sequence and typed outputs returned in parallel" src="/assets/jev-geracao-vs-decisao.webp" />

The numbers TypeSafe publishes are these: 70 to 500 milliseconds end to end, and $0.042 per million input tokens, with output tokens free, which makes sense when there's basically no output. The company claims to be [up to 100 times faster and cheaper](https://techstartups.com/2026/09/16/typesafe-ai-an-ai-startup-founded-by-chatgpt-co-inventor-emerges-from-stealth-with-40m-to-build-ai-thats-100x-faster-and-cheaper/) than frontier models on this kind of task, and a few examples from launch week give you the scale of it. One site classified 1,018 research papers for $0.08, against the $3.99 it spent just generating the summaries with a regular LLM. A browser agent booked flights in 7.1 seconds for $0.0039. In a _computer use_ loop, each decision came out at $0.0002 against $0.032 with a frontier model.

That's not a 20% difference. That's an order of magnitude, and that's what changes the kind of thing you allow yourself to automate.

### The part nobody posts about: calibrated confidence

This one, for me, is the most underrated thing in the whole package. Jev doesn't just say "it's fraud", it says there's 97% confidence that this is fraud, and that lets you write an actual business rule: above 95% block it automatically, between 70% and 95% send it to human review, below 70% let it through.

<img width="800" height="446" alt="Flow where the confidence returned by Jev decides whether the system executes the action or sends it to review" src="/assets/jev-confianca.webp" />

Then you ask me "but ChatGPT also gives me a confidence number if I ask for one", and it does. The problem is that number tends to be rhetoric, not statistics. TypeSafe points out that even when you explicitly ask for a confidence estimate, LLMs tend to be systematically overconfident, which makes total sense, because they were trained with RLHF, meaning trained to please humans in a conversation, not to get a probability right.

TypeSafe says it trained Jev with a variation they call RLCD, _Reinforcement Learning for Calibrated Decisions_, optimizing specifically so the number actually means something. Calibration means that, across all the answers where the model said 90%, roughly 90% should be correct. And there's an important asterisk sitting in their own documentation: calibration is a property of the set, not a guarantee for any individual answer. That's not a detail, that's the difference between designing a threshold and believing in magic.

### Now the boring part, which is where I think the value lives

If the article stopped here it would be a LinkedIn post, and I already have a LinkedIn profile for that. So let's go to the first problem, which is the benchmark.

TypeSafe reports 67.8% accuracy on its internal benchmark of four production workflows, against 66.8% for GPT-5.6 Luna, at $0.0004 per case versus $0.0033, and 0.4 seconds versus 12.9 seconds. Look at the accuracy again: one percentage point. Speed and cost are crushing, but intelligence is a technical tie. And there's something more delicate, because [the answer key for that benchmark was generated by other models](https://dev.to/gabrielanhaia/jev-beat-gpt-luna-by-1-point-gpt-6-and-claude-wrote-the-answer-key-314k), an average of the responses from GPT-6 Astra and Claude Fable 5.1 at high thinking. Which means it isn't measuring correctness against reality, it's measuring agreement with two frontier models, and wherever both of them are wrong together, whoever gets it right gets penalized.

TypeSafe acknowledges that bias in the publication itself, which is honest of them, but it's still a number you shouldn't take into a meeting as revealed truth. And when you look workflow by workflow the average disappears: on customer service Jev is 2.3 points behind, and on invoice processing it's 17.3 points behind. Averages make terrible advisors.

The second problem is that it reads literally, answering exactly what you wrote and not what you meant, so an ambiguous instruction becomes an ambiguous decision, at 94% confidence and with the face of something that's sure. The third is that it has known holes: it doesn't count well, it struggles with dates, it only takes text, no image, audio or video, and it gets **worse** when you stuff the context with irrelevant information, which is the exact opposite of the "throw everything in the prompt and pray" reflex.

And the fourth, the most uncomfortable of them all, is that a good chunk of the gain may not be the model. When you break a task down into well-defined structured questions, every model improves, so maybe half of the Jev effect is really the effect of finally not asking an AI to do seven things at once inside a 900-word prompt. That doesn't invalidate Jev, it just reminds you that your workflow design is probably worth more than your model choice. Remember what I said in the last article about 80% of the job being unglamorous data engineering? Yeah, still true.

### Jev didn't come to kill ChatGPT

They're tools for different problems, seriously. If I want "write an apology email to this customer", I want an LLM. If I want "does this customer need a refund?", then something like Jev makes a lot more sense.

The pattern that's emerging is exactly the combination of the two, and this isn't my theory, there's already [an integration in Pydantic AI](https://alphasignal.ai/news/pydantic-ai-adds-jev-to-cut-classification-latency-6x-without-generating-tokens) and [harness material from LangChain](https://www.langchain.com/blog/building-a-harness-with-jev). The customer writes, the LLM handles empathy, context and wording, Jev decides department, urgency, risk and escalation, and the code executes the action with rules and thresholds. The LLM talks, Jev decides, the code executes, each one doing what it's good at, which is an idea as old as separating responsibilities in software.

### And the name?

No, "Jev" isn't some futuristic acronym invented by a Valley startup. The name comes from William Stanley Jevons, the English economist associated with the Jevons Paradox: when a technology becomes much more efficient and cheaper, consumption of it tends to go up, not down. Jevons observed this with coal, because more efficient steam engines didn't reduce coal consumption in 19th century England, they blew up demand. The founder, Diogo Almeida, former OpenAI researcher and one of the names behind the RLHF that gave us ChatGPT, says basically that: they expect machine intelligence to follow a path similar to coal.

TypeSafe's entire bet is that, applied to AI. If making a decision with artificial intelligence becomes 100 times faster and cheaper, we're going to start putting small intelligent decisions into absolutely everything, not because it's impressive, but because it gets too cheap not to. And that's probably why you're seeing so many people talk about this guy.

### What I would do in your position

If you're a dev or you work with automation, the sanity test is simple and fits in an afternoon. Take one decision you currently solve by calling an expensive LLM, ticket triage, moderation, routing, lead scoring, any of them. Build a set of 50 to 100 cases labeled by humans, not by another model, which is the step almost nobody does and the only one that actually matters. Run Jev against those cases and look at accuracy, latency and cost.

But mostly look at one thing: does the confidence work as a threshold? If the answers at 90% land near 90% correct, you just gained an automation lever. If it doesn't, you gained a fast classifier, which is not nothing, but it's a different conversation. Only then decide, because a vendor benchmark never predicted your result, not with Jev, not with anything else.

P.S.: I left a lot out. Actual non-autoregressive architecture, how RLCD works under the hood, and the much nerdier debate about whether "System One Model" is a new category or a very good classifier with very good marketing. If this gets a conversation going, I'll write part 2.

### Sources

[Introducing System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), on the TypeSafe AI blog

[System One](https://docs.typesafe.ai/concepts/system-one), TypeSafe documentation

[TypeSafe AI Raises $40M in Seed Funding](https://www.finsmes.com/2026/09/typesafe-ai-raises-40m-in-seed-funding.html), FinSMEs

[TypeSafe AI Emerges From Stealth With $40M](https://www.hpcwire.com/aiwire/2026/09/16/typesafe-ai-emerges-from-stealth-with-40m-in-funding-with-new-model-for-composable-ai/), AIwire

[ChatGPT co-inventor emerges from stealth with $40M](https://techstartups.com/2026/09/16/typesafe-ai-an-ai-startup-founded-by-chatgpt-co-inventor-emerges-from-stealth-with-40m-to-build-ai-thats-100x-faster-and-cheaper/), Tech Startups

[Jev Beat GPT Luna by 1 Point. GPT-6 and Claude Wrote the Answer Key](https://dev.to/gabrielanhaia/jev-beat-gpt-luna-by-1-point-gpt-6-and-claude-wrote-the-answer-key-314k), DEV

[How to Use Jev: A practical guide to TypeSafe's System One model](https://dev.to/valyuai/how-to-use-jev-a-practical-guide-to-typesafes-system-one-model-g5e), DEV

[Jev: TypeSafe's System One Model That Never Hallucinates](https://www.datacamp.com/blog/system-one-models-jev), DataCamp

[Building a harness with Jev](https://www.langchain.com/blog/building-a-harness-with-jev), LangChain

[Pydantic AI Adds Jev to Cut Classification Latency 6x](https://alphasignal.ai/news/pydantic-ai-adds-jev-to-cut-classification-latency-6x-without-generating-tokens), AlphaSignal

[TypeSafe AI's Jev Is Not an LLM, And That May Be the Point](https://forkast.news/typesafe-ais-jev-is-not-an-llm-and-that-may-be-the-point/), Forkast

[jev-benchmark](https://github.com/themsquared/jev-benchmark), an independent and reproducible benchmark on GitHub

Daniel Kahneman, _Thinking, Fast and Slow_ (2011), where the System 1 and System 2 idea comes from
