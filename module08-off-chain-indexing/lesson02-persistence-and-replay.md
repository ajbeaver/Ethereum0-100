# Lesson 8.2 — Persistence and replay

**Course:** Ethereum 0–100, Edition 1  
**Module:** [08 — Off-chain Indexing](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Design stable identifiers for event ingestion.
2. Commit logs and progress together.
3. Rebuild a projection from ordered events.

## 8.2.1 — The event journal

Store raw logs before projecting a human-readable table. For an index bound to a network and deployment, a useful log identity includes block hash, transaction hash, and log index. Hashes preserve a distinction between copies of a transaction in different block histories. Also store the number for ordering and range queries.

SQLite provides unique constraints for event identity and transactions for grouped changes. The design must connect both mechanisms to the journal, derived state, and resume position.

## 8.2.2 — A projection is a derived view

The **journal** retains accepted source events. A **projection** computes convenient current tables from them, such as status per deposit and credit per user. The **checkpoint** identifies how far the journal/projection pair has been processed. These are three related responsibilities, not three interchangeable copies of one value.

For a credit event of 7 wei, importing the same log twice must leave credit at 7, not 14. Deduplicating only the journal is insufficient if projection code runs again regardless of whether insertion was new. Either project newly accepted events exactly once within the import transaction, or rebuild the projection from the deduplicated ordered journal.

An identity collision should not silently accept conflicting payloads. If a supposedly identical log identity arrives with different topics or data, stop and investigate the inconsistent evidence. Duplicate handling means recognizing the same event, not permitting arbitrary overwrite of its content.

## 8.2.3 — Commit a bounded batch

A simplified Python transaction boundary looks like this:

```python
with connection:
    connection.executemany(
        "INSERT INTO journal (log_key, amount_wei) VALUES (?, ?) "
        "ON CONFLICT(log_key) DO NOTHING",
        log_rows,
    )
    connection.execute(
        "UPDATE checkpoint SET height = ?, block_hash = ? WHERE id = 1",
        (ending_height, ending_hash),
    )
```

This example assumes a unique `log_key`, a TEXT `amount_wei`, an existing checkpoint row, and a connection configured for transactional writes. An exception inside the block rolls back the pending transaction. It illustrates the commit boundary for a journal and cursor. If you update a persisted projection during ingestion, include those updates in the same transaction.

## 8.2.4 — Trace a crash boundary

Imagine events are committed, but the checkpoint is not. A restart requests the range again. Without idempotency, it can double-apply obligations. Now reverse the order: the checkpoint commits before its events. A restart skips a range whose evidence was never stored. One transaction groups the journal, derived changes, and progress into one application-level unit.

For Python 3.12+, an explicit pattern is `sqlite3.connect(path, autocommit=False)`. The shown `with connection:` then commits on successful exit and rolls back on an escaping exception. Create the schema and initial checkpoint first and commit them; close the connection explicitly afterward. `autocommit=True` would not give the fragment the intended transaction guarantee.

## 8.2.5 — Preserve the integer domain

SQLite INTEGER is signed 64-bit, smaller than Solidity’s uint256. Store arbitrary wei amounts as canonical decimal TEXT or fixed-size binary. Convert to Python `int` for arithmetic. SQL text sorts digit strings lexicographically, so order by block and log position rather than by amount text.

For example, storing `10**20` as decimal text preserves its exact digits; converting that text back to Python int permits exact addition. Do not use SQL `SUM` over amount text and assume uint256 precision: SQL numeric coercion can exceed the integer domain and lose accuracy. Compute the teaching projection's arithmetic with Python integers.

Likewise, JSON presentation should choose an explicit convention for large amounts, commonly decimal strings for clients whose number type has limited precision. The representation is an interface choice, while the accounting remains integer arithmetic.

A minimal journal needs emitter, block number and hash, transaction hash and index, log index, topics, and data. A projection for Lab Escrow can track deposit status and credit per owner. One source event may update several projection rows, so checkpoint the whole change atomically.

## Self-review

Why can a deduplicated journal still coexist with double-applied credit? Which commit ordering failures are prevented by putting events, projection updates, and checkpoint in one transaction?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Python sqlite3](https://docs.python.org/3/library/sqlite3.html)
