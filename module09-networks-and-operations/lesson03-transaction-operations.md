# Lesson 9.3 — Transaction operations

**Course:** Ethereum 0–100, Edition 1  
**Module:** [09 — Networks and Operations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Describe safe local deployment, smoke testing, and reporting.
2. Distinguish pending, reverted, and unconfirmed application states.
3. Build a runbook that survives an interrupted command.

## 9.3.1 — Record what a retry means

A client can time out after its request was accepted. The absence of a local response does not prove that the transaction failed. If you have a hash, check the receipt and its canonical block. If you have no hash, inspect the sender nonce and application state before deciding what happened. A new nonce means a distinct operation, even if the calldata matches. Retrying an open-deposit action can create a second valid deposit.

## 9.3.2 — Runbook order

Verify chain and deployment first. Simulate with the intended sender, value, and arguments. Broadcast only after checking those inputs. Record the hash as soon as it is available. Fetch the receipt and inspect status and logs. Read the contract state and compare it to the requested change. Document how to reconcile a timeout or a mismatch.

For the capstone demo, start with a new Anvil process and a clean deployment database. Keep a separate dry-run or inspection command that reads state only. Document how the operator can tell which tools were used and which inputs were supplied.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Ethereum transactions](https://ethereum.org/developers/docs/transactions/)
