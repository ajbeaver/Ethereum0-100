# Exercise 4.1 — Events and receipts

**Textbook:** [Events and receipts](../../lesson01-events-and-receipts.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

For `ReadingSet`, predict the number and order of topics and the data length. Predict what survives if the function emits and then reverts.

## Part B — Investigate and build

Create `src/ReadingFeed.sol` with the example event and a reporting function. Send two reports with different IDs and writers. Save the receipts in `events.md` and decode address, topic zero, ID, writer, and value. Explain how a filter would select a single ID.

## Part C — Design reasoning

Why should the escrow indexer filter on both the event topic and the emitting address?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
