# Exercise 3.1 — Contracts and deployment

**Textbook:** [Contracts and deployment](../../lesson01-contracts-and-deployment.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

If you deploy Counter twice with initial values 7 and 20, predict both getter results after incrementing only the second deployment. Predict whether their runtime code differs.

## Part B — Investigate and build

Create `src/Meter.sol` in a new project. Give it a constructor that initializes a public unsigned reading and an external function that increases it by an unsigned delta. Deploy two instances and demonstrate independent storage with calls and receipts. Include the Foundry configuration.

## Part C — Design reasoning

Which pieces of information would let someone else reproduce a deployment and distinguish it from a second instance of the same source?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
