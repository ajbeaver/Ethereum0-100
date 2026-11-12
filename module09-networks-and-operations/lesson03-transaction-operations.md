# Lesson 9.3 — Transaction operations

**Course:** Ethereum 0–100, Edition 1  
**Module:** [09 — Networks and Operations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Describe safe local deployment, smoke testing, and reporting.
2. Distinguish pending, reverted, and unconfirmed application states.
3. Build a runbook that survives an interrupted command.

## 9.3.1 — Timeouts and transaction evidence

A timeout leaves the client uncertain whether its request was accepted. The example below shows why that uncertainty must be reconciled before retrying a value-moving operation.

Suppose the node accepts an open-deposit transaction, but the client loses its connection before receiving the response. Repeating the business request with a fresh nonce can open another deposit. The original request might have succeeded even though the first terminal never printed success.

Keep the stages separate:

| Stage | Evidence | Next question |
|---|---|---|
| Prepared | Validated inputs and intended sender nonce | Was it submitted? |
| Submitted | Hash or other submission record | Was it included canonically? |
| Included | Receipt status and block identity | Did intended application state result? |
| Reconciled | Getters/events match the requested effect | Is the required confirmation policy satisfied? |

If you have a hash, inspect its receipt and canonical block. Without one, inspect the sender nonce and application state. A nonce advancing shows that some transaction occupied a position, not which application action occurred; match the available transaction data, receipts, events, and contract state. If evidence remains ambiguous, report that state instead of silently retrying a value-moving action.

## 9.3.2 — Define the unit of a retry

Rebroadcasting the same signed transaction preserves its hash and nonce. A replacement uses the same sender nonce with changed transaction terms under the node's acceptance policy. A new nonce defines a distinct transaction. These are different operational actions even when a user describes all of them as “try again.”

Business-level idempotency is another layer. Lab Escrow's repeated release rejects because the deposit is already terminal. Opening another deposit can be perfectly valid, so the contract cannot infer that a second open was only a retry. The operator's runbook must address that ambiguity.

For the local demo, capture input parameters, sender, hash, receipt, and resulting deposit ID together. A read-only inspection command should be able to show enough state to resume after a terminal closes. Keep generated transaction evidence separate from the public course source and avoid assuming exported shell variables survive into a new session.

The runbook is successful when another person can determine the current stage and next safe action from recorded evidence. A sequence of commands without starting-state assumptions or recovery branches is only a happy-path transcript.

## 9.3.3 — Runbook order

Verify chain and deployment first. Simulate with the intended sender, value, and arguments. Broadcast only after checking those inputs. Record the hash as soon as it is available. Fetch the receipt and inspect status and logs. Read the contract state and compare it to the requested change. Document how to reconcile a timeout or a mismatch.

For the capstone demo, start with a new Anvil process and a clean deployment database. Keep a separate dry-run or inspection command that reads state only. Document how the operator can tell which tools were used and which inputs were supplied.

## Self-review

Why does a sender nonce check not fully identify a timed-out open-deposit action? How do rebroadcast, replacement, and a new transaction differ?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Ethereum transactions](https://ethereum.org/developers/docs/transactions/)
