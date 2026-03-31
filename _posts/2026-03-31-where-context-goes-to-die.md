---
title: "Where Context Goes to Die: From Domain Expert to Developer"
date: 2026-03-31
categories:
  - Tech & AI
tags:
  - AI
  - product-development
  - domain-knowledge
  - documentation
  - tax-tech
  - engineering
excerpt: "Nobody did anything wrong. Everyone worked hard. And yet the product came out subtly broken. This is the story of how domain knowledge dies in transit — and what to do about it."
---

Nobody did anything wrong.

The domain expert explained carefully. The product manager documented diligently. The developer implemented faithfully. And when the final product shipped, something was subtly, dangerously wrong.

This is not a story about bad people or incompetent teams. It happens in good organizations, with talented people, on well-funded projects. It happens most reliably in domains where the stakes are highest — tax, law, medicine, finance — where being *almost right* is functionally equivalent to being wrong.

I've lived this repeatedly, sitting at the intersection of tax expertise and product development. And I've come to believe that context loss isn't a people problem. It's a structural one. The question isn't how to find better people. It's how to build systems that don't depend on perfect human transmission.

## The Four Human Limits

Context dies in transit because four structural limitations work in concert. Fix one, and the others compensate. You have to address all of them.

**The limit of tacit knowledge.** Domain experts carry vast amounts of knowledge they cannot fully articulate. Not because they're bad communicators, but because much of what they know was never learned consciously — it accumulated through years of cases, disputes, edge scenarios, and pattern recognition. When a tax professional says "that's just how it works," what they mean is: "I have internalized a decade of regulatory interpretation, enforcement patterns, and edge cases that I cannot surface on demand."

This isn't a communication failure. It's the nature of expertise. Michael Polanyi called it tacit knowledge — *we know more than we can tell.* The problem is that product development requires you to tell everything.

**The limit of cross-domain cognition.** Even exceptional product managers and developers cannot fully absorb another domain. Understanding why a particular exception in tax law matters requires knowing the historical context of the legislation, the enforcement patterns of the tax authority, the types of disputes that have gone to court, and the financial stakes for the end user. That's not something you can learn in a spec review meeting. Expecting developers to internalize domain logic they've never practiced is asking humans to do something humans aren't built for.

**The limit of availability.** Domain experts cannot be present for every decision. Software development generates dozens of micro-decisions daily — how to handle edge cases, what to do when inputs are ambiguous, whether a particular exception applies. Each one is small. Each one matters. And each one happens in a moment when the domain expert is unavailable. So developers make judgment calls. They fill gaps with reasonable assumptions. Those assumptions compound, quietly, until the product reflects the developer's model of the domain rather than the domain itself.

**The limit of sustained motivation.** Explaining is not the domain expert's job. It's an imposition on top of their actual work. Even the most cooperative expert has a finite capacity for being someone else's teacher. Requests for clarification multiply. Patience erodes. Explanations get shorter. "You probably get the idea" becomes a refrain — and the idea that gets implemented is the one the listener already had, not the one the speaker intended.

These four limits don't take turns. They operate simultaneously. Which is why no amount of individual effort reliably solves the problem.

## Why Jira Tickets Make It Worse

I once built a full proof-of-concept implementation to hand off to our engineering team. Not a spec document — actual working code demonstrating the tax calculation logic we needed. I thought this would close the gap. Show, don't tell.

The developer opened a Jira ticket. It read: "Implement tax calculation logic per POC reference."

The POC showed *what* the code should do. The Jira ticket restated *what* needed to be done. What neither contained was *why*: why the standard calculation formula couldn't apply here, which real-world scenarios triggered the exception, what legal exposure a wrong implementation would create, and what "wrong" even looked like from a regulatory standpoint.

The developer implemented it. The code was functionally similar to the POC. It passed review. It was wrong — not because anyone was careless, but because the knowledge that would have caught the error never entered the system. It lived in my head, got partially transmitted through a code file, and the rest evaporated.

Jira tickets are task trackers. They're optimized for *what* and *when*. They have no structure for *why*, no place for *when does this not apply*, no field for *here's what happens if you get this wrong*. Pouring domain knowledge into a task tracker is like trying to carry water in a net.

## The Fix: Let Context Live Next to Code

The structural answer is to give context a home — not in someone's head, not in a ticket, but in the repository itself, versioned alongside the code it explains.

We created a `docs/domain/` directory in our codebase. For every significant domain logic implementation, it contains:

- **Scenario coverage**: 3–5 real-world cases the feature must handle correctly, described in plain language
- **Exception taxonomy**: explicit rules for when standard logic doesn't apply and why
- **Failure modes**: what goes wrong legally, financially, or for the user if the implementation is incorrect
- **Regulatory anchors**: the specific statutes, rulings, or interpretive guidance that govern the behavior

Pull requests now include a link to the relevant domain document. Code review happens alongside domain review. The *why* lives in the same system as the *what*, under the same version control, visible to the same people.

This changes the domain expert's role in a meaningful way. They cannot write production code. But they can write this. In fact, they're the only ones who can write this well. Their contribution to the repository isn't code — it's the knowledge that makes code correct. That reframing matters both practically and for motivation: reviewing a PR becomes a professional act, not a favor.

## Why AI Doesn't Solve This — And Why It Completes It

There's a tempting shortcut here: use AI to bridge the gap. Have the domain expert explain verbally, let an LLM translate it into a development spec. Skip the documentation step.

This doesn't work, and the reason reveals something important.

LLMs are powerful at translation and synthesis. But they hallucinate at domain boundaries — exactly where your risk is highest. Without a structured, verified source of domain truth, an AI assistant will confidently fill gaps with plausible-sounding incorrect reasoning. It will give your developer an answer. The answer will be wrong in ways that are hard to detect.

The documentation isn't just for humans. It's the ground truth the AI needs to be useful.

When domain documents exist in the repository, the picture changes. A developer mid-implementation can query the domain documentation — directly or through an LLM that's been given the document as context. The AI becomes a 24-hour proxy for the domain expert, able to answer "does scenario X fall under the exception in section 3?" with actual grounding. The four human limits haven't disappeared, but they've been partially compensated by a system.

The structure enables the AI. The AI amplifies the structure. Neither works without the other.

## Context Belongs in the System, Not in Someone's Head

Organizations where domain knowledge lives only in specific people are fragile. Those people become bottlenecks when present and single points of failure when absent. The knowledge that took years to accumulate can leave in an afternoon.

Moving context from heads to repositories is not just a documentation practice. It's an architectural decision about where your organization's intelligence resides. It's the difference between a team whose quality depends on who shows up today and a team that improves incrementally because every hard-won insight gets captured and stays.

When a product is subtly wrong despite everyone's best efforts, the diagnostic question isn't "who failed?" It's "where did the knowledge need to go, and did we build a path for it to get there?"

Build the path. The people will follow it.

---

*Jason Jung is R&D Director at Zenterprise and a CPA. He writes about the intersection of AI, professional services, and capital markets.*
