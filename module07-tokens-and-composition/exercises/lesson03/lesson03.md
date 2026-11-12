# Exercise 7.3 — External dependencies

**Textbook:** [External dependencies](../../lesson03-external-dependencies.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A token reports success on a requested deposit of 100, while the vault’s observed balance rises by 97. Predict the liability if credited from the request. What safety property may fail?

## Part B — Investigate and build

Create a local fixture that simulates a three-unit transfer fee on a 100-unit deposit. Test one accounting rule that rejects the deposit when the delta differs and one that credits the observed delta. Describe the limitations of the second rule in `asset-policy.md`.

## Part C — Design reasoning

Which assumption is stronger: calling a known interface, or verifying that a specific dependency exhibits the behavior your accounting needs? Give a concrete example.

## Submission

Keep predictions, commands, observed results, and design answers in `lesson03-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
