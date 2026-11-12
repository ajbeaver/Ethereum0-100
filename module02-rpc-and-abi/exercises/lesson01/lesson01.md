# Exercise 2.1 — JSON-RPC requests

**Textbook:** [JSON-RPC requests](../../lesson01-json-rpc-requests.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Classify these three outcomes: connection refused, HTTP 200 with a JSON-RPC `error`, and HTTP 200 with `result: null` for a receipt.

## Part B — Investigate and build

Create `rpc-transcript.md` with raw curl requests and responses for `eth_chainId`, `eth_blockNumber`, and `eth_getBalance` at that exact block. Use Alice’s address from setup. Decode each quantity with Python integers. Then call an invented method and record how its response differs.

## Part C — Design reasoning

How should Lab Escrow handle an unavailable receipt differently from a receipt with status zero?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
