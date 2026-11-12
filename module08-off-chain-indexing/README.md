# Module 08 — Off-chain Indexing

**Course:** Ethereum 0–100, Edition 1  
**Prerequisite:** [Tokens and Composition](../module07-tokens-and-composition/README.md)

## Purpose

Read contract history into Python, persist a local projection, and reconcile when the chain view changes.

## Learning objectives

1. Filter logs by contract address, topic, and bounded block range.
2. Design stable identifiers for event ingestion.
3. Detect checkpoint mismatch using block hashes.

## Chapters

| Lesson | Textbook | Exercise specification |
|---|---|---|
| 8.1 | [Reading and decoding logs](lesson01-reading-and-decoding-logs.md) | [Exercise](exercises/lesson01/lesson01.md) |
| 8.2 | [Persistence and replay](lesson02-persistence-and-replay.md) | [Exercise](exercises/lesson02/lesson02.md) |
| 8.3 | [Reorg recovery](lesson03-reorg-recovery.md) | [Exercise](exercises/lesson03/lesson03.md) |

## Workflow

1. Read the textbook chapter.
2. Predict before you run the example.
3. Complete the exercise from its written requirements and save your evidence.
4. Review your answers and repeat any experiment you cannot explain.
5. Submit the [100-point assessment](assessment.md) after all three lessons.

## Lab Escrow connection

Build a replayable SQLite index keyed by chain, deployment, and canonical block evidence.

**Status:** Not started

**Next module:** [Networks and Operations](../module09-networks-and-operations/README.md)
