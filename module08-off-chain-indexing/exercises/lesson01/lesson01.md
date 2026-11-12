# Exercise 8.1 — Reading and decoding logs

**Textbook:** [Reading and decoding logs](../../lesson01-reading-and-decoding-logs.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

Predict which of three logs a filter selects: the correct signature from the wrong address, a different signature from the right address, and a matching signature and address within the range.

## Part B — Investigate and build

Redeploy ReadingFeed if needed. Emit at least three reports. Build `decode_logs.py` to load a saved raw log result, validate and decode the expected schema, then print ID, writer, value, and canonical position. Test with the input list reversed.

## Part C — Design reasoning

How could two contracts emitting the same event signature confuse a decoder that ignores the emitter?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson01-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
