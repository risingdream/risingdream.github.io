---
title: "What Agile Assumes But Never Says: The Contact Point Between Loops"
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

Something feels off. Someone floats an idea that sounds familiar. "Didn't we look at that last sprint and shelve it?" someone asks. But nobody can quite remember why it was shelved. The owner digs through Jira, opens a Notion page, scrolls back through Slack. It was a decision made three weeks ago — and the context has vanished.

They iterated. But nothing compounded. Why?

Agile promises that repetition makes teams wiser. When that promise fails in practice, the problem usually isn't the methodology. It's that a certain precondition for iteration isn't being met.

---

## What Divergence Builds

Think about what actually happens during divergence — a brainstorm, an idea session, sprint planning. People aren't just throwing out options.

*"What about this direction?"* — *"I'm not sure our customer segment is ready for that. The interview last week pointed that way too."* — *"What about this angle instead?"* — *"That's technically out of reach this quarter — there's a latency issue with the current infrastructure."* — *"If the infrastructure issue gets resolved, could we revisit it?"* — *"Not before Q3."*

In this exchange, people aren't just trading information. They're building **Common Ground** — a term introduced by Clark & Brennan (1991). It's the accumulated mutual confirmation that *we are looking at the same thing*: why this idea survived, why that direction was dropped, what constraint shaped that judgment.

The critical point is this: the fact that "infrastructure won't support it before Q3" can be written down. But the way that fact wove together with other constraints to produce an implicit consensus — *let's hold off on this whole direction for now* — that part doesn't get recorded.

Then convergence happens.

---

## What Convergence Erases

Convergence compresses that rich context into a conclusion. This sprint, we're doing A. It goes into the backlog, tickets are made, the decision is recorded.

But what gets recorded is the **conclusion**. The context that produced it — the alternatives that were considered and discarded, the implicit constraints that guided the convergence, the shared assumptions the team held at that moment — goes unrecorded.

Consider a concrete example. A design session concludes: "We'll use custom authentication instead of OAuth." The ticket says "implement custom auth." But the reasoning — avoiding dependency on a specific OAuth provider, a concern raised by the security team, a tacit agreement to revisit in six months — is nowhere to be found. Six months later, a new developer joins, doesn't understand the decision, and the team restarts the same discussion from scratch.

Nonaka (1995) frames this as the fundamental problem of knowledge transformation. What forms between people during divergence is **tacit knowledge**: difficult to fully articulate, embedded in experience and shared context. When convergence translates that into **explicit knowledge**, compression is inevitable. Explicit knowledge is transmissible. But it can't carry the full richness of what it replaced.

This isn't a bug. It's the nature of convergence. The problem comes next.

---

## The Width of the Contact Point

If we visualize the diverge-converge cycle as two diamonds, the first diamond closes at convergence and the second opens at the next divergence. Where they meet is the **contact point**.

The critical question: **how wide is that contact point?**

A narrow contact point means only the conclusion — the output of convergence — carries into the next divergence. A wide contact point means the conclusion *plus* the context that produced it: the alternatives considered, the reasoning behind the choice, the constraints that were active at that moment.

The wider the contact point, the more genuinely the next divergence builds on what came before. Teams explore directions they haven't tried yet, rather than re-examining territory they've already covered. The narrower it is, the more teams spend energy reconstructing context from scratch — or worse, re-explore the same ground without realizing it. Progress looks real. But the team is circling.

Star & Griesemer (1989) called the tools that widen this contact point **Boundary Objects**: artifacts that people with different viewpoints can all reference — prototypes, shared documents, common vocabulary, decision logs. These become the connective tissue between loops. The richer these artifacts, the wider the contact point.

But here another problem surfaces. Widening the contact point itself is hard.

---

## The Cognitive Ceiling

Compressing the rich context of divergence into something transmissible — organized enough to survive the loop boundary and useful enough to matter in the next one — is cognitively demanding work. Not everyone does it equally well.

Herbert Simon (1955) showed that human rationality is **bounded**. We want to make optimal decisions, but cognitive resources are finite. There's a ceiling on how much complexity we can process, and we inevitably stop at "good enough" — what Simon called *satisficing*.

Sweller's Cognitive Load Theory (1988) makes this concrete: working memory capacity is fixed. When the context from divergence is complex, important pieces get pushed beyond the boundary of working memory. That's not a failure of will. It's a structural constraint.

High-capability individuals can compress more context into the handoff. They chunk information more densely, select what matters with better judgment, and reconstruct it at the next loop. The breadth of divergence, the precision of convergence, the density of what crosses the contact point — all of this scales with cognitive capacity.

Most teams have someone who naturally fills this role. The person who, after a long meeting, says: "Let me summarize what we actually decided and why." The person who, at the next sprint planning, says: "We looked at this before — here's why we set it aside." That person is, in effect, acting as the contact point.

This is an uncomfortable truth. Not everyone can widen the contact point equally. And this is exactly where the problem becomes one of organizational design.

---

## An Organizational Design Problem

So is the answer to hire smarter people?

Partly, yes. But it's not sufficient — for two reasons.

First, not every team can secure that kind of person. Second, when that capability concentrates in a single individual, the organization creates a single point of failure.

Hutchins (1995) offers a different frame with **Distributed Cognition**: cognition doesn't live only in individual minds. Tools, artifacts, team structures, environments — all of these become part of the cognitive system. Hutchins studied naval navigation and found that calculations impossible for a single person became achievable through a distributed system of people and instruments. The system compensates for individual cognitive limits.

The organizational design question, then, is this: does the contact point depend on individual capability, or is it built into the system?

When a high-capability person carries loop context in their head, their departure collapses the contact point. This is Wegner's **Transactive Memory** (1985) failing: the team's distributed knowledge of *who knows what* becomes dangerously centralized. When that person leaves, the organizational memory goes with them. Every startup that has lost a key developer or PM and found that "nobody knows why it was designed that way" has experienced this.

An organization that designs the contact point systemically looks different. Architecture Decision Records don't just log what was chosen — they log *why*: "We selected Y over X. The reason was Z. The alternatives we considered and discarded were A and B. The constraints active at the time were C." That's not a conclusion. It's a record of reasoning. Structured retrospectives shift the question from "what did we complete this sprint" to "what did we learn, and what should change." These tools preserve context in organizational memory, not personal memory. They absorb individual variance.

---

## Three Conditions for Iteration That Compounds

Agile's promise — that iteration produces learning — holds only when three things are true.

**First, divergence must produce context, not just options.** The quality of a divergence session isn't measured by how many ideas surfaced. It's measured by how much shared context was built — how richly the team now understands why certain directions are worth pursuing and others aren't.

**Second, convergence must externalize context, not just record conclusions.** The goal isn't perfect documentation. It's enough that when the next divergence begins and someone asks "why did we go this direction?", there's somewhere to look for an answer.

**Third, the contact point must be maintained by systems, not individuals.** Cognitive limits are real. People leave. A norm of "we should document things" isn't enough. The structure needs to make documentation the natural path, not the extra effort.

---

## Closing

Speed only matters when you have direction. Direction comes from context. Context lives at the contact point between loops.

A two-week sprint cycle means 26 contact points in a year. At each one, is enough context actually making it through?

Without an answer to that question, fast iteration becomes fast circling. The laps pile up. The ground covered doesn't.

Agile's real assumption isn't fast iteration. It's **iteration that accumulates**. And that structure doesn't emerge on its own.

It has to be designed.
