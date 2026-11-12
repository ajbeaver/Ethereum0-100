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

## 10.3.3 — Organize evidence by the claim it supports

The clean-run sequence establishes that the submitted source and instructions can recreate the system. It also needs observable checkpoints. Retain the manifest and the receipts required above alongside indexed height/hash and getter comparisons at each meaningful stage.

The expected-state table is an independent oracle. Work through its arithmetic before executing: 7 plus 11 gives 18 active wei; releasing 7 moves it to unpaid credit; refunding 11 leaves 18 unpaid; withdrawals remove 7 then 11 from both obligations and assets. Each party's credit explains who is entitled to the intermediate totals.

Keep payer account balances separate from the vault ledger. The payer also pays transaction fees, so a wallet balance delta is not automatically equal to the contract's principal change. Use receipts to explain fees and getters/events to explain obligations.

## 10.3.4 — Establish what the tests prove

A reverting receiver must preserve the owner’s credit. A malicious callback must fail to withdraw more than the entitlement, while an unrelated credit remains intact. Fuzz bounded amounts and action sequences. Check per-user obligations as well as aggregate solvency. Demonstrate an unsolicited balance increase through a local fixture or Anvil balance adjustment and show that it creates no user credit.

Your final README should name the supported network, actors, compiler and tool versions, commands from an empty setup, and the location of the design and test evidence. Document the chosen deadline policy, local-only signer assumptions, indexer rebuild policy, and transaction retry behavior.

## 10.3.5 — Demonstrate recovery without concealing identity changes

For the replacement-suffix experiment, retain the same deployment and restore an after-deployment snapshot. Show the old checkpoint, the new block hash at its height, the discarded suffix, and the rebuilt projection. A tool that deletes every file and redeploys from scratch has demonstrated a clean install, not recovery from changed history under a retained deployment.

A separate full-reset case can prove the stale-manifest guard. It should refuse to continue against nonexistent or mismatched creation evidence. Refusal is useful behavior when automatic recovery would fabricate the identity it is supposed to verify.

## 10.3.6 — State the scope of confidence

Unit tests establish specific transitions and rejection cases. Fuzz/invariant tests explore more inputs and action sequences within their configured domain. Reconciliation establishes agreement between a chosen on-chain snapshot and a derived view. A clean run establishes reproducibility of the provided workflow. None alone is a production security audit.

The handoff should make those distinctions clear through concrete evidence and limitations. Record the local-only account model, supported chain, pinned build settings, single-writer indexing assumptions, and retry policy. Preserve reproducible tests for discovered defects rather than retaining unexplained manual workarounds.

Completion means you can explain the path from a payer's intent through execution, receipt logs, persisted events, derived obligations, and verification. The capstone's small scope makes that end-to-end explanation possible without hiding behind tooling.

## Self-review

Which bugs could be hidden by checking only the final zero balance? How does the recovery demonstration prove more than a fresh deployment does?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Foundry invariant testing](https://www.getfoundry.sh/guides/invariant-testing)
