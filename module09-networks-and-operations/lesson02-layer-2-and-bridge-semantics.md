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

## 9.2.2 — Follow an action across two state machines

A bridge is not one atomic transaction spanning every chain involved. The source chain records one action, a messaging/verification mechanism carries evidence, and the destination chain processes a corresponding action under its own rules. Each stage can have a different completion condition and failure mode.

A useful progress model is:

```text
Source transaction included
    -> Source evidence accepted under bridge policy
    -> Message eligible for destination processing
    -> Destination action executed or explicitly failed
```

This is a reasoning diagram, not a universal bridge API. Actual networks define their own message identifiers, proof requirements, delays, and retry rules. A source receipt proves only the first execution. It does not establish that the destination recipient has received spendable funds.

## 9.2.3 — Chain-specific fees, assets, and trust assumptions

Fee components and finality signals vary. Some networks charge separately for L1 data publication. Display contract addresses and token identities with their network. A capstone estimate based on gas used times price is useful for local execution and does not automatically describe total fees on a rollup.

The optimistic/validity distinction above concerns acceptance of proposed state changes. Neither label alone establishes a particular network's fee formula, withdrawal delay, operator assumptions, or precise finality signal.

The EVM may support familiar Solidity behavior while the operational environment differs. An account needs the appropriate fee asset on the chosen network. A token bridged to a destination may use a different contract address and carry bridge-specific trust assumptions. Copying an L1 address into an L2 tool is not an asset-identification policy.

The capstone deliberately has one chain and no bridge. Use this lesson to identify which assumptions are local to that scope. A cross-chain extension would require message identity, replay prevention, destination authorization, and partial-progress recovery in addition to the existing deposit state machine. Those are new requirements to design, not features obtained automatically by redeploying the same bytecode.

## Self-review

What can a successful source receipt leave unresolved about a bridge transfer? Which operational assumptions remain chain-specific even when a network executes EVM bytecode?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum scaling documentation](https://ethereum.org/developers/docs/scaling/)
