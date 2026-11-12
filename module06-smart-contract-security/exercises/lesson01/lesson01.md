# Exercise 6.1 — Reentrancy

**Textbook:** [Reentrancy](../../lesson01-reentrancy.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Assume attacker credit is 1 ETH and the contract holds 3 ETH. Predict the effects of a single successful nested withdrawal when the outer function clears credit only after calling the recipient.

## Part B — Investigate and build

In a new local Forge project, create a deliberately vulnerable two-depositor vault and a one-callback test receiver. Demonstrate double payment in a test, then move effects before interaction. Keep the two versions isolated and assert the fixed version pays exactly one credit.

## Part C — Design reasoning

If a guarded withdrawal calls an untrusted receiver, which other public functions would you review for shared-state hazards?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
