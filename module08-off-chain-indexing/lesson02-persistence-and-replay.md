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

SQLite supports unique constraints and transactions. Ingesting the same range twice should leave the same journal and projection. Write the events and advance the cursor in one database transaction. A crash between separate commits can skip events or apply them twice. If the application updates a derived balance, apply a log only when its journal row was newly inserted, or rebuild the view from the full journal.

## 8.2.2 — Commit a bounded batch

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

## 8.2.3 — Preserve the integer domain

SQLite INTEGER is signed 64-bit, smaller than Solidity’s uint256. Store arbitrary wei amounts as canonical decimal TEXT or fixed-size binary. Convert to Python `int` for arithmetic. SQL text sorts digit strings lexicographically, so order by block and log position rather than by amount text.

A minimal journal needs emitter, block number and hash, transaction hash and index, log index, topics, and data. A projection for Lab Escrow can track deposit status and credit per owner. One source event may update several projection rows, so checkpoint the whole change atomically.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Python sqlite3](https://docs.python.org/3/library/sqlite3.html)
