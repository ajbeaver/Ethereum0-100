# Exercise 4.2 — Payable calls

**Textbook:** [Payable calls](../../lesson02-payable-calls.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A payout function clears a credit, then its low-level call returns false. Predict final storage when the function reverts and when it ignores false.

## Part B — Investigate and build

Implement `src/LocalCredits.sol` with positive payable deposits, a per-caller credit, and withdrawal of the full credit to a supplied payable address. Clear credit before calling the recipient and revert on failure. Add a `RejectEther` fixture with a reverting receive function. On Anvil show an ordinary payout, a rejected payout, and recovery using another recipient.

## Part C — Design reasoning

Why should a failed ETH transfer preserve the user’s ability to claim it later?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
