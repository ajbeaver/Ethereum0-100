# Lesson 6.1 — Reentrancy

**Course:** Ethereum 0–100, Edition 1  
**Module:** [06 — Smart Contract Security](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Explain how a recipient can call back before the original invocation returns.
2. Apply checks, effects, and interactions to a withdrawal.
3. Test both same-function and cross-function callback paths.

## 6.1.1 — A recipient is executable code

A withdrawal might read a credit, send ETH to the caller, then clear the credit. During the send, a recipient contract can call back into withdrawal. Since the original call has yet to clear the credit, the callback can see the same entitlement. This is observable even though Ethereum processes transactions in order. The nested calls are part of one transaction.

## 6.1.2 — Trace the vulnerable ordering

Reentrancy does not require two transactions to execute simultaneously. The vulnerability occurs when one execution hands control to another contract before it has restored the invariants other entry points rely on.

Suppose an attacker has 7 wei of credit and the vault also holds an honest user's 11 wei. In a vulnerable withdrawal:

| Step | Attacker's stored credit | Consequence |
|---|---:|---|
| Read credit | 7 | Original call decides to pay 7 |
| Send before clearing | 7 | Recipient code runs with the claim still visible |
| Callback reads credit | 7 | Nested call can decide to pay another 7 |
| Both calls eventually clear | 0 | More ETH left than one claim allowed |

The honest user's extra funds make the second payment possible. Without them, the callback might fail for insufficient balance, hiding the ordering defect. A good attack fixture therefore establishes both the entitlement being abused and enough unrelated assets to reveal the loss.

## 6.1.3 — Effects establish what callbacks can observe

Checks validate the current request. Effects update credit and aggregate liabilities. Interactions call external code. With this order, a callback sees the credit already consumed. If the recipient rejects and the outer function reverts, the effects roll back, restoring the claim.

A reentrancy guard rejects protected nested entry. It does not replace a coherent state model: review every public entry point that can observe or change the shared accounting. Two guarded functions can still be incorrectly designed around unguarded functions reading inconsistent totals. Review which invariants are true at every external call, including calls that appear read-only from the caller's perspective.

Lab Escrow keeps settlement free of recipient interaction: it creates credit. Withdrawal performs the interaction later. During a permitted callback into another function, that function must see consistent active principal, credit, and unpaid totals. The threat model should say which entries are allowed and why they remain safe.

An attack test should let the receiver record the nested rejection without forcing the entire legitimate withdrawal to fail. Assert the total payment, final credit, aggregate liabilities, and honest depositor's claim. Those observations distinguish a protected one-payment withdrawal from a contract that simply rejects every recipient contract.

## 6.1.4 — Construct the attack test

Use a local receiver whose receive hook attempts one callback. Have it catch or record failure so that a protected withdrawal can still complete. Fund the fixture with a second honest depositor so there is enough balance for the vulnerable path to pay twice. Assert both receiver gain and preservation of the honest depositor’s claim.

Lab Escrow settles by crediting a payee without an external call. Withdrawal updates liability and account credit before calling the recipient. A reentrancy guard can also protect the shared withdrawal path. Keep the test focused on observable loss or double payment.

## Self-review

Why must an attack fixture include assets beyond the attacker's own credit? Which invariant should a callback observe after withdrawal effects have been applied?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity security considerations](https://docs.soliditylang.org/en/latest/security-considerations.html)
