# Exercise 2.3 — ABI and calldata

**Textbook:** [ABI and calldata](../../lesson03-abi-and-calldata.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Predict the total byte length of `setValue(uint256)` with one argument. Predict which bytes change when the value goes from 255 to 256. Then use Cast to check.

## Part B — Investigate and build

Write `calldata.md`. Encode `record(address,uint256)` using Bob’s address and 257. Split the selector and both words and independently decode the address and amount. Repeat with `record(address,string)` using the same address and `lab`. Mark the dynamic offset and describe where it points.

## Part C — Design reasoning

Explain why sending correctly encoded `release(uint256)` calldata to an unverified target can still perform the wrong operation.

## Submission

Keep predictions, commands, observed results, and design answers in `lesson03-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
