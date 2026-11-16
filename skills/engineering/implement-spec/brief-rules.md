# Brief rules

How `implement-spec` writes each subagent brief, and the rules every brief binds. The role agent's definition carries its process (tdd, merge before report, report shape) and the repo's instructions carry the execution rules; a brief links them and repeats neither.

## Writing the brief

A brief hands one subagent one **work item**: one ticket, one merge, or one review fix. It carries one acceptance condition and one deciding check. A brief that carries more is cut before it is sent, and the rest goes to a new subagent. One subagent runs at most one long job at a time.

Fill one line a field, in this order; leave out a field the work item does not take:

```text
Ticket: <tracker link>; tier <tier>
Rules: <absolute path of this file>; repo brief: <link>
Scope: <worktree>, <branch>; owned files; files not to touch
State: <integration tip>; earlier branch or decisions to reuse; blockers and their branches
Hypothesis: <expected value and why> — confidence <Low|Medium|High>; refuted if <value>
Jobs: <each job, its estimate, and the shared resource it takes>
Expected finish: <1.5 × the estimate>
Progress line: <what each job prints, every <n> <units>>
Acceptance: <the stopping condition>
Return: <what comes back>
```

- **Lean**: at most 40 lines. The ticket holds the design, the repo's instructions hold the execution rules, this file holds the standing rules; the brief carries only what is specific to its work item.
- **Hypothesis** goes in a brief whose work item is a measurement or a probe: the expected value, a confidence word, and the value that refutes it.
- **Estimate** each job from a recorded time of the same input, or of the nearest recorded one. `Expected finish` is 1.5× the estimate; a job still running at 2× it is **overdue** (see Waiting).
- **Budget**: a brief whose jobs take a shared, finite resource (GPUs, a device, a license, a node) names how much each job takes. The dispatcher sends a brief only when the sum over every live brief stays within that resource's capacity; the rest wait in the frontier, never in a job queue.
- **Progress line** goes in every brief that runs jobs: the counter each job prints at a fixed cadence, so a stall shows as a stopped counter. A run past about 15 minutes carries every counter its measurement needs before it starts, so it never repeats for a missing number.

## Measuring

- **One deciding check** a work item: the smallest case that shows the change wrong; for a fix, the check that fails on the base and passes on the fix. Every further check names the decision it changes.
- **The full suite runs once**, on the green candidate: the integration head before the final gate.
- **Smallest input first**: a large input runs only after the same path ran end to end on a small one. The long run confirms; it never discovers.
- **A number taken is cited, never re-measured**, unless evidence shows it wrong. A null is recorded and the work moves on.
- **Over-measured**: an item whose measuring took longer than its building. Say so in its record and cut the next brief.

## Rules the brief binds

- **Ownership.** The subagent owns every step of its work item: builds, tests, long runs, runs on remote machines, measurements, reports. A step that needs hardware or hours of wall time is still its step. A simple obstacle (a build error, a failed submission, a wrong argument) is fixed in the same round; a design question comes back as the facts and the options.
- **Scope.** The subagent works in its own worktree, commits by explicit paths, and leaves other subagents' changes as they are. A closed or abandoned ticket is history: read its comments and branch for earlier decisions and code, and reuse what meets the acceptance condition.
- **Early start.** Against an unmerged blocker, the subagent reads the blocker's branch, builds every part that does not need it merged, then commits and reports `waiting on #<blocker>` with what remains.
- **Waiting.** One **terminal watcher** per job: a background loop that polls the job with a long sleep, stays silent, and exits at a terminal state or when the job is overdue. It wakes the subagent once, because every end of its turn costs the dispatcher a turn. An overdue job is the subagent's to explain: it reads what the job is doing, stops a job that waits on a resource instead of working, and reports `overdue` with the cause. The subagent ends its turn while a job runs only when that job's watcher is alive, and stops only the processes and jobs it started.
- **Messages.** At most two before the report: one when its reading of the brief departs from the design, one when a long job is submitted (its id and expected finish).
- **Records.** The numbers go in a comment on the ticket. Temporary scripts and drafts go in the scratchpad the brief names.
- **Return.** The delta only: the commits, the job ids, each acceptance criterion with its decisive output, the link to the ticket comment that holds the numbers, and the decisions the dispatcher must make, one line each.
