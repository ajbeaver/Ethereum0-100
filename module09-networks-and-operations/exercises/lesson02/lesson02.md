# Exercise 9.2 — Layer 2 and bridge semantics

**Textbook:** [Layer 2 and bridge semantics](../../lesson02-layer-2-and-bridge-semantics.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Given a successful L2 withdrawal initiation receipt and no destination claim evidence, what can a UI state about each chain? Which label should remain pending?

## Part B — Investigate and build

Write `bridge-state.md` from this synthetic evidence: source receipt success at block 50, message registered at L1 block 180, claim eligible at block 210 under the fixture rules, and no claim receipt. Create a timeline and design states for each observation. Add a branch where the source receipt becomes noncanonical before registration.

## Part C — Design reasoning

Why should a single-chain escrow avoid modeling a bridge withdrawal as a normal ETH transfer?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
