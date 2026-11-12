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

## 4.2.2 — Separate acceptance of ETH from recognition of a deposit

A contract balance can increase outside the intended payable deposit function. Therefore “balance increased” does not identify a payer, payee, or deposit ID. Only the supported deposit path should create those obligations. This is the reason solvency later uses an inequality that permits surplus.

`receive()` handles empty calldata when available; `fallback()` handles unmatched call data and can also handle empty data when receive is absent. To accept attached ETH through fallback it must be payable. An ordinary nonpayable entry point rejects value instead of silently becoming a deposit function.

Pull payments separate settlement from a recipient's willingness to execute successfully. Releasing an escrow can record Bob's credit without calling Bob. Bob can later withdraw to an address he controls. If that recipient rejects, Bob's attempt fails while another user's settlement or withdrawal remains available. This is a failure-isolation decision, not merely a different syntax for sending ETH.

## 4.2.3 — Paying another address

```solidity
error PayoutFailed();
(bool paid, ) = recipient.call{value: amount}("");
if (!paid) revert PayoutFailed();
```

A call can invoke recipient code. Low-level `call` reports success as a Boolean, and its return bytes can contain revert data. If you ignore false after clearing the recipient’s credit, you may commit a state change without actually paying. Revert to restore the claim. Update the claim before the call to limit reentrancy, then examine cross-function interactions in Module 06.

## 4.2.4 — Follow one payout through its failure paths

The low-level call fragment belongs inside a function with a defined payable `recipient` and a validated `amount`. `{value: amount}` attaches that many wei. The empty string supplies empty calldata; it does not mean the recipient has no code. A contract recipient may execute its receive or fallback logic.

A low-level call returns `(success, returnData)`. In `(bool paid, )`, the trailing empty position deliberately discards the bytes. When the recipient rejects payment, `paid` is false and the caller chooses what happens next. The EVM does not automatically revert the outer function merely because this Boolean is false.

Suppose a user has 7 wei of credit:

| Stage | Credit | Payment outcome |
|---|---:|---|
| Before withdrawal | 7 | None |
| Effects prepared | 0 | Not attempted yet |
| Recipient accepts | 0 | 7 wei paid |
| Recipient rejects and outer call reverts | 7 after rollback | No payment retained |

Ignoring false would leave the second row committed without a payment. Reverting restores the claim and any aggregate liability changed in the same call. Updating effects first also makes the current claim unavailable during a callback, the security issue examined in Module 06.

## 4.2.5 — Accounting and failure

A rejecting contract should be unable to block unrelated users’ payouts. One approach records a credit and lets each recipient initiate a withdrawal. Separate the contract balance, owed amounts, and the amount requested in a withdrawal. Lab Escrow will credit a party when a deposit settles and let that party direct the withdrawal to a payable recipient.

## Self-review

Why must the caller check low-level call success? Which state must survive a rejecting recipient, and why is the contract's total balance insufficient to reconstruct individual claims?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity receive and fallback](https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function)
