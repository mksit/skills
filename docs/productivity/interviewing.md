## What it does

`interviewing` is the interview loop that settles a plan, a decision, or an idea with you while it asks as little as it can. It maps the subject as a **decision tree**: every decision branches into the decisions that hang off it. It then works the tree in **rounds**.

The defining constraint is that it investigates before it asks. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reads the code, the docs, the glossary and the ADRs, and infers a **default** for every decision they can settle. It puts a question to you only when the decision is architectural, critical, consequential, hard to reverse, a preference only you hold, or cannot be settled from what is on disk. A trivial implementation detail is never a question, and no unrelated scope enters.

## When to reach for it

Type `/interviewing`, or the agent reaches for it on its own when a task fits. Usually a skill you typed runs it for you.

| What you have | Reach for |
| --- | --- |
| A plan or design, and a repo with docs to ground it | [interview-me](https://aihero.dev/skills-interview-me): the same interview, reading the docs first and recording terms and ADRs as it goes |
| An effort too big to hold in one session | [wayfinder](https://aihero.dev/skills-wayfinder): it charts a map and runs the interview inside the decision tickets |
| A question that talking cannot settle: how something should look or feel | [prototype](https://aihero.dev/skills-prototype): build the throwaway version, then come back |
| A skill of your own that needs an interview | Invoke `/interviewing` from it, rather than writing another one |

## The round, the frontier, and the defaults list

The **frontier** is every open decision whose prerequisites are settled and that passes the ask-or-default test. A **round** asks at most three of them, most consequential first. More than three on the frontier means the rest wait for the next round, or become a default when a wrong default stays reversible.

Each question offers two or three concrete options, a one-line trade-off for each, and one recommended option. It is worded so "yes" accepts the recommendation, and it says the recommendation applies unless you object. Questions arrive in plain reply text, never through a harness's question tool.

Facts are the agent's job and decisions are yours. When a frontier question needs something the [environment](https://www.aihero.dev/ai-coding-dictionary/environment) can settle, the agent looks it up, with a [sub-agent](https://www.aihero.dev/ai-coding-dictionary/subagent) when the search is wide.

The interview ends as soon as implementation can proceed responsibly: every consequential or hard-to-reverse decision is your answer or a recorded default, and every remaining unknown stays reversible during implementation. It closes with one **defaults list**, each default with a one-line reason, so you can audit what the agent settled without answering anything.

Shape questions (a module, an interface, a data shape, a seam) are never asked here. The interview writes each down and hands them, with the settled decisions and defaults, to [to-design](https://aihero.dev/skills-to-design).

## Common questions

**Can I go back to one question at a time?**
Add a line to your global `CLAUDE.md`, such as `When interviewing, ask one question at a time.` Users who read slowly, or who work in a second language, report that it helps.

**Why only three questions a round?**
A round that asks everything buries the one decision that matters. The cap is per round: the total number of questions is not capped, and a hard problem still takes several rounds. A very long interview usually means the scope was too big; break the work up and interview the pieces.

**It settled something I wanted to decide.**
Read the defaults list at the end and object to the line. A default is a recorded decision, so changing it costs one reply. If a decision of that kind keeps slipping through, say so in your own `AGENTS.md` or `CLAUDE.md`.

**It answered its own questions instead of asking me.**
That is a bug in the run. Preferences only you hold, and consequential or hard-to-reverse decisions, are yours. It happens most when another skill runs `interviewing` to resolve a ticket and the agent takes that as permission to keep moving.

**I installed `interview-me` and nothing happens.**
`interview-me` is a short skill that calls this one, so install both. It also calls [domain-modeling](https://aihero.dev/skills-domain-modeling) when a repo is present.

## It's working if

- You see few questions, each with options, a recommendation and a "yes" that accepts it.
- Nothing it asks could have been found by reading the repo.
- A round holds at most three questions, and the most consequential comes first.
- It ends with a defaults list, and each default has a reason you can check.
- It stops when the rest is reversible, instead of probing every branch.
- It writes shape questions down and hands them to `to-design`, instead of answering them.

## Where it fits

`interviewing` is a **primitive**, not a step you schedule: the single source of truth for the interview technique, so every skill that needs one reaches for it instead of inventing one. [interview-me](https://aihero.dev/skills-interview-me) is its user-invoked front door and the head of the main build chain, ahead of [to-design](https://aihero.dev/skills-to-design). [wayfinder](https://aihero.dev/skills-wayfinder) runs it to resolve decision tickets, [triage](https://aihero.dev/skills-triage) to interview a vague report into a workable one, and [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) to walk the tree once you pick a candidate. When you are unsure which entry point fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
