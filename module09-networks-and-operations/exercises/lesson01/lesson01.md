# Exercise 9.1 — Network and deployment identity

**Textbook:** [Network and deployment identity](../../lesson01-network-and-deployment-identity.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Two nodes share the same chain ID and first account address. The Counter address is present on one and missing on the other. Which manifest checks would catch the difference?

## Part B — Investigate and build

Build `check_deployment.py` to read a manifest for your locally deployed Counter and validate chain ID, creation receipt, block hash, and current code. Use Cast for hashing or invoke it from Python. Run against the expected node, then a second loopback node with a different chain ID. Demonstrate that neither mismatch path sends a transaction. Repeat after a restart with the original chain ID and no redeployment.

## Part C — Design reasoning

Why should a command that prepares a transaction verify chain and deployment before displaying a success-ready prompt?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
