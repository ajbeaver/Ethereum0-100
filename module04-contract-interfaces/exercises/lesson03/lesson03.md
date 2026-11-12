# Exercise 4.3 — State machines

**Textbook:** [State machines](../../lesson03-state-machines.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

For deadline 100, fill a table for release and refund at timestamps 99, 100, and 101. Do the same for a non-payer and a deposit already released.

## Part B — Investigate and build

Build `src/TimedRecord.sol` with the textbook states and role guards, but keep it nonpayable and use a stored numeric amount instead of crediting ETH. Expose open, release, refund, and lookup. Demonstrate legal and illegal transitions, including the exact boundary. Use `cast rpc --rpc-url "$RPC_URL" evm_setNextBlockTimestamp <decimal-timestamp>` on Anvil before the relevant transaction and confirm the timestamp in its block.

## Part C — Design reasoning

Why would using `<=` for release and `>=` for refund permit two outcomes at the same deadline?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson03-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
