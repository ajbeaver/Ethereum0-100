# Exercise 3.2 — Types and storage

**Textbook:** [Types and storage](../../lesson02-types-and-storage.md)  
**Estimated time:** 25–45 minutes

## Part A — Predict

For a never-written key, predict both fields in `entries[id]`. For an existing amount of 30, predict the memory and storage values after the textbook fragment.

## Part B — Investigate and build

Build `src/ReadingBook.sol` with an internal mapping from ID to a struct containing a value and an existence flag. Add a write method, a read method, and a preview method that copies the struct to memory and returns its value plus one. Confirm the preview leaves the saved record unchanged. Include a record with value zero.

## Part C — Design reasoning

Why would checking only `amount == 0` make it harder to distinguish never-created deposits from fully settled deposits?

## Submission

Keep predictions, commands, observed results, and design answers in `lesson02-answers.md`. Include any source files required in Part B.

- [ ] I wrote my prediction before running the lab.
- [ ] I recorded enough evidence to reproduce the result.
- [ ] I explained any difference between my prediction and observation.
- [ ] I checked the exercise requirements against my submission.

**Status:** Not submitted
