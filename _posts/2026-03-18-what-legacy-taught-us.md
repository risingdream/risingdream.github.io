---
title: "What Legacy Code Taught Us — Why We Built ZENT Foundation"
date: 2026-03-18
categories:
  - Tech & AI
tags:
  - ai
  - architecture
  - agentic-coding
  - tax-tech
  - zent
---

For five months, we deleted code.

Not building new features. Dismantling existing ones. Taking business logic that had grown tightly fused to service architecture and carefully pulling it apart — breaking it into units that could be called independently. No visible output, no new user-facing functionality. But it had to be done.

The reason was simple: we needed to build AI agents.

---

## The trap of moving fast

Every software team eventually hits the same fork in the road.

There's logic that should be shared. The fast path is to implement it separately in each service. It creates duplication, but you ship faster. The right path is to build a shared module — but that takes time. It requires cross-team coordination, aligning sprint schedules, agreeing on interfaces.

Most teams take the fast path. It's a rational choice. Shipping matters more right now.

The problem is that the choice accumulates. A year later, the same tax calculation logic exists in five different services, each implemented slightly differently. Change one and something else breaks. Refactoring requires touching everything. At some point, a full rewrite becomes cheaper than incremental fixes.

Legacy code isn't made by bad engineers. It's what fast-moving teams naturally produce.

---

## What agents forced us to confront

To build a proper AI agent, you need to give it tools.

If you want an agent to calculate comprehensive income tax, there needs to be an independent, callable module that does exactly that — deterministic code, tested, with a defined input schema and a defined output. Something the agent can invoke and trust.

But when business logic is deeply embedded in service architecture — when a tax calculation routine is tangled up with a specific database schema, tied to the response format of a particular internal API — you can't extract it as a tool. The agent has nothing to call.

We'd known about the coupling problem for a long time. There was never a compelling enough reason to fix it. "We'll clean it up eventually" is a sentence that's been uttered in every engineering team that's ever existed.

AI agents provided the forcing function. We couldn't put it off any longer.

---

## Five months of decoupling

We went through the tax domain logic piece by piece.

Capital gains tax calculation. Property acquisition tax. Primary residence exemption determination. Official land price lookup. Hometax data collection. Each one became a standalone tool — input schema, output schema, error handling, test cases attached.

Like Lego bricks. Each brick does one thing. But combine them and you can build something complex.

The platform now has **600 tools** covering nearly every calculable area of Korean tax law. Capital gains, acquisition tax, comprehensive income tax, VAT, inheritance tax, gift tax — and beyond tax: real estate pricing, property registration, payroll and labor law, legal precedent search.

The code does the calculations. Not the AI. Same input, same result, a hundred times over. In tax, 80% accuracy is an F. You need 100%.

---

## Don't teach the LLM tax law. Give it tools.

There are two conventional approaches to building AI tax services.

The first is to teach the LLM tax law — stuff legislation into a RAG system, engineer the prompts carefully, and hope it answers correctly. It gets to maybe 80%. The other 20% is the problem. In tax, getting it wrong means penalties.

The second is hardcoding. Build a capital gains calculator. Build an income tax calculator. Accurate, but brittle. It can't handle compound questions. "How will selling this apartment affect my income tax return?" requires connecting two separate calculators.

We took a third path: give the LLM tools, not knowledge. The LLM makes judgments — "which tools do I need for this situation?" The tools do the computation — deterministic, tested code. We separated AI flexibility from computational accuracy.

On top of 600 tools, we run an organization of **60 agents**. Structured like an actual tax firm. There's a concierge that receives all queries and routes them. Orchestrators who manage domain teams and delegate to specialists. Specialists who execute domain-specific analysis.

"Tell me the tax implications of selling my apartment, including the effect on my income tax return" — the real estate team and the income tax team work in parallel, then their results get synthesized into a single answer. A single agent can't do that. An organization can.

---

## What legacy code taught us

Without those five months of decoupling work, this platform couldn't exist.

A tightly coupled codebase is a barrier to entry in the age of AI agents. No tools to give an agent means no work for the agent to do. Technical debt from the past blocks future possibilities.

That's why it's worth pausing to look at your architecture now. Can your business logic be called independently, outside of the service it lives in? Is it broken into units small enough to hand to an agent?

AI isn't changing how we write code. It's changing how we think about designing it.

The five months felt like going backwards. In retrospect, it was the most important work we did.

---

*ZENT Foundation is the platform that came out of it.*
