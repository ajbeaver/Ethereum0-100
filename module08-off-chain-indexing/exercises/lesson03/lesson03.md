# Exercise 8.3 — Reorg recovery

**Textbook:** [Reorg recovery](../../lesson03-reorg-recovery.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

A stored cursor is height 9 and the node now reports height 8. What should the indexer show while reconciling? How is that different from seeing no logs at height 9?

## Part B — Investigate and build

Extend `journal.py` with checkpoint hashes. Use the snapshot/revert workflow to create two histories above the same deployed feed. Reject stale progress, replay from deployment, and prove the final view includes only the replacement report. Save the block evidence in `recovery.md`.

## Part C — Design reasoning

Why is the database’s chain ID insufficient to detect either a reorg or a local reset?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson03-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
