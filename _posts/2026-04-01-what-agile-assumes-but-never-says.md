---
title: "What Agile Assumes But Never Says: Context Loss Between Loops"
date: 2026-04-01
categories:
  - Tech & AI
tags:
  - agile
  - product-development
  - organizational-design
  - knowledge-management
excerpt: "Agile promises that iteration compounds. But iteration only compounds when context survives the loop boundary. Most teams treat context loss as normal. It isn't — it's a design failure."
---

Sprint planning is over. The team debated for two hours, eventually converging on a direction for the week. Tickets get created, tasks get assigned.

Two weeks later: another planning session. The team diverges again.

Something feels off. Someone floats an idea that sounds familiar. "Didn't we look at that last sprint and shelve it?" someone asks. But nobody can quite remember why it was shelved.

They iterated. But nothing compounded. Why?

---

## What Divergence Builds

Think about what actually happens during divergence — a brainstorm, an idea session, sprint planning. People aren't just throwing out options.

*"What about this direction?"* — *"I'm not sure our customer segment is ready for that."* — *"Right, the interview last week pointed that way too."* — *"What about this angle instead?"* — *"That's technically out of reach this quarter."*

In this exchange, people are building **Common Ground** — a term introduced by Clark & Brennan (1991). It's not just information transfer. It's the accumulated mutual confirmation that *we are looking at the same thing*. Why this idea survived. Why that direction was dropped. What constraint shaped that judgment. All of it becomes implicitly shared among the people in that room.

Then convergence happens.

---

## What Convergence Erases

Convergence compresses that rich context into a conclusion. This sprint, we're doing A. It goes into the backlog, tickets are made, the decision is recorded.

But what gets recorded is the **conclusion**. The context that produced it — the alternatives that were considered and discarded, the implicit constraints that guided the convergence, the shared assumptions the team held at that moment — goes unrecorded.

Nonaka (1995) frames this as the fundamental problem of knowledge transformation. What forms between people during divergence is **tacit knowledge**: difficult to fully articulate, embedded in experience and shared context. When convergence translates that into **explicit knowledge**, compression is inevitable. Explicit knowledge is transmissible. But it can't carry the full richness of what it replaced.

This isn't a bug. It's the nature of convergence. The problem comes next.

---

## The Width of the Contact Point

If we visualize divergence-convergence as two diamonds, the critical question is this:

**How wide is the contact point where they meet?**

A narrow contact point means only the conclusion — the output of convergence — carries forward into the next divergence. A wide contact point means the conclusion *plus* the context that produced it: the alternatives considered, the reasoning behind the choice, the constraints that were active at that moment.

The wider the contact point, the more the next divergence genuinely builds on what came before. The narrower it is, the more the team spends energy reconstructing context from scratch — or worse, re-explores the same ground without realizing it.

Star & Griesemer (1989) called the tools that widen this contact point **Boundary Objects**: artifacts that people with different viewpoints can all reference — prototypes, shared documents, common vocabulary, decision logs. These become the connective tissue between loops.

---

## The Cognitive Ceiling

Here's the uncomfortable reality: widening the contact point is hard.

Compressing the rich context of divergence into something transmissible — organized enough to survive the loop boundary — is cognitively demanding work. Not everyone does it equally well.

Herbert Simon (1955) showed that human rationality is **bounded**. We want to make optimal decisions, but cognitive resources are finite. There's an upper limit to how much complexity we can process. Sweller's Cognitive Load Theory (1988) makes this concrete: working memory capacity is fixed, and the more complex the context, the less can be carried across at once.

High-capability individuals can compress more context into the handoff. The breadth of their divergence, the precision of their convergence, the density of what they pass across the loop boundary — all of this scales with cognitive capacity. Organizations have a distribution of this capability, and that distribution shapes the quality of every loop.

This is an uncomfortable truth. Not everyone can widen the contact point equally.

---

## An Organizational Design Problem

So is the answer to hire smarter people?

Partly, yes. But it's not sufficient. Hutchins (1995) offers a different frame with **Distributed Cognition**: cognition doesn't live only in individual minds. Tools, artifacts, team structures, environments — all of these become part of the cognitive system. The system can compensate for individual cognitive limits.

An organization that leaves contact-point widening to individual capability gets a different outcome than one that designs the contact point into its systems.

When a high-capability person carries loop context in their head, their departure collapses the contact point. Wegner's **Transactive Memory** (1985) — the distributed knowledge a team builds of *who knows what* — becomes dangerously centralized. When that person leaves, the organizational memory leaves with them.

An organization that designs the contact point systemically looks different. Architecture Decision Records, decision logs, structured retrospectives — these preserve context in organizational memory, not personal memory. They absorb individual variance.

The difference between these two organizations isn't just about documentation hygiene. It's about whether iteration actually compounds.

---

## Three Conditions for Iteration That Compounds

Agile's promise — that iteration produces learning — holds only when three things are true.

**First, divergence must produce context, not just options.** The conversation needs to capture not only *what* was explored, but *why* certain directions were kept and others weren't.

**Second, convergence must externalize context, not just record conclusions.** The contact point between loops must carry more than the outcome. The reasoning, the discarded alternatives, the active constraints — these need to survive in accessible form.

**Third, the contact point must be maintained by systems, not individuals.** Cognitive limits are real. People leave. The container for context needs to exist inside the organization, not inside a person.

---

## Closing

Speed only matters when you have direction. Direction comes from context. Context lives at the contact point between loops.

Agile's real assumption isn't fast iteration. It's **iteration that accumulates**. And that structure doesn't emerge on its own.

It has to be designed.
