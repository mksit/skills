---
name: principle-no-over-defending
description: "Apply when you plan checks for a change, when you brief or monitor subagents or long-running jobs, and when work runs against a deadline. Spend verification only where it can change the outcome, and keep every agent and machine busy."
---

# No Over-Defending

The user's time is the scarcest resource. A check earns its place only when its result can change what ships. Each hour spent defending a result that is already safe is an hour of delay.

## Decisions

- **Decide; never block.** Take the recommended option, record it with a one-line reason where the work is tracked, and continue (see "principle-work-autonomously").
- **Subagents decide too.** Every brief says: "take your recommended option, record why, and build it." A subagent returns options only when every option breaks the requirements.
- **Test a hypothesis only when its answer changes the decision.** If every outcome leads to the same action, take the action.

## Verification

- **Run only the checks the change can move** (see "principle-check-the-blast-radius"). Start them all at once, never one after another.
- **Size timeouts to the measured runtime**, not to a worst case.
- **Run a slow check once, at the final state.** A slow check that passed at a state the later changes cannot affect is not rerun; say so with the result.
- **Verify each fix on the one case it targets.** Run the full set of expensive end-to-end cases once, at the final state.
- **One review pass.** Fix the correctness items it raises; list the rest for later.

## Monitoring

At every notification or check-in, look at two things:

1. **Each agent's last activity.** An agent with no live background work, and with no activity for longer than its job should take, has stalled. Resume it now and give it the exact next step.
2. **Each machine's free capacity.** Free capacity gets the next job that does not wait on a pending result.

## Deadlines

- **Set a freeze time** as soon as the user names a deadline, and record it where the work is tracked.
- **Work that is not finished and verified at the freeze** is reported as open, with its diagnosis. It does not hold back the rest.
- **Wake yourself at the freeze** with a background wait. Then integrate what is frozen and start the final runs.

**Done when** every check you ran could have changed the outcome, no agent stayed stalled and no machine stayed idle while work was ready, and the result arrived by the freeze plan.
