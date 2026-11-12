# Exercise 5.2 — Traces and revert data

**Textbook:** [Traces and revert data](../../lesson02-traces-and-revert-data.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A recipient reverts and the paying contract then reverts with `PayoutFailed()`. Which error does the outer caller see? Where would you look for the recipient’s error?

## Part B — Investigate and build

In your LocalCredits project, add a test that deposits, attempts a withdrawal to RejectEther, and then withdraws successfully elsewhere. Assert the credit after each call. Save a short annotated call tree as `trace.md`.

## Part C — Design reasoning

Why is the outer error useful to a user while the inner failure is useful to a developer?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
