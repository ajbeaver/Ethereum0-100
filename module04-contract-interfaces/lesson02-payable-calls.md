# Lesson 4.2 — Payable calls

**Course:** Ethereum 0–100, Edition 1  
**Module:** [04 — Contract Interfaces](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Trace ETH into and out of a contract.
2. Explain the consequences of an external call returning false.
3. Distinguish explicit payable methods from receive and fallback.

## 4.2.1 — Receiving ETH deliberately

A payable method can require a positive `msg.value` and associate that amount with an ID or account. An empty-data transfer normally enters `receive()` when defined. Unmatched calldata reaches `fallback()` when defined. A contract with neither appropriate payable entry point typically rejects a plain transfer. ETH can still reach an address outside its normal deposit path, for example by balance transfer via `SELFDESTRUCT`.

## 4.2.2 — Paying another address

```solidity
error PayoutFailed();
(bool paid, ) = recipient.call{value: amount}("");
if (!paid) revert PayoutFailed();
```

A call can invoke recipient code. Low-level `call` reports success as a Boolean, and its return bytes can contain revert data. If you ignore false after clearing the recipient’s credit, you may commit a state change without actually paying. Revert to restore the claim. Update the claim before the call to limit reentrancy, then examine cross-function interactions in Module 06.

## 4.2.3 — Accounting and failure

A rejecting contract should be unable to block unrelated users’ payouts. One approach records a credit and lets each recipient initiate a withdrawal. Separate the contract balance, owed amounts, and the amount requested in a withdrawal. Lab Escrow will credit a party when a deposit settles and let that party direct the withdrawal to a payable recipient.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity receive and fallback](https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function)
