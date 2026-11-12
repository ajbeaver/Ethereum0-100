# Lesson 8.3 — Reorg recovery

**Course:** Ethereum 0–100, Edition 1  
**Module:** [08 — Off-chain Indexing](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Detect checkpoint mismatch using block hashes.
2. Replay a short lab history after it changes.
3. Verify derived state against on-chain reads at a shared block.

## 8.3.1 — Treat the cursor as a claim

A checkpoint says which block was processed and which hash was observed there. Before resuming, query that height. If the hash differs or the endpoint has no such block, the stored history may have been replaced. Stop projecting that history as current. In production you can walk back to a known common ancestor, delete the orphaned suffix, and replay. On a short local chain, a full rebuild is acceptable if the recorded deployment block and code still match.

Store checkpoints for empty ranges too. An event-less block can still be replaced. While processing a range, verify returned log hashes against the block headers you accept, and recheck the ending block hash before committing. A remote reorg can still occur immediately afterward, so verify again on the next poll.

## 8.3.2 — Check what a checkpoint actually asserts

A checkpoint `(height, hash)` says the index reflects a particular prefix of history. It does not say the application merely saw that many blocks. Before resuming, compare the stored hash with the node's block at that height. A missing block or changed hash invalidates the claim even when no deposit event occurred in that block.

Suppose block 8 opened a deposit and block 9 released it. The journal and projection now show a payee credit. A replacement block 9 instead contains no release. Keeping the old terminal status while scanning forward from 10 would preserve an event that is no longer canonical. Recovery must remove the orphaned contribution, not merely add newer events.

For a short lab chain, rebuilding from a verified deployment is easier to explain than reversing every event. It exercises the important property: the projection is reproducible from accepted canonical evidence. A large production history may need a common-ancestor rollback strategy, but that added mechanism is unnecessary for the required exercise.

## 8.3.3 — Keep detection, recovery, and verification separate

Detection finds the mismatch. Recovery selects a valid history and rebuilds the journal/projection coherently. Verification compares that reconstructed state with contract getters at the same block. Passing one stage does not establish the others.

Pin reconciliation reads to the indexed height and check its hash before and after the read set. Comparing an index at block 9 with getters at latest block 12 could report a false bug simply because the views concern different times. If the pinned block changes during the check, discard the comparison and reconcile again.

The Anvil snapshot experiment should be taken after deployment. Restoring it preserves the contract while allowing a different suffix to be produced. A full node restart can remove the creation event and code too. Replaying from an invalid old creation record cannot repair that; require new deployment evidence instead.

Store recovery evidence separately from the ordinary smoke test: original checkpoint, observed mismatch, retained deployment evidence, replayed range, and new reconciled state. This makes it clear whether the tool repaired a replaced suffix or merely started against an unrelated empty database.

## 8.3.4 — Test two different cases

Use `cast rpc --rpc-url "$RPC_URL" evm_snapshot` on Anvil after deploying ReadingFeed. Add a report and index it. Restore using `evm_revert` with the snapshot ID, then add a different report. This keeps the deployment while replacing the newer history. A full Anvil restart can also invalidate the deployment itself. Your application should detect that separately and require a new deployment record before resuming.

## Self-review

Why must event-less blocks be included in checkpoint reasoning? How would you prove a rebuilt projection reflects the replacement suffix rather than its orphaned events?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Ethereum JSON-RPC, eth_getBlockByNumber](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getblockbynumber)
