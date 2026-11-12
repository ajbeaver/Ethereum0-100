# Lesson 9.2 — Layer 2 and bridge semantics

**Course:** Ethereum 0–100, Edition 1  
**Module:** [09 — Networks and Operations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Describe what a rollup executes and what it uses L1 for.
2. Separate local inclusion, settlement, and cross-chain completion.
3. Identify chain-specific assumptions in fees and assets.

## 9.2.1 — Separate the views of progress

An Ethereum rollup executes transactions in its own environment and uses Ethereum L1 for settlement, with data availability and verification arrangements defined by that rollup. Optimistic designs use a dispute process to challenge incorrect assertions. Validity designs use proofs checked by the settlement system. Sequencers can provide early execution evidence. Early L2 inclusion, L1 acceptance, and withdrawal completion can occur at different times.

A bridge action can involve a transaction on the source chain, message processing, and a transaction or claim on the destination chain. A successful source receipt proves the source action executed. Track the message’s destination state separately.

## 9.2.2 — Avoid assuming EVM-equivalence means fee-equivalence

Fee components and finality signals vary. Some networks charge separately for L1 data publication. Display contract addresses and token identities with their network. A capstone estimate based on gas used times price is useful for local execution and does not automatically describe total fees on a rollup.

Lab Escrow remains on one local chain. In a future cross-chain version, each deposit would need a separate model for message submission, confirmation, replay protection, and destination settlement.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum scaling documentation](https://ethereum.org/developers/docs/scaling/)
