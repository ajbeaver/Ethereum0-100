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

## 10.2.2 — Replay is another implementation of the state machine

A decoder turns bytes into typed event fields. The projector then applies the capstone's transition rules. It should not treat every well-shaped event as automatically meaningful. Released for a missing ID, a second settlement, a mismatched amount, or a withdrawal exceeding credit indicates inconsistent evidence or a faulty projection.

For a release, require an active record whose stored payee and principal agree with the event. Move that principal out of the active total and into that payee's credit. Refund uses the stored payer instead. Withdrawn consumes the credit owner's entitlement; the recipient is a payout destination, not necessarily the account whose credit decreases.

These checks do not replace on-chain verification. They make the projection's assumptions explicit and prevent corrupt data from silently accumulating into a plausible table.

## 10.2.3 — One accepted prefix, one transaction

Capture explicit range boundaries and validate their block context. Insert source logs, apply only newly accepted events, and advance the checkpoint together. If decoding or a transition check fails, roll back that import range and retain the previous accepted checkpoint. A printed “processed through H” must describe committed state.

Use the single-writer lab model and transactional connection behavior from Lesson 8.2. Store uint256-domain amounts losslessly, perform arithmetic as Python integers, and expose decimal representations deliberately. A small SQLite schema is sufficient; a message broker or indexing framework would add machinery without clarifying the required guarantees.

## 10.2.4 — Verification is part of the feature

Expose a command that prints deposit status, each observed user’s credit, active principal, unpaid total, and indexed height and hash. Pin contract getter queries to that height and verify the block hash around the read. Report differences with the specific record and observed values. The projection may lag the head. Show its indexed height explicitly.

## 10.2.5 — Reconcile both totals and ownership

Suppose the projected unpaid total is 7 and the getter also returns 7. If the projection assigns it to Alice while the contract assigns it to Bob, the aggregate match hides a serious error. Compare deposit states, stored parties/amounts, individual credits, and totals at the shared indexed block.

Print that height and hash beside the result, including when the index lags behind the head. Recheck the hash around the getter read set. A mismatch means the comparison is no longer about one stable prefix, so discard it rather than interpreting mixed-time values as an accounting defect.

The verification command is part of the delivered interface because it makes the derived view auditable. A successful ingestion command alone cannot establish that the intended contract and projector agree.

## Self-review

Which well-encoded events should the projector still reject? Why does matching aggregate unpaid credit not establish correct credit ownership?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Python sqlite3](https://docs.python.org/3/library/sqlite3.html)
