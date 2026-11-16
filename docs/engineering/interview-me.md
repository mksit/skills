## What it does

`interview-me` interviews you about a plan or design until you and the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) share one understanding of it. It reads your repo's docs, glossary and ADRs before the first question, and it records the vocabulary and the hard decisions in your repo while it works.

The defining constraint is that it asks as little as it can. What the code and docs settle becomes a recorded default. Only the consequential, hard-to-reverse or user-only decisions reach you, at most three a round, each with options and a recommended answer. The interview ends with a list of every default it set, so you audit instead of answer. The interview itself is [interviewing](https://aihero.dev/skills-interviewing); this page covers what the repo grounding adds.

It is **[stateful](https://www.aihero.dev/ai-coding-dictionary/stateful)**. When a term resolves, the skill writes it to `GLOSSARY.md` at once. When a decision is hard to reverse, surprising without context and a real trade-off, it writes an ADR. With no repo in the working directory, it skips the docs and runs the plain interview.

## When to reach for it

You invoke this by typing `/interview-me`, and the agent won't reach for it on its own.

Reach for it at the start of a change when the plan is still fuzzy and the words for the thing are not settled yet. It is the single-session tool.

| What you have | Reach for |
| --- | --- |
| A repo, or no repo at all, and a plan you can settle in one session | `interview-me` |
| An effort too big to hold in one session | [wayfinder](https://aihero.dev/skills-wayfinder) |
| A repo with no domain docs, and no particular feature in mind | `interview-me`, aimed at the repo rather than a change |
| A decision blocked on knowledge in someone else's head | [to-questionnaire](https://aihero.dev/skills-to-questionnaire) |

The wayfinder split comes down to session count: `/interview-me` for single-session planning, `/wayfinder` for multi-session planning.

## Prerequisites

The skill writes into your repo, so you need to be somewhere it is safe to write. Resolved terms go to `.agents/docs/GLOSSARY.md`, or to the relevant context's `src/<context>/.agents/docs/GLOSSARY.md` when an `.agents/docs/GLOSSARY-MAP.md` marks a multi-context repo. Decisions go to `.agents/docs/adr/`. Both are created only when needed, so you set up nothing in advance.

It needs two other skills present, because its own `SKILL.md` delegates to them: [interviewing](https://aihero.dev/skills-interviewing) supplies the interview, and [domain-modeling](https://aihero.dev/skills-domain-modeling) supplies the writing.

## The paper trail

| What resolved | Where it lands |
| --- | --- |
| A term: the project's own word for a thing | `GLOSSARY.md`, inline, the moment it resolves |
| A decision that is hard to reverse, surprising without context, and a real trade-off | An ADR under `.agents/docs/adr/` |
| An agent default | The defaults list at the end of the interview, and nowhere else |
| Everything else you decided | The conversation, and nowhere else |

`GLOSSARY.md` is only a glossary: no implementation details, no [spec](https://www.aihero.dev/ai-coding-dictionary/spec), no scratch notes. An ADR needs all three conditions at once, so most sessions produce none. Hand the same conversation to [to-design](https://aihero.dev/skills-to-design) and then [to-spec](https://aihero.dev/skills-to-spec) rather than [clearing](https://www.aihero.dev/ai-coding-dictionary/clearing) it.

## Common questions

**Should I use this or `/wayfinder`?**
Scope decides it. Use this for anything you can settle in one session. Use [wayfinder](https://aihero.dev/skills-wayfinder) when the effort is too big to hold in one, and it charts the work as a map of decision [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket) first.

**It ran, but no `GLOSSARY.md` and no ADRs appeared.**
Either nothing qualified, because a change with no new vocabulary has nothing to write, or the agent did not load [domain-modeling](https://aihero.dev/skills-domain-modeling). Ask the agent which skills it loaded.

**It asked everything at once, with no recommendations.**
The agent did not load [interviewing](https://aihero.dev/skills-interviewing) and is improvising an interview. Ask which skills it loaded.

**Where did all my other decisions go?**
Into the conversation, and into the defaults list. Keep the session and feed it to [to-design](https://aihero.dev/skills-to-design) and [to-spec](https://aihero.dev/skills-to-spec), then re-read the spec against your own answers.

**Can I point it at an existing repo that has no docs at all?**
Yes. Invoke it and say "help me document my repo". It reads the code and asks you only about the words and decisions the code cannot settle.

**What should I do when the interview ends?**
The skill hands off to [to-design](https://aihero.dev/skills-to-design), which settles the shape, and then [to-spec](https://aihero.dev/skills-to-spec) in the same conversation.

## It's working if

- `GLOSSARY.md` changes *during* the session, term by term, and reads as pure vocabulary.
- Questions the codebase can answer get answered by reading the codebase, not asked of you.
- You see at most three questions a round, each with a recommended option.
- The interview closes with a defaults list you can audit in one read.
- You get few or no ADRs, and the ones you get are decisions you would be annoyed to argue again.

## Where it fits

`interview-me` is the head of the main build chain:

```txt
interview-me → to-design → to-spec → to-tickets → implement → code-review → retro
```

It produces the settled decisions and vocabulary that [to-design](https://aihero.dev/skills-to-design) and [to-spec](https://aihero.dev/skills-to-spec) then use without asking you again. Its close neighbours are [interviewing](https://aihero.dev/skills-interviewing), the interview primitive it runs, and [domain-modeling](https://aihero.dev/skills-domain-modeling), the glossary-and-ADR discipline it drives. Upstream, [wayfinder](https://aihero.dev/skills-wayfinder) charts efforts too large for one session. When you're unsure which skill or flow fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
