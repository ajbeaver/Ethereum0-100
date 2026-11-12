# Exercise 1.2 — Transactions and gas

**Textbook:** [Transactions and gas](../../lesson02-transactions-and-gas.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A plain transfer sends 3,000 wei using 21,000 gas at an effective price of 2 gwei. Predict both balance deltas. Then predict what differs for an included failed contract call with 3,000 wei attached, assuming the same gas used and price.

## Part B — Investigate and build

Send 0.002 ETH from Alice to Bob on a fresh local node. Create `transfer.md` with before and after balances, the transaction hash, and the receipt’s status, gas used, and effective gas price. Recalculate both balance changes using integer wei and compare them to the observations.

## Part C — Design reasoning

A user retries a slow deposit after seeing a transaction hash. Explain how you would determine whether the first request was mined and whether a new transaction could create a second deposit.

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
