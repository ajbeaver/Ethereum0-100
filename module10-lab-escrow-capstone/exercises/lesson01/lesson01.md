# Exercise 10.1 — Escrow contract specification

**Textbook:** [Escrow contract specification](../../lesson01-escrow-contract-specification.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Alice opens 9 wei for Bob, releases it, and Bob attempts withdrawal to a rejecting recipient. Trace the status, active principal, unpaid total, Bob’s credit, and contract balance after each step.

## Part B — Investigate and build

Write the contract from the interface and event requirements. Add tests for a successful release and refund, boundary timestamps, invalid open inputs, a wrong caller, and repeated settlement. Save the interface and authority matrix in `design.md`.

## Part C — Design reasoning

Which effects must be complete before the recipient receives control? Why does event ordering matter to the indexer?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
