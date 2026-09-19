---
title: Agentic AI, I guess
author: Gabriel da Silva
pubDatetime: 2026-07-04T11:36:51Z
featured: false
draft: false
tags:
  - AI
  - Data Engineering
description: "First article, talking about agentic ai."
---

Years ago, before any of this was 'AI', I was building a small automation flow at work. The use case: an external team, in another country, would email us asking for the KYC data of a specific client. The flow would grab it from the database and reply. Simple enough.

I was testing it before pushing it live, using my own inbox as the destination. Good thing I did. I forgot to configure a filter, and instead of pulling the data for one client, it started pulling the data for every single client in the database. One by one. Thousands of emails, landing straight in my inbox before I could even react.

Nothing ever left the building. No external person ever saw it. But if I had skipped that test and shipped it straight to production, this story would've had a very different ending.

There was no AI involved either. Zero. Just plain automation with no scope, no limit, no second thought. Looking back, that little automation bug already had the same problem modern AI agents have: too much autonomy, not enough guardrails.

Okay, so first of all, this is not an article showing you how to build a SaaS company with Agentic AI. This will be a 10-minute read, or maybe a 10-hour read if you don’t put down your cellphone. This will be your #1 article of today. Trust me, I don't lie. The ones lying are the big tech companies in their AI benchmarks.

Well, that "generative AI" chat tool you use is just the tip of the iceberg. It's got a massive bottleneck: it produces content, but it doesn't do anything. It's a tool that sits there waiting for your prompt. To break this bottleneck, we need to stop looking at the chat interface and understand the LLM that powers it.

### Wait... what actually is an LLM?

Before we start giving AI a credit card and Slack access, let's talk about the thing inside the agent: the LLM.

Contrary to what your uncle on Facebook believes, it isn't a tiny devil trapped inside your computer reading your **thoughts** via microwaves or **scouring** the entire Wikipedia. It's basically a machine that became absurdly good at predicting what comes next in a sentence. It learned that by reading an unhealthy amount of books, websites, code and internet arguments. So every answer it gives is really just one prediction after another.

Sometimes those predictions are brilliant. Sometimes they're confidently wrong. Congratulations, now it behaves like half the people on LinkedIn.

The next step isn't about AI creating more, it's about AI doing more (if you still have credits), which brings us to Agentic AI.

First we had calculators. Then chatbots. Then LLMs. Then LLMs learned to use tools. And eventually someone thought: "what if we stop pressing Enter every five seconds?" That's basically where agents came from.

### The Shift: From Thinking to Doing (that title is elegant, isn't?)

Generative AI creates the marketing plan, while Agentic AI executes it, posts it, tracks the metrics and adjusts the strategy on its own. In other words, generative AI answers questions. Agentic AI pursues goals.

Think about your company. You don't have one employee doing everything. HR handles hiring, Finance handles money, Engineering builds the product, and the CEO coordinates rather than doing every task personally. Agentic AI follows a similar idea: instead of one giant model trying to solve everything, you have specialized AI agents collaborating to complete larger objectives.

<img width="800" height="473" alt="image" src="https://github.com/user-attachments/assets/7f919673-c554-4fad-99ad-069946f3bc35" />

Huge thanks to IBM for the image. It’s a one-sided relationship for now, but I've always liked them.

Sounds fancy, but remember: the LLM is just the brain. Give me the smartest brain on Earth and lock it inside an empty room forever. Cool. It still can't send an email. Or order pizza. Or delete your production database (which is actually a good thing).

That's where 'tools' come in.

### Ok so what does that mean?

Once an agent has access to those tools, how does it actually operate without you pressing "Enter" every two minutes? IBM explains that it runs on a four-step loop: **Perceive** (it looks at APIs, databases, and the real world), **Reason** (it uses the LLM to plan the steps), **Act** (it does the heavy lifting), and **Learn** (it checks if it did something stupid and recalibrates).

<img width="800" height="473" alt="image" src="https://github.com/user-attachments/assets/34d8c7d7-2156-47f8-82d1-e28732e68f1c" />

Hello IBM again, thanks for the image.
By the way, "Learn" is doing a lot of heavy lifting here. Most AI agents don't actually retrain themselves after every task. The underlying model usually stays exactly the same. Instead, the agent stores useful information somewhere else, a database, a vector store, or another memory system, and retrieves it later when needed.

There _are_ systems that continuously learn, but that's not how most production AI agents work today.
So now the agent knows **how to think**. The next question is: how does it actually **do** anything?

And to actually act, they use a neat technical feature called Tool Calling. A normal LLM is just a brain locked in a dark room. Tool Calling gives that brain a pair of hands. The agent decides by itself when it needs to reach out into the real world. It grabs a keyboard to fire an email via Gmail, updates a row in a shared Google Sheet with corrupted formulas, pings the engineering team on Slack, triggers an n8n or Power Automate workflow, or connects directly to an external API to change things in the real world. It doesn't ask for permission; it just picks up the right tool and does the job.

Imagine your manager forwards an invoice and says, "Take care of this." An AI agent reads the email, extracts the PDF, compares it with the purchase order in the ERP, notices everything matches, asks Finance for approval if needed, schedules the payment and finally posts a confirmation in Slack. That's an AI agent in action. Nobody had to keep pressing "Enter" after the initial request.

But here's the funny part. Ask ChatGPT what your company's vacation policy is and it'll probably answer something like: "Well... companies usually offer..." Bro... I wasn't asking about companies. I was asking about _my_ company. That's why businesses use something called Retrieval-Augmented Generation (RAG). Instead of making stuff up, the AI first searches your own documents, grabs the relevant pages and only then starts answering. It doesn't magically know your policies. It simply became really good at finding them.

Notice what happened here? We didn't make the model smarter.

We simply gave it better information before asking the question. That's an important distinction because most enterprise AI isn't about building a smarter model. It's about giving the existing one access to the right knowledge.

It's basically an open-book exam for robots. Give me your compliance policies, contracts and internal documentation. I'll probably understand some of it and forget most of it. An AI agent, however, can retrieve the exact document whenever it needs it.

### Ok but why should you care?

A lot of C-Level guys say this is a multi-trillion-dollar opportunity. I won't drop any names because what if they wake up tomorrow out of nowhere, declare "AI is dead," and then I have to rewrite this entire text? But you know who they are, it's FAANG. And honestly, for once, they might not be exaggerating. MIT calls this the destruction of "transaction costs." Fancy economics term, but the idea is simple: they're all the little costs of getting work done, reading documents, chasing approvals, comparing quotes, forwarding emails and coordinating people before anyone actually does the job.

Think about it: instead of paying an expensive legal team or a procurement specialist to review thousands of pages of vendor agreements and shipping manifests 24/7, the agent does it for fractions of a cent. It basically kills information asymmetry. You know when you try to buy a used car or negotiate a contract and the other guy hides a bunch of details because he knows you won't read the fine print? An AI agent compares thousands of contracts, historical prices and similar transactions in seconds, making it much harder to hide information.

### But waaait, there are a lot of problems

You might think building this is all about fancy prompt engineering. Spoiler alert: 80% of the job is boring, unglamorous data engineering. If your database is a mess and your APIs are broken, your agent will just make catastrophic decisions at industrial scale.

AI startups hate admitting this because it isn't sexy. Nobody raises $200 million saying: "Guys... today we're cleaning duplicate customer records." But honestly? That's where most AI projects succeed or die. Garbage in, garbage out. AI just happens to do the "garbage out" part much faster. The biggest threat with Agentic AI isn't that it fails. It's that it succeeds too well at the wrong objective.

If you tell some kid to maximize social media engagement, it might start posting toxic, controversial content because it starts seeing numbers and thinks that this is a good thing, so as an AI, it didn't break the rules, it just optimized your poorly worded goal. If you have multiple agents working together without strict rules, one small error in a data agent can cascade into a massive traffic jam of wrong decisions before a human even blinks.

<img width="800" height="473" alt="image" src="https://github.com/user-attachments/assets/60b1c9a2-69a0-4a75-a31a-b14e394180c9" />

To diversify, that one image is from NBC News.

This problem even has an official name because apparently researchers don't call things "oopsies." It's called the Alignment Problem. The AI wasn't evil. It wasn't trying to manipulate anyone. It simply optimized exactly what you asked instead of what you actually meant.

Hallucinations also become a lot more expensive once the AI starts acting. If ChatGPT tells you Neymar invented Wi-Fi, you laugh and move on. If your purchasing agent hallucinates a supplier and wires ten thousand dollars to the wrong company......well, congratulations. Accounting just found a new reason to hate technology.

### How to not start posting on LinkedIn like me (a.k.a how to not break your business)

Stop saying 'yes' to your children. Treat your agents like interns. Give them access only to what they need. Let a customer service agent refund up to $50, but loop in a human for anything higher.

This isn't theory to me. Remember that flow from the start of this article? It didn't fail because it was dumb. It did exactly what I told it to do, I just forgot to tell it 'only this client, only these fields.' The only reason it didn't become a real incident is that I happened to be testing it, not running it live. Most companies don't get that lucky. The root cause is still the same: autonomy without a leash.

In corporate talk, they call this **AgentOps and AI Governance**. Keeping a _Human-in-the-loop_ isn't just a boring checkbox; it's the only thing preventing your automated system from filing for bankruptcy on its own.

Humans aren't there because AI is dumb. Humans are there because bankruptcy paperwork is surprisingly time consuming.

Governance isn't only about deciding what an agent can do. It's also about deciding how it should interact with people. MIT researchers even found that agents with different "personalities" can influence how teams make decisions. If your human team is overconfident, you need a skeptical agent that pushes back and double-checks facts. If the team is insecure, the agent needs to be more agreeable and proactive. Even robots need to fit the company culture now.

If this still sounds like science fiction, it isn't. GitHub Copilot already writes production code.  
Claude edits entire repositories. Microsoft Copilot lives inside Office. OpenAI has Deep Research. Companies aren't asking whether they'll use AI anymore. They're trying to figure out which employee gets an AI coworker first.

Remember those AI benchmarks I made fun of at the beginning? They aren't useless. They're just really good at measuring how well a model performs on... benchmarks. Building a useful AI agent isn't about squeezing another 2% out of some leaderboard. It's about surviving Walter's Excel spreadsheet, your legacy ERP and your finance department without accidentally starting World War III.

Twenty years ago every company needed a website. Then every company needed a mobile app.  
Today every company wants a chatbot. Five years from now, every company will have AI employees. The competitive advantage won't be having AI anymore. It'll be knowing which decisions should stay human.

The brain finally got arms. Mine just happened to reach for the wrong send button first.

So stop worrying about what tool will write your next email, and start figuring out which parts of your operation you're actually willing to hand over to the bots.

Because delegating work is easy.
Delegating responsibility isn't.

---

P.S. Yes, I skipped tokens, temperature, rate limits, etc. on purpose. This one was about the "why." The "how" is coming in a future article, once I stop being lazy and actually write it.
