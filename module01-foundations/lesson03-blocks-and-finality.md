# Lesson 1.3 — Blocks and finality

**Course:** Ethereum 0–100, Edition 1  
**Module:** [01 — Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Explain the difference between inclusion, canonicality, and finality.
2. Read block and receipt identifiers together.
3. Recognize why a local chain reset invalidates earlier observations.

## 1.3.1 — Ordered history

Each block references a parent by hash. Its number identifies a position and its hash identifies a particular block at that position. The next example shows why that distinction matters when history changes.

Ethereum mainnet separates execution from consensus. Execution clients validate transactions and compute state changes. Consensus clients participate in selecting and tracking the canonical chain using proof of stake. Validators propose blocks and attest to what they observe. Anvil is a local execution simulator with convenient mining controls. Its immediate receipts do not model mainnet consensus or economic finality.

## 1.3.2 — A height is a position; a hash identifies a block

Imagine your indexer stores “processed through block 12.” The node later reports a different block at height 12. The number still matches, but the transactions and resulting state may differ. Recording the pair `(12, block_hash)` makes the original claim checkable.

**Canonical** means the history the network currently treats as the chain. **Included** means a transaction appears in a block. A transaction can be included in a block that is no longer canonical, so these are not synonyms.

| Observation | Interpretation |
|---|---|
| A transaction hash is returned | A request has an identifier; inclusion is not established |
| A receipt names block H with hash X | The endpoint reports execution in X |
| Block H currently has hash X | The receipt agrees with that endpoint's current chain view |
| The relevant chain reports finality covering X | Stronger consensus evidence under that chain's rules |

A query to one endpoint is still a query to that endpoint. Later operational work adds checks around the identity and trustworthiness of the source. For now, recording height and hash is the essential step beyond treating a number as permanent evidence.

## 1.3.3 — Read the evidence together

On the same node used for Lesson 1.2, replace the placeholder with the hash of a mined transfer:

```bash
TX='PASTE_TRANSACTION_HASH'
cast receipt --rpc-url "$RPC_URL" "$TX"
cast block --rpc-url "$RPC_URL" latest
cast rpc --rpc-url "$RPC_URL" eth_getBlockByNumber latest false
```

Look at the receipt’s `blockNumber` and `blockHash`. If later blocks exist, inspect the receipt’s height rather than assuming it is still the latest block. Compare the block hash returned at that height with the hash in the receipt. A matching hash establishes what that endpoint currently reports.

## 1.3.4 — Confirmation policy

A reorganization changes which blocks are canonical. A transaction from an orphaned block may reappear in another block or remain unincluded. More descendants reduce ordinary short-reorg exposure. Ethereum proof-of-stake finality is a consensus guarantee tied to validator votes and penalties. Where the endpoint and chain support them, `safe` and `finalized` block tags express stronger consensus views than `latest`.

Lab Escrow’s indexer will store block hashes alongside progress. An Anvil restart gives you an easy way to practice stale-state handling even though it is a reset of the lab, not a mainnet reorg.

## 1.3.5 — Finality is a policy input

A **confirmation** commonly refers to a block's position relative to later descendants, though applications differ in how they count the inclusion block. State your convention. A count of descendants is useful evidence about depth; it is not the same mechanism as Ethereum proof-of-stake finality.

An application decides what to show at each stage. It might display an observed deposit promptly while waiting for a stronger view before acting outside the chain. The important design choice is to keep those states explicit instead of calling every receipt “final.” Anvil's fast local mining is useful for execution experiments, not a shortcut for measuring mainnet settlement guarantees.

A restart also needs a separate interpretation. It can erase the local deployment itself, even if accounts and chain ID look familiar. A short reorg-recovery exercise later replaces only history after a retained deployment. Both invalidate old assumptions, but the recovery actions differ. A stale deployment requires a new manifest; a replaced suffix may be recoverable by replaying from a still-valid deployment.

## Self-review

Why might a stored block number remain plausible after its contents change? Which evidence distinguishes a retained deployment with changed later history from an entirely reset local chain?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Ethereum proof of stake](https://ethereum.org/developers/docs/consensus-mechanisms/pos/)
