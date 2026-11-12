# Lesson 7.1 — ERC-20 balances and allowances

**Course:** Ethereum 0–100, Edition 1  
**Module:** [07 — Tokens and Composition](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Distinguish native ETH from a token’s internal ledger.
2. Explain transfer, approve, and transferFrom using three actors.
3. Read token balances in base units and handle return values.

## 7.1.1 — A ledger behind a contract address

An ERC-20 balance is returned by the token contract’s `balanceOf(address)`. It is separate from the address’s native ETH balance. The token address and network identify the ledger. Symbols and names are display metadata. If a token reports six decimals, 1.25 display units equals 1,250,000 base units. Keep the amount as an integer until presentation.

## 7.1.2 — The three actors

Alice owns 500 units. She approves Bob for 80. The approval changes Bob’s spending authority over Alice’s balance. It does not move the units. Bob then calls `transferFrom(Alice, Carol, 30)`. In a conventional implementation with finite allowance, Alice has 470 units, Carol gains 30, and the remaining allowance is 50. Bob is the spender and Carol is the recipient.

```bash
cast calldata 'approve(address,uint256)' "$BOB" 80
cast calldata 'transferFrom(address,address,uint256)' "$ALICE" "$BOB" 30
```

These commands encode two payloads without executing either. A new nonzero approval can replace an existing allowance, creating an ordering risk if the spender uses the old allowance before replacement. Use narrow permissions and verify the correct spender address.

## 7.1.3 — Handle token behavior explicitly

ERC-20 specifies Boolean returns for transfer operations. Some implementations revert on failure, some return false, and some deployed tokens omit a return value. A production integration can use a vetted safe wrapper for return handling. Transfer fees and rebasing also affect balance accounting and require separate support decisions. Lab Escrow uses native ETH, so its principal can be credited from `msg.value`.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [ERC-20 specification](https://eips.ethereum.org/EIPS/eip-20)
