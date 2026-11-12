# Exercise 6.2 — Authorization and griefing

**Textbook:** [Authorization and griefing](../../lesson02-authorization-and-griefing.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Alice calls a relay, which invokes a target. Predict `msg.sender` and `tx.origin` in the target. Which check would a relay exploit if the owner is Alice?

## Part B — Investigate and build

Create an authorization matrix for your TimedRecord exercise. Write `test/TimedRecordAuth.t.sol` with a payer, outsider, and relay. Demonstrate the relay cannot perform a payer-only action when Alice merely calls the relay. Test both terminal paths.

## Part C — Design reasoning

Why should a withdrawal accept a recipient chosen by the credit owner? Which ownership check makes this safe?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
