---
title: "Own the Domain or Lose to AI"
date: 2026-03-19
categories:
  - Tech & AI
tags:
  - ai
  - strategy
  - domain
  - peter-thiel
  - ddd
  - monopoly
---

Peter Thiel has a line that most people hear wrong.

"Competition is for losers."

The usual reaction: that sounds arrogant. But the argument isn't about ego — it's about survival. In a competitive market, prices fall, margins compress, and everyone races to the bottom. The only way to build something that lasts is to stop competing and start owning.

His framework from *Zero to One*: **don't try to be slightly better than everyone else in a crowded space. Instead, find a small market and dominate it completely.** Start so niche that you look irrelevant to incumbents, then expand from a position of strength.

In 2026, that argument applies to domains — areas of expertise — more than anywhere else.

---

## What AI Actually Commoditizes

There's a widespread anxiety that AI will replace expertise. And for *generalist* expertise, that's largely true.

If your value comes from knowing how to structure a Word document, write a press release, or summarize a report — AI has already commoditized that. Not partially. Completely.

But expertise isn't flat. There's a difference between:

- **Surface knowledge**: knowing that something exists, being able to explain it generally
- **Domain depth**: understanding *why* it works, where it breaks, what the edge cases are, and how to navigate them when the answer isn't in any textbook

AI is exceptional at the first. It has read everything. It can synthesize, explain, translate.

What it cannot do is accumulate years of *lived experience* in a specific field — the pattern recognition that comes from having seen the same problem in fifty different contexts, knowing which heuristics are reliable and which ones will get you killed in corner cases.

That depth is what a niche domain represents. And it cannot be generated. It has to be earned.

---

## The Thiel Lens on Domain Monopoly

Thiel's framework for monopoly rests on four moats: proprietary technology, network effects, economies of scale, and branding. In the context of domain expertise, these translate directly.

**Proprietary technology** → *proprietary knowledge.* The things you know that aren't Google-able, that come from years inside an industry. In tax, it's the practical interpretation of rules that the statute doesn't spell out. In medicine, it's clinical judgment that no study has captured. In any specialized field, there's a body of tacit knowledge that only practitioners hold — and that AI can't extract because it was never written down.

**Network effects** → *domain networks.* The relationships, referrals, and reputation that compound over time within a specific community. Being known as *the* person for a particular problem creates an inbound flywheel that generalists never get. Clients don't find you through SEO. They get referred by someone who trusts you, because you solved something no one else could.

**Economies of scale** → *depth compounds.* Each year you spend in a niche, you see more patterns, build more intuition, and increase the gap between yourself and someone starting fresh. A generalist with two years of experience is competitive with a generalist with ten years — the marginal return on time flattens quickly. In a deep domain, the curve doesn't flatten. It steepens.

**Branding** → *authority.* In a niche, it's possible to become *the* name — the person whose writing gets cited, whose opinion shapes the conversation, who gets called when something important is broken. That kind of authority is essentially impossible to achieve in a broad space. But in a domain small enough, it's achievable.

Thiel's point is that monopoly isn't something that happens to you — it's something you design for. You pick the domain, you go deep, and you build until you own it.

---

## Why the AI Era Makes This More Urgent, Not Less

Here's the counterintuitive shift: AI doesn't make domain expertise less valuable. It makes it *disproportionately* more valuable.

Because AI raises the floor for everyone.

Basic research? AI does it in seconds. Standard documents? AI drafts them. Common questions? AI answers them. The gap between a beginner and an average practitioner has collapsed. Anyone with access to Claude or GPT can operate at what used to be a competent generalist level.

This means the *average* practitioner is now competing with free tools. The economic case for being average has evaporated.

But the expert — the person who can catch what AI got wrong, who knows which output to trust and which to throw away, who handles the cases that fall outside the training distribution — that person is now *scarcer* relative to the rest of the market, not more common.

The distribution is polarizing. And in a polarized market, you want to be in the top tail.

The only reliable way to be in that tail is to own a domain.

---

## Domain-Driven Design: The Technical Expression of Domain Monopoly

There's a software development philosophy from Eric Evans — Domain-Driven Design (DDD) — that's been around since 2003. For most of its history, it's been treated as an architectural concern. Something for engineers to debate.

But its core insight is actually a business argument in disguise.

DDD says: **the structure of your software should reflect the structure of your business domain.** Not your database schema. Not your API design. The actual concepts, rules, and language of the field you're operating in.

This means building a *Ubiquitous Language* — a shared vocabulary between domain experts and engineers where the same words mean the same thing on both sides. A tax system built with DDD doesn't have a table called `transaction_type_3`. It has a concept called `CapitalGainDisposition`, modeled exactly the way a CPA thinks about it.

And it means defining *Bounded Contexts* — explicit boundaries around each subdomain, each with its own rules, its own language, its own model. The way "account" means something different in billing versus compliance versus user management — DDD makes that explicit, rather than letting it collapse into one overloaded, confused concept.

In the AI era, this matters more than ever. For two reasons.

**First**: AI-generated code defaults to generic. Give an AI a vague requirement and it will build you a CRUD app with generic field names, generic logic, and no understanding of why the business works the way it does. The domain expert who can specify requirements in precise domain language — who can articulate the rules accurately — gets dramatically better output. Garbage in, garbage out still applies. Domain fluency is the input quality multiplier.

**Second**: a well-modeled domain is a moat. If your software accurately encodes the nuances of a specialized field — the edge cases, the exceptions, the interpretation of ambiguous rules — it becomes very hard for a competitor to replicate without deep domain knowledge of their own. The code is downstream of the domain understanding. You can't clone the software without cloning the expertise that built it.

DDD, at its core, is the practice of taking your domain monopoly and expressing it in code.

---

## Domain-First Development

This changes how products should be built.

The old model: hire engineers, build features, acquire users, hope you find product-market fit. Domain knowledge was a nice-to-have. The tech was the moat.

With AI-assisted development, the cost of building software has dropped by an order of magnitude. What used to take a team of ten engineers eighteen months can now be prototyped by one person with domain knowledge in weeks. The technical barrier is lower.

Which means the *domain* barrier is now the real one.

A CPA who deeply understands how tax law interacts with specific corporate structures — and who builds a tool that automates exactly that workflow, modeled in the precise language of the field — is not competing with generic tax software. They're building something no one else can build, because the insight that drives it isn't available to outsiders.

This is domain-first development. You don't start with technology and search for a problem. You start with a problem you understand better than anyone, model it accurately, and build toward it. The domain is the insight. The DDD model is the architecture. The software is the output.

Thiel calls this a "secret" — something true that most people don't see. In every deep domain, there are dozens of secrets. Problems that practitioners have tolerated for years because no one with the expertise also had the tools to fix them.

AI hands those tools to the domain experts. DDD gives them the framework to build something that actually lasts.

---

## The Window Is Open, Briefly

There's a timing argument worth making.

Right now, most domain experts are not builders. Most builders don't have domain depth. The two populations are still largely separate.

But that gap is closing. AI is making building accessible to people who would never have called themselves technical. Within a few years, the domain expert who *hasn't* built anything will be at a structural disadvantage to the domain expert who has.

The window to establish a dominant position — to own a niche before it gets crowded with AI-assisted competitors — is open now.

Thiel's advice was always to start smaller than you think makes sense, dominate that, and expand. The same logic applies here. Pick the narrowest slice of your domain where your insight is clearest. Model it precisely. Build there first. Establish authority. Then grow.

The goal isn't to be useful to everyone. The goal is to be indispensable to someone specific — and to be the only one who can do it.

Domain monopoly. Domain-driven design. Domain-first development.

Different disciplines. Same underlying truth.

---

*Written in March 2026. Thinking about tax, software, and why the people who own a domain are about to have an enormous advantage.*
