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

## 5.3.2 — State a property before generating inputs

A fuzz test is a parameterized experiment. Forge supplies inputs; the test defines what must remain true for the valid domain. For a credit operation, the property might be that credit increases by exactly the accepted amount and an unrelated user's credit does not change.

`bound` maps a generated integer into a chosen range. A Solidity amount such as `10 ether` is an integer number of wei using a source-code unit suffix. Funding the actor with `vm.deal` ensures a valid operation does not fail merely because the test fixture lacks funds. These preparations should support the property without calculating its expected answer from the implementation under test.

A simple Counter property can be explored without the entire escrow. In a test contract with a fresh `counter` from Lesson 5.1, this method checks repeated increments:

```solidity
function testFuzzIncrementCount(uint8 raw) public {
    uint256 steps = bound(uint256(raw), 0, 20);
    uint256 beforeValue = counter.value();
    for (uint256 i = 0; i < steps; i++) {
        counter.increment();
    }
    assertEq(counter.value(), beforeValue + steps);
}
```

The method is a fragment inside that test contract. The bounded loop keeps each run small. It checks a general relationship, while deterministic tests still cover known boundaries and specific regressions.

## 5.3.3 — Accounting invariants across action sequences

Invariant testing runs sequences of calls and checks a property across reachable states. A handler selects actors and actions, provides funds, and keeps an independent record of expected obligations. Use it to reach both settlement paths.

For Lab Escrow, tracked liabilities consist of active principal plus unpaid credits. Successful settlement moves an amount between these categories. Successful withdrawal reduces unpaid credits and contract balance equally. The solvency condition is `balance >= active principal + unpaid credits`. Equality is expected in controlled runs without unsolicited ETH.

Vary the order of actions: open, settle, withdraw, attempt a rejected transition, and open again. This explores relationships that an isolated successful deposit cannot establish.

For principal 7 and credit 0, settling moves the model to principal 0 and credit 7 while leaving vault balance unchanged. Withdrawal then moves credit and balance down by 7. An independent test ledger can predict those changes without simply reading the implementation's totals back as its expected values.

The condition `balance >= liabilities` is necessary but not sufficient. If a bug credits Alice instead of Bob, solvency can still hold. Assert per-user obligations, terminal record states, and meaningful action success as well. A suite in which every action reverts explores very little despite reporting no invariant failure.

## Self-review

What property would distinguish correct total liabilities from correct ownership of those liabilities? Why does bounding successful cases not replace explicit invalid-input tests?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Foundry invariant testing](https://www.getfoundry.sh/guides/invariant-testing)
