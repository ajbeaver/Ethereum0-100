# Exercise 5.3 — Fuzzing and invariants

**Textbook:** [Fuzzing and invariants](../../lesson03-fuzzing-and-invariants.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A test only checks contract balance after release. Can it detect that the wrong party was credited? Propose an additional property and identify the actors.

## Part B — Investigate and build

Add a fuzz test to LocalCredits that deposits two bounded positive amounts from distinct actors. Withdraw one actor’s full credit and assert their credit is zero, the other credit is unchanged, and the remaining balance covers it. Add deterministic tests at the chosen bounds. Save in `test/LocalCreditsFuzz.t.sol`.

## Part C — Design reasoning

How could a fuzz test pass without exercising any successful withdrawal? State how you would detect this.

## Submission

Keep predictions, commands, observed results, and design answers in `lesson03-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
