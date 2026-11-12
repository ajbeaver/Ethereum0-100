# Exercise 7.2 — ERC-721 ownership and callbacks

**Textbook:** [ERC-721 ownership and callbacks](../../lesson02-erc-721-ownership-and-callbacks.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Alice approves Bob for token #17 only and transfers #17 to Carol. Can Bob use the old approval? What if Alice also approved Bob as her operator?

## Part B — Investigate and build

Build a local ownership fixture for IDs 17 and 18 with explicit token approvals. Prove token-specific approval does not authorize movement of the other token and clears on transfer. In `receiver-notes.md`, describe how you would extend the test to check a rejecting receiver.

## Part C — Design reasoning

What callback can run when your contract uses a safe NFT transfer, and what state should already be consistent?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
