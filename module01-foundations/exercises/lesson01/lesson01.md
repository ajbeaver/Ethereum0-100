# Exercise 1.1 — State and accounts

**Textbook:** [State and accounts](../../lesson01-state-and-accounts.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A fresh Anvil node returns `0x` from `cast code` for both Alice and an unused address. Predict which other observations could distinguish them. Explain whether an observer can derive a signing key from either result.

## Part B — Investigate and build

Create `accounts.tsv` with columns for chain ID, block number, address, balance in wei, and code. Fill three rows for Alice, Bob, and the unused address in the textbook. Use one block number for all reads via `--block` on `cast balance` and `cast code`. Explain why the unused balance can be represented as zero.

## Part C — Design reasoning

If two Anvil instances use the same deterministic accounts but different chain IDs, what information should a deployment record include so another person can find the correct contract?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
