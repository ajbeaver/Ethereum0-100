# Lesson 4.2 — Payable calls

**Course:** Ethereum 0–100, Edition 1  
**Module:** [04 — Contract Interfaces](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Trace ETH into and out of a contract.
2. Explain the consequences of an external call returning false.
3. Distinguish explicit payable methods from receive and fallback.

## 4.2.1 — Receiving ETH and recognizing deposits

A payable method can require a positive `msg.value` and associate it with an ID or account. ETH can also arrive outside that deposit path, for example by balance transfer via `SELFDESTRUCT`. The receiving interface must therefore distinguish acceptance of assets from creation of a recognized claim.

A contract balance can increase outside the intended payable deposit function. Therefore “balance increased” does not identify a payer, payee, or deposit ID. Only the supported deposit path should create those obligations. This is the reason solvency later uses an inequality that permits surplus.

`receive()` handles empty calldata when available; `fallback()` handles unmatched call data and can also handle empty data when receive is absent. To accept attached ETH through fallback it must be payable. Without an appropriate payable entry point, a plain transfer is normally rejected; it does not silently become a deposit.

Pull payments separate settlement from a recipient's willingness to execute successfully. Releasing an escrow can record Bob's credit without calling Bob. Bob can later withdraw to an address he controls. If that recipient rejects, Bob's attempt fails while another user's settlement or withdrawal remains available. This is a failure-isolation decision, not merely a different syntax for sending ETH.

## 4.2.2 — Low-level payouts and their failure paths

```solidity
error PayoutFailed();
(bool paid, ) = recipient.call{value: amount}("");
if (!paid) revert PayoutFailed();
```

The low-level call fragment belongs inside a function with a defined payable `recipient` and a validated `amount`. `{value: amount}` attaches that many wei. The empty string supplies empty calldata; it does not mean the recipient has no code. A contract recipient may execute its receive or fallback logic.

A low-level call returns `(success, returnData)`; the bytes can contain return values or revert data. In `(bool paid, )`, the trailing empty position deliberately discards those bytes. When the recipient rejects payment, `paid` is false and the caller chooses what happens next. The EVM does not automatically revert the outer function merely because this Boolean is false.

Suppose a user has 7 wei of credit:

| Stage | Credit | Payment outcome |
|---|---:|---|
| Before withdrawal | 7 | None |
| Effects prepared | 0 | Not attempted yet |
| Recipient accepts | 0 | 7 wei paid |
| Recipient rejects and outer call reverts | 7 after rollback | No payment retained |

Ignoring false would leave the second row committed without a payment. Reverting restores the claim and any aggregate liability changed in the same call. Updating effects first also makes the current claim unavailable during a callback, the security issue examined in Module 06.

## 4.2.3 — Accounting and failure

Separate the contract balance, owed amounts, and the amount requested in a withdrawal. Lab Escrow will credit a party when a deposit settles and let that party direct the withdrawal to a payable recipient.

## Self-review

Why must the caller check low-level call success? Which state must survive a rejecting recipient, and why is the contract's total balance insufficient to reconstruct individual claims?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity receive and fallback](https://docs.soliditylang.org/en/latest/contracts.html#receive-ether-function)
