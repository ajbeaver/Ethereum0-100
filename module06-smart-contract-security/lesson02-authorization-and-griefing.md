# Lesson 6.2 — Authorization and griefing

**Course:** Ethereum 0–100, Edition 1  
**Module:** [06 — Smart Contract Security](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Identify the actor authorized to change each state.
2. Explain why tx.origin is unsuitable for access control.
3. Separate one user’s failure from global progress.

## 6.2.1 — Authority belongs to a role

Build a matrix with actions down the rows and actors across columns. Include payer, payee, unrelated address, and a relay contract. For each cell, specify whether the action may succeed and under what state and deadline. An unprotected function that changes a role can bypass an otherwise correct authorization check.

`tx.origin` refers to the transaction’s original sender. If Alice calls a malicious relay and it calls a target that authenticates via `tx.origin`, the target still sees Alice as the origin. An immediate-caller check would instead see the relay. Contract wallets also need authorization models that can recognize the wallet contract as the actor.

## 6.2.2 — One recipient should not halt another

A settlement function that pushes ETH to every recipient in one loop can be blocked by one rejecting receiver or exceed the block’s usable gas budget. Bounded per-deposit settlement plus per-user withdrawal isolates these failures. Track exactly who can direct a payout. If anyone can supply the destination for someone else’s credit, authorization has shifted away from the rightful owner.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity security considerations, tx.origin](https://docs.soliditylang.org/en/latest/security-considerations.html#tx-origin)
