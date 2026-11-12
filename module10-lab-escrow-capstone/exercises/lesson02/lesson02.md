# Exercise 10.2 — Indexer and reconciliation

**Textbook:** [Indexer and reconciliation](../../lesson02-indexer-and-reconciliation.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

The indexer receives the same Withdrawn event twice after a network retry. Predict results for a replay that deduplicates before applying and one that subtracts credit on every delivery.

## Part B — Investigate and build

Implement `indexer.py` and `test_indexer.py`. Use saved fixtures for duplicate logs, an empty range, a uint256-sized amount, a failed import with retry, and replaced history. Compare one live local projection against pinned contract getters. Include a CLI usage example in `design.md`.

## Part C — Design reasoning

Which errors should stop indexing, and which warrant rebuilding from deployment? Describe how you would distinguish them using the manifest.

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
