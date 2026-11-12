# Lesson 10.2 — Indexer and reconciliation

**Course:** Ethereum 0–100, Edition 1  
**Module:** [10 — Lab Escrow Capstone](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Build a Python SQLite projection from the four capstone events.
2. Make replay idempotent and reject invalid deployment history.
3. Compare projected state with on-chain getters at one block.

## 10.2.1 — Inputs, storage, and replay

Create `indexer.py` with an explicit RPC URL, deployment manifest path, and database path. The deployment manifest carries chain ID, contract address, creation transaction hash, block number and hash, and runtime-code hash. Validate it before importing. Use the Python standard library for RPC and SQLite. You may call Cast to compute Keccak topic hashes once or capture and document the expected hashes in the decoder.

Scan bounded ranges starting at the creation block. Filter by emitter and the four known signature topics. Decode integers as Python ints and persist arbitrary wei amounts losslessly. Store raw log identity, order, and block hash. Apply events in ascending numeric chain order:

1. Opened creates a deposit and increases active principal.
2. Released or Refunded changes an active record to its terminal state and adds the specified credit.
3. Withdrawn subtracts credit from the owner, who can differ from the recipient.

Reject impossible transitions, invalid field sizes, and negative credit. Store the journal, projection updates, and range checkpoint in one database transaction. A mismatch at the checkpoint triggers a rebuild from verified deployment history or a clear error when deployment itself is stale.

## 10.2.2 — Verification is part of the feature

Expose a command that prints deposit status, each observed user’s credit, active principal, unpaid total, and indexed height and hash. Pin contract getter queries to that height and verify the block hash around the read. Report differences with the specific record and observed values. The projection may lag the head. Show its indexed height explicitly.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Python sqlite3](https://docs.python.org/3/library/sqlite3.html)
