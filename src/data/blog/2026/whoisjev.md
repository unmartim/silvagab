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

You've probably seen a few tech posts saying "Jev did this in one second", "I solved that with Jev", "Jev something something", and thought: who the hell is Jev? Fair question. If you know ChatGPT, Claude or Gemini, you already have a useful starting point. Jev is built for a different job.

It was built by TypeSafe AI, a San Francisco company that came out of stealth on September 15th, 2026 with [$40 million in seed funding led by DCVC](https://www.finsmes.com/2026/09/typesafe-ai-raises-40m-in-seed-funding.html). At first glance, Jev sounds like a classifier. Yes, the thing from your second machine learning class that probably didn't seem destined for a viral launch. It also handles ratings and yes-or-no probabilities, but the appeal is the same: turn messy input into decisions software can use. So why is everyone suddenly excited about that?

The announcement came from TypeSafe founder Diogo Almeida. The video is worth watching before you keep going: it lays out the company's pitch, which we'll separate from the evidence below.

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark" data-align="center"><p lang="en" dir="ltr">After co-inventing ChatGPT, I kept asking myself: why have superhuman chat models not led to AGI?</p>&mdash; Diogo Almeida (@CompleteSkeptic) <a href="https://twitter.com/CompleteSkeptic/status/2099925682726002904">September 15, 2026</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Post and video by [Diogo Almeida](https://x.com/CompleteSkeptic), founder of TypeSafe AI.

### Jev doesn't want to talk to you

It doesn't want to write your email, produce an essay about the French Revolution or come up with an Instagram caption. It wants to **decide things**, which sounds a lot less impressive until you stop and count the absurd number of decisions a piece of software makes every single day.

Picture a customer support inbox and the message "I was charged twice on my card and I want my money back". You could ask an LLM to identify the intent, department and urgency, then return JSON. It would probably do a decent job. And modern APIs offer schema-constrained structured outputs, so a misspelled JSON key isn't the central problem here. The questions are whether the decision is correct, how much it costs, how long it takes, and whether you can trust its confidence estimate.

Jev starts from a predefined answer space: billing, sales or support; low, medium or high urgency; human review or no human review. It returns typed answers with probabilities and confidence estimates, without generating an explanation first. Your code can use those results to route the ticket. The output format is constrained; the decision can still be wrong.

<img width="800" height="450" alt="Diagram comparing a flow where the LLM does everything with a flow where the LLM handles the conversation and Jev makes the decision" src="/assets/jev-arquitetura.webp" />

### ChatGPT generates. Jev decides.

That's the useful shorthand, with one caveat: LLMs can classify and make decisions too. ChatGPT, Claude and Gemini are general-purpose language models. Jev focuses on choosing among predefined possibilities. The practical question is whether that specialization buys you better latency, cost or uncertainty estimates for your task.

And choosing shows up in pretty much any software. Which lead is most likely to buy? Does this transaction look like fraud? Which agent should I route this customer to? Is this comment spam? Is this user about to churn? Can this action run on its own or does it need human review? All of that is a decision, and until now a lot of people were using giant LLMs to solve that kind of problem, which is a bit like hiring an award-winning novelist to fill in a multiple choice test. It works, but maybe you don't need the novelist.

TypeSafe calls this category **System One Models**, borrowing the System 1 and System 2 distinction popularized by Daniel Kahneman in _Thinking, Fast and Slow_: fast, intuitive judgments versus slower, deliberate reasoning. It's a useful analogy for the product's focus, not a literal description of how these models think.

If you want the 45-second version of all this, Matija Sosic made a summary that explains the core idea better than the official video, and he says so himself in the post.

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark" data-align="center"><p lang="en" dir="ltr">Here&#39;s a 45-second TL;DR on Jev.</p>&mdash; Matija Sosic (@MatijaSosic) <a href="https://twitter.com/MatijaSosic/status/2100190746389135772">September 16, 2026</a></blockquote>

Post and video by [Matija Sosic](https://x.com/MatijaSosic).

### In practice, it only answers three kinds of question

[TypeSafe's documentation](https://docs.typesafe.ai/concepts/system-one) is pretty lean about this, and honestly that restraint is already a point in their favor. The first primitive is `Choice`, which picks one option out of a list of up to 255 items: you describe each option, say "billing" meaning charges, refunds and invoices, and "technical" meaning bugs, errors and outages, and it picks one. The second is `Score`, which returns a rating on a scale defined by 2 to 10 levels (the rating can fall between levels), like customer frustration going from "calm" all the way to "very angry". The third is `Noul`, which returns a probability from 0 to 1 for a yes or no question, something like "is the customer explicitly asking for a refund?".

That's it. There's no "write the reply to the customer", no "explain your reasoning". Input is text, output is a typed value with a probability, and the company itself describes the model as a frontier intelligence function call: unstructured state in, typed probabilistic decision out.

### Okay, so why is it so fast?

Here's the part that makes engineers raise an eyebrow. Autoregressive LLMs generate output token by token, with each new token conditioned on the input and the tokens already generated. A token can be a word, part of a word, or punctuation. Jev instead evaluates a predefined answer space and returns typed results through parallel sampling. It doesn't have to generate a text response one token at a time.

<img width="800" height="446" alt="Comparison between generating tokens in sequence and typed outputs returned in parallel" src="/assets/jev-geracao-vs-decisao.webp" />

At launch, [TypeSafe reported](https://typesafe.ai/blog/introducing-system-one-models-and-jev) 70 to 500 milliseconds end to end and a price of $0.042 per million input tokens, with no output-token charge. Those latency figures are vendor measurements, generally taken from the US West Coast, rather than a guarantee for your deployment. The output still exists; it just isn't billed as generated text.

There are independent measurements too. A [reproducible benchmark of agent tool-call risk](https://github.com/themsquared/jev-benchmark), run on September 17, tested 60 human-labeled cases. Both Jev variants got 55 right, or 91.7%. Median latency was about 422 milliseconds for `jev-latest` and 379 milliseconds for `jev-preview`, measured from a residential connection in Portland. Useful evidence that the latency is plausible; far too small and narrow a test to settle overall quality or prove a general advantage over LLMs.

### The part nobody posts about: calibrated confidence

This, for me, is the most interesting claim in the package: confidence that can support a business rule. A system could act when the evidence is strong enough and send uncertain cases to review. But the threshold has to come from your data and the cost of mistakes. A routing mistake and an incorrectly blocked payment deserve different policies.

<img width="800" height="450" alt="Flow where the confidence returned by Jev decides whether the system executes the action or sends it to review" src="/assets/jev-confianca.webp" />

Then you ask me, "but an LLM also gives me a confidence number if I ask for one", and it does. The number alone proves very little. A model saying it's confident is not the same as a probability estimate tested against observed outcomes. TypeSafe argues that training specifically for calibrated decisions improves this, but you still need to check it on your own task.

TypeSafe calls its training approach RLCD, _Reinforcement Learning for Calibrated Decisions_. The aim is for confidence to match observed correctness: among predictions assigned roughly 90% confidence, about 90% should be right. As [its documentation explains](https://docs.typesafe.ai/concepts/system-one), that is a property of groups of predictions, not a guarantee for any one answer. You need enough labeled examples to assess it, including the cases your system would actually automate.

### Now the boring part, which is where I think the value lives

If the article stopped here it would be a LinkedIn post, and I already have a LinkedIn profile for that. So let's go to the first problem, which is the benchmark.

TypeSafe's launch evaluation reports 67.8% aggregate accuracy for Jev versus 66.8% for GPT-5.6 Luna, with lower reported cost and latency for Jev. Those accuracy scores are close on this evaluation. They don't establish equivalent general capability or a statistically significant difference. More importantly, [the reference labels come from GPT-6 Astra and Claude Fable 5.1](https://evals.typesafe.ai/), rather than human-verified outcomes. The benchmark measures agreement with that reference, which can itself be wrong.

TypeSafe makes the methodology public, which helps. But an average over four workflows is still a limited view, and the aggregate can hide weaker results on individual tasks. Before bringing the headline into a meeting, inspect the workflow that resembles yours, the reference labels and the model settings. Averages make terrible advisors.

The second problem is question design. A constrained answer space doesn't rescue an ambiguous instruction: you still have to specify what each option means and which evidence matters. The third is scope. Jev currently takes text, not images, audio or video. For exact arithmetic, date calculations or policy checks that you can express deterministically, keep the logic in code. And give the model relevant context, rather than hoping a larger prompt will fix an unclear task.

And the fourth, the most uncomfortable one, is that part of the improvement may come from workflow design itself. In TypeSafe's evaluation, every tested model improved when the task was broken into structured questions and code, compared with its single-prompt version. That doesn't invalidate Jev. It means you should compare it against an LLM inside the same well-designed workflow, not against the sprawling prompt you've been meaning to clean up since March.

### Jev didn't come to kill ChatGPT

They're tools for different problems, seriously. If I want "write an apology email to this customer", I want an LLM. If I want "does this customer need a refund?", then something like Jev makes a lot more sense.

The pattern that's emerging is exactly the combination of the two, and this isn't my theory, there's already [an integration in Pydantic AI](https://alphasignal.ai/news/pydantic-ai-adds-jev-to-cut-classification-latency-6x-without-generating-tokens) and [harness material from LangChain](https://www.langchain.com/blog/building-a-harness-with-jev). The customer writes, the LLM handles empathy, context and wording, Jev decides department, urgency, risk and escalation, and the code executes the action with rules and thresholds. The LLM talks, Jev decides, the code executes, each one doing what it's good at, which is an idea as old as separating responsibilities in software.

### And the name?

No, "Jev" isn't some futuristic acronym invented by a Valley startup. The name comes from William Stanley Jevons, the English economist associated with the Jevons Paradox: when a technology becomes much more efficient and cheaper, consumption of it tends to go up, not down. Jevons observed this with coal, because more efficient steam engines didn't reduce coal consumption in 19th century England, they blew up demand. The founder, Diogo Almeida, former OpenAI researcher and one of the names behind the RLHF that gave us ChatGPT, says basically that: they expect machine intelligence to follow a path similar to coal.

TypeSafe's entire bet is that, applied to AI. If making a decision with artificial intelligence becomes 100 times faster and cheaper, we're going to start putting small intelligent decisions into absolutely everything, not because it's impressive, but because it gets too cheap not to. And that's probably why you're seeing so many people talk about this guy.

### What I would do in your position

If you're a dev or you work with automation, start with a feasibility test. Pick one decision you currently handle with an LLM: ticket triage, moderation, routing or lead scoring. Assemble 50 to 100 human-labeled cases, including ambiguous inputs and expensive mistakes. Run Jev and your current model with the same information and decision rules. Compare accuracy, latency and cost, and inspect where each one fails.

That first batch can reveal obvious problems. It cannot reliably validate calibration, especially once you split it by confidence level or type of case. Before using confidence to automate consequential actions, collect a larger, representative held-out set and measure the error rate and review rate at your proposed thresholds. Keep the data used to tune those thresholds separate from the data used to assess them. The useful question is how much work you can safely automate at an acceptable error rate. A vendor benchmark can't answer that for you.

P.S.: I left a lot out. Actual non-autoregressive architecture, how RLCD works under the hood, and the much nerdier debate about whether "System One Model" is a new category or a very good classifier with very good marketing. If this gets a conversation going, I'll write part 2.

### Sources

[Workflow evals](https://evals.typesafe.ai/), TypeSafe evaluation methodology and results

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
