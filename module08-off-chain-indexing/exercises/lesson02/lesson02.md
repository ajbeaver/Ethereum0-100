# Exercise 8.2 — Persistence and replay

**Textbook:** [Persistence and replay](../../lesson02-persistence-and-replay.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Your script writes events, then crashes before advancing its checkpoint. What happens on rerun with and without a unique event identity? What if it advances the checkpoint before writing?

## Part B — Investigate and build

Create `journal.py` using Python’s sqlite3. Import your ReadingFeed fixture twice and verify row count remains unchanged. Rebuild a latest-reading view in block and log order. Test an amount above `2**63 - 1` and verify exact round-trip.

## Part C — Design reasoning

How can a database claim it processed an entire range even when it received no matching logs?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
