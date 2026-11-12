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

`tx.origin` identifies the transaction’s original sender, not the immediate caller. The relay example below shows why that makes it unsuitable for this access-control boundary.

## 6.2.2 — Authorization is a relationship, not a special address

For each action, write the object being affected and whose authority controls it. A payer controls release/refund of its active deposit. A credit owner controls withdrawal of its credit. A recipient may receive ETH without gaining authority over the owner's other claims.

The relay case makes `tx.origin` misleading. Suppose Alice authorizes only an interaction with a relay, which then calls a vault. An origin-based rule can mistake Alice's presence at the start of the transaction for Alice authorizing this particular downstream action. The immediate-caller boundary instead identifies the relay as the actor that reached the vault.

A contract wallet is also an immediate caller. Requiring every legitimate actor to be an EOA would unnecessarily exclude it. Model authority using the supported actor/interface contract rather than a blanket belief that code-bearing accounts are malicious.

## 6.2.3 — Failure isolation and legitimate progress

A **safety** rule prevents an unauthorized payment. A **liveness** concern asks whether a legitimate action can eventually make progress. A vault that never pays anyone avoids theft in a trivial sense while failing its purpose. Griefing exploits the ability to prevent progress, sometimes without directly stealing assets.

Consider a function looping over 100 recipients and requiring every send to succeed. If recipient 41 rejects, the transaction can revert payments to the earlier recipients too. Even with cooperative recipients, an ever-growing list may become too expensive to process in one transaction. Those are independent reasons to prefer bounded per-deposit settlement and individually initiated withdrawals.

With per-user credit, Bob's failed withdrawal preserves Bob's claim. Carol can attempt hers separately. The system still needs clear authority over payout destinations: Alice must not be able to consume Bob's credit by selecting herself as recipient. In the capstone, `msg.sender` identifies the credit being consumed, while the supplied recipient chooses where that owner's payment goes.

Test both rejection and legitimate progress. A wrong-role test should leave the record intact, and a separate authorized test should demonstrate the expected change. This prevents a broken function that always reverts from looking like successful access control.

## Self-review

How can a contract prevent theft but still fail legitimate users? Which distinction between credit owner and recipient determines authorization for withdrawal?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity security considerations, tx.origin](https://docs.soliditylang.org/en/latest/security-considerations.html#tx-origin)
