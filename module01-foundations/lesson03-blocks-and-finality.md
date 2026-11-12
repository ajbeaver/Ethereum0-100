# Lesson 1.3 — Blocks and finality

**Course:** Ethereum 0–100, Edition 1  
**Module:** [01 — Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Explain the difference between inclusion, canonicality, and finality.
2. Read block and receipt identifiers together.
3. Recognize why a local chain reset invalidates earlier observations.

## 1.3.1 — Ordered history

Each block references a parent by hash. The block number is its position in a chain. The block hash identifies a particular block. Several candidate blocks can exist at the same height, so a height alone cannot prove that the history you read still holds.

Ethereum mainnet separates execution from consensus. Execution clients validate transactions and compute state changes. Consensus clients participate in selecting and tracking the canonical chain using proof of stake. Validators propose blocks and attest to what they observe. Anvil is a local execution simulator with convenient mining controls. Its immediate receipts do not model mainnet consensus or economic finality.

## 1.3.2 — Read the evidence together

On the same node used for Lesson 1.2, replace the placeholder with the hash of a mined transfer:

```bash
TX='PASTE_TRANSACTION_HASH'
cast receipt --rpc-url "$RPC_URL" "$TX"
cast block --rpc-url "$RPC_URL" latest
cast rpc --rpc-url "$RPC_URL" eth_getBlockByNumber latest false
```

Look at the receipt’s `blockNumber` and `blockHash`. If later blocks exist, inspect the receipt’s height rather than assuming it is still the latest block. Compare the block hash returned at that height with the hash in the receipt. A matching hash establishes what that endpoint currently reports.

## 1.3.3 — Confirmation policy

A reorganization changes which blocks are canonical. A transaction from an orphaned block may reappear in another block or remain unincluded. More descendants reduce ordinary short-reorg exposure. Ethereum proof-of-stake finality is a consensus guarantee tied to validator votes and penalties. Where the endpoint and chain support them, `safe` and `finalized` block tags express stronger consensus views than `latest`.

Lab Escrow’s indexer will store block hashes alongside progress. An Anvil restart gives you an easy way to practice stale-state handling even though it is a reset of the lab, not a mainnet reorg.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Ethereum proof of stake](https://ethereum.org/developers/docs/consensus-mechanisms/pos/)
