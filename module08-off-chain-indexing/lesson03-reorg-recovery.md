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

## 8.3.2 — Test two different cases

Use `cast rpc --rpc-url "$RPC_URL" evm_snapshot` on Anvil after deploying ReadingFeed. Add a report and index it. Restore using `evm_revert` with the snapshot ID, then add a different report. This keeps the deployment while replacing the newer history. A full Anvil restart can also invalidate the deployment itself. Your application should detect that separately and require a new deployment record before resuming.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Ethereum JSON-RPC, eth_getBlockByNumber](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getblockbynumber)
