# Exercise 5.1 — Unit tests

**Textbook:** [Unit tests](../../lesson01-unit-tests.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Two tests each increment the Counter once after setup. Predict each final count. Predict what can happen if a getter consumes a prank intended for the next state-changing call.

## Part B — Investigate and build

Add tests to your GatedMeter project. Cover an authorized update and an unauthorized call from a distinct address. Assert the exact custom error data and unchanged reading in the rejection case. Save under `test/GatedMeter.t.sol`.

## Part C — Design reasoning

Which two assertions would catch a function that rejects the wrong caller for the wrong reason?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
