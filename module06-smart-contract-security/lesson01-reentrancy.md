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

Move the credit update before the external call. If payment fails, revert to restore the original value. Review every public entry point that can observe or change the shared state. A second function may rely on the same accounting even if the first function protects its own local variables.

## 6.1.2 — Construct the attack test

Use a local receiver whose receive hook attempts one callback. Have it catch or record failure so that a protected withdrawal can still complete. Fund the fixture with a second honest depositor so there is enough balance for the vulnerable path to pay twice. Assert both receiver gain and preservation of the honest depositor’s claim.

Lab Escrow settles by crediting a payee without an external call. Withdrawal updates liability and account credit before calling the recipient. A reentrancy guard can also protect the shared withdrawal path. Keep the test focused on observable loss or double payment.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity security considerations](https://docs.soliditylang.org/en/latest/security-considerations.html)
