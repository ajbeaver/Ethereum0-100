# Exercise 3.3 — Callers and errors

**Textbook:** [Callers and errors](../../lesson03-callers-and-errors.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Predict `msg.sender` seen by a contract called by Bob directly and then by a relay contract that Bob calls. Predict what a nonpayable function does when called with positive value.

## Part B — Investigate and build

Build `src/GatedMeter.sol`. Set the operator through the constructor, allow only that operator to overwrite a reading, and revert with a custom error for other callers. On Anvil, demonstrate the authorized update and an unauthorized simulation. Show the reading after both.

## Part C — Design reasoning

Which authority would a contract accidentally grant if it set its operator to `msg.sender` during factory deployment? How would you test the desired behavior?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson03-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
