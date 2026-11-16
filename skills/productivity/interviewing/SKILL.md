---
name: interviewing
description: Interview the user about a plan, decision, or idea, asking only what the code and docs cannot settle. Use when the user wants to stress-test their thinking, or a skill needs decisions settled with them.
---

Reach shared understanding with the least user intervention. Map the subject as a **decision tree**: every decision branches into the decisions that hang off it. Settle each decision yourself where you responsibly can, and put only the **consequential** ones to the user.

## Investigate first

Finding _facts_ is your job, never the user's. Before any question, read what the repo holds on the subject: the code it touches, the docs, the glossary and the ADRs. When a fact needs a wider search, dispatch a sub-agent with a fresh context window. A running exploration is an unsettled prerequisite: only the questions downstream of it wait, so go on with the rest.

Infer a reasonable **default** for every decision the context can settle: the codebase's own convention, an ADR, the glossary's term, the nearest precedent. Record each default as you set it.

## Ask or default

Ask the user only when the decision is architectural, critical, consequential, hard to reverse, a preference only the user holds, or unresolvable from available context. Everything else takes a default. A trivial implementation detail is never a question, and a decision outside the subject's scope is never introduced.

Interviewing settles **what** and **why**, never **shape**. Never ask or answer a question about a module, an interface, a data shape or a seam: write it down as a **shape question** for `to-design`, and go on with the rest of the tree.

## Rounds

The **frontier** is every open decision whose prerequisites are settled and that passes the ask-or-default test. Ask the frontier in one round, **at most 3 questions, most consequential first**. A frontier of more than 3 keeps the remainder for the next round, or turns it into a default where a wrong default stays reversible. A question whose answer depends on another open question belongs to a later round.

Put the round in plain reply text, and let the user answer by replying to it. A harness's question tool stays unused: a round reads as one message, recommendations included.

Each question offers 2-3 concrete options, a one-line trade-off for each, and one recommended option. Word it so "yes" accepts the recommendation, and say that the recommendation applies unless the user objects:

```
❓ **Q1** - **<question title>**: <the decision and why it matters, briefly>

- **A (recommended)**: <one-line trade-off>
- **B**: <one-line trade-off>
- **C**: <one-line trade-off>

➡️ Reply "yes" to accept A. A applies unless you object.
```

When a wrong answer would be costly to undo (a migration, a data or history change), get the recommended answer from the **advisor subagent** (the `fable-advisor` agent) before you ask.

Each round the user answers reshapes the tree: settled decisions unblock the decisions that hung off them. Recompute the frontier and ask the next round.

## Done

Stop as soon as implementation can proceed responsibly. The interview is **done** when every consequential or hard-to-reverse decision is the user's answer or a recorded default, and every remaining unknown stays reversible during implementation.

Before you call it done, dispatch the advisor with every settled decision, every default, and the code they rest on. Put each gap or contradiction it names back on the frontier.

Then close with one **defaults list**: every default you set yourself, once, each with a one-line reason, so the user can audit without answering. The user's silence on it accepts the defaults.

Call the Skill tool with "to-design". Pass it the settled decisions, the defaults and every shape question. Interviewing always hands off to `to-design`; it never designs and never goes straight to a spec or to code.
