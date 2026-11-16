---
"mattpocock-skills": minor
---

`grilling`, `grill-me` and `grill-with-docs` are replaced by two skills. `interviewing` (model-invoked) is the procedure: it investigates the code and docs first, infers a default for every decision they settle, asks only about decisions that are architectural, critical, consequential, hard to reverse or the user's alone, and asks at most 3 questions a round, most consequential first. Each question offers 2-3 options with a one-line trade-off and a recommended one, worded so "yes" accepts it. It ends when every consequential decision is an answer or a recorded default, with a single list of the defaults it set and why. It still hands shape questions to `to-design`, and the advisor still checks before it closes. `interview-me` (user-invoked) is the entry point, and replaces both `grill-me` and `grill-with-docs`: it reads the repo's glossary, ADRs and docs before the first question, and runs `domain-modeling` as it goes.

Every caller moved: `to-design`, `triage`, `wayfinder`, `improve-codebase-architecture`, `ask-matt`, `fable-advisor` and `principle-work-autonomously` now name `interviewing`. The `wayfinder:grilling` ticket type is renamed `wayfinder:interview`: relabel tickets on existing maps. The docs pages for the three removed skills stay as archived pages.
