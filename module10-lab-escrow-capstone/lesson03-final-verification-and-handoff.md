# Lesson 10.3 — Final verification and handoff

**Course:** Ethereum 0–100, Edition 1  
**Module:** [10 — Lab Escrow Capstone](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Demonstrate the system from a clean local chain and empty database.
2. Prove critical security and recovery cases with reproducible evidence.
3. Communicate the implemented scope and remaining assumptions.

## 10.3.1 — Build a reproducible demonstration

Start a fresh loopback Anvil instance. Build and test the contract. Deploy and record the manifest. Open two deposits with different roles or deadlines. Release one before its deadline, refund the other at or after its deadline, and withdraw each credit. Record all receipts and reconcile the indexer with the pinned state.

Then use a snapshot taken after deployment to produce and replace a suffix. Confirm the indexer detects the old checkpoint and that the rebuilt view reflects only the replacement history. Keep the replay demonstration separate from the release and refund smoke test so the two sets of evidence stay readable.

## 10.3.2 — Compare the contract and indexer

Here is a small expected-state table for a clean run. Alice opens deposit 1 for Bob with 7 wei and deposit 2 for Carol with 11 wei. Alice releases deposit 1 in time and refunds deposit 2 at its deadline.

| Action completed | Deposit 1 | Deposit 2 | Active wei | Unpaid wei | Vault wei |
|---|---|---|---:|---:|---:|
| Open both | Active | Active | 18 | 0 | 18 |
| Release 1 | Released | Active | 11 | 7 | 18 |
| Refund 2 | Released | Refunded | 0 | 18 | 18 |
| Bob withdraws | Released | Refunded | 0 | 11 | 11 |
| Alice withdraws | Released | Refunded | 0 | 0 | 0 |

Bob’s credit reaches 7 and Alice’s reaches 11. Carol has no credit because her deposit was refunded. Compare these values at the corresponding block hashes, especially after release and refund. Checking only the final zero balance would miss some incorrect intermediate states.

## 10.3.3 — Establish what the tests prove

A reverting receiver must preserve the owner’s credit. A malicious callback must fail to withdraw more than the entitlement, while an unrelated credit remains intact. Fuzz bounded amounts and action sequences. Check per-user obligations as well as aggregate solvency. Demonstrate an unsolicited balance increase through a local fixture or Anvil balance adjustment and show that it creates no user credit.

Your final README should name the supported network, actors, compiler and tool versions, commands from an empty setup, and the location of the design and test evidence. Document the chosen deadline policy, local-only signer assumptions, indexer rebuild policy, and transaction retry behavior.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Foundry invariant testing](https://www.getfoundry.sh/guides/invariant-testing)
