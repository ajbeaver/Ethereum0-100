# Exercise 7.1 — ERC-20 balances and allowances

**Textbook:** [ERC-20 balances and allowances](../../lesson01-erc-20-balances-and-allowances.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Alice approves Bob for 40, Bob spends 15 sending to Carol, and then Bob tries to spend 30. Predict balances and remaining allowance for a conventional ERC-20 with sufficient Alice balance.

## Part B — Investigate and build

Create a local Forge test for a minimal allowance ledger with Alice, Bob, and Carol. Implement balance query, finite approval, and delegated transfer for the exercise. Prove the spender and destination can differ and that an excessive spend leaves balances and allowance unchanged. Save in `test/AllowanceLedger.t.sol`. Document which ERC-20 requirements your teaching fixture omits.

## Part C — Design reasoning

What could go wrong if a contract credits a deposit when its token transfer returned false?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
