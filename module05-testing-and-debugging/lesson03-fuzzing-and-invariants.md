# Lesson 5.3 — Fuzzing and invariants

**Course:** Ethereum 0–100, Edition 1  
**Module:** [05 — Testing and Debugging](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Use fuzz input to check a rule over many examples.
2. Distinguish single-operation properties from sequence invariants.
3. State accounting conditions suitable for an escrow.

## 5.3.1 — Generalize the example

A unit test may verify a single deposit of 1 ETH. A fuzz test accepts generated inputs and checks the same rule across many amounts. Bound inputs to a range that makes the desired action valid. For example, choose `amount = bound(raw, 1, 10 ether)`, set the payer balance with `vm.deal`, then assert the observed credit equals the integer amount.

Test rejected inputs separately. If you discard every invalid sample, the fuzz test provides no evidence about rejection behavior. When Forge reports a counterexample, capture it as a focused deterministic test.

## 5.3.2 — Check a property after sequences

Invariant testing runs sequences of calls and checks a property across reachable states. A handler can select meaningful actors and actions, provide funds, and keep an independent record of expected obligations. Use the handler to reach both settlement paths. Confirm operations actually succeed so a suite of only rejected calls cannot pass vacuously.

For Lab Escrow, tracked liabilities consist of active principal plus unpaid credits. Successful settlement moves an amount between these categories. Successful withdrawal reduces unpaid credits and contract balance equally. The solvency condition is `balance >= active principal + unpaid credits`. Equality is expected in controlled runs without unsolicited ETH.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Foundry invariant testing](https://www.getfoundry.sh/guides/invariant-testing)
