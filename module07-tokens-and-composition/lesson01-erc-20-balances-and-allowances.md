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

Alice owns 500 units and approves Bob for 80. Approval changes Bob’s spending authority; it does not move units. Bob can then call `transferFrom(Alice, Carol, 30)`. The table below traces the balances and finite allowance, keeping Bob the spender distinct from Carol the recipient.

```bash
CAROL='PASTE_THIRD_ADDRESS_FROM_YOUR_ANVIL_RUN'
cast calldata 'approve(address,uint256)' "$BOB" 80
cast calldata 'transferFrom(address,address,uint256)' "$ALICE" "$CAROL" 30
```

These commands encode two payloads without executing either. The allowance-replacement example below explains why verifying the spender and using narrow permissions matter.

## 7.1.3 — Keep owner, spender, and recipient separate

An allowance is indexed by owner and spender in one token ledger. It does not grant the spender ownership of the units, and it does not designate the eventual recipient. Approval is a transaction from the owner; transferFrom is a later action normally initiated by the spender or its contract.

For Alice's approval to Bob and Bob's transfer to Carol:

| Point | Alice balance | Carol balance change | Bob allowance |
|---|---:|---:|---:|
| Before approval | 500 | 0 | 0 |
| After approval of 80 | 500 | 0 | 80 |
| After spending 30 to Carol | 470 | +30 | 50 |

The calldata example encodes Carol as recipient, but encoding a payload does not set its caller. Replace the Carol placeholder with the third account address from your Anvil run. To reproduce the table, submit approval from Alice and transferFrom from Bob. The spender is determined by the actual call context, not an argument naming Bob.

Native ETH used for transaction fees remains separate from these token balances. Holding token units alone does not establish that an EOA has native ETH to submit its approval transaction on a conventional fee-paying path.

## 7.1.4 — Handle token behavior explicitly

ERC-20 specifies Boolean returns for transfer operations. Some implementations revert on failure, some return false, and some deployed tokens omit a return value. A production integration can use a vetted safe wrapper for return handling. Transfer fees and rebasing also affect balance accounting and require separate support decisions. Lab Escrow uses native ETH, so its principal can be credited from `msg.value`.

## 7.1.5 — Treat units and authority as interface contracts

Replacing an allowance can race with a spender's use of the existing authority. A planned replacement of 80 with 20 is not automatically equivalent to “allow only 20 more total.” Transaction ordering can let the old permission be used before the replacement executes. Narrow approval policies and asset-specific interaction patterns need explicit decisions.

Return handling is another boundary. A successful outer transaction can call a token function that returns false. A safe wrapper can normalize common return conventions, but it does not prove an arbitrary token transfers the expected amount. Fee-taking and rebasing semantics still need an asset support policy. Lab Escrow avoids those additional accounting contracts by accepting native ETH only.

## Self-review

Who calls transferFrom in the three-actor example, and why is the recipient not necessarily the spender? Why does a safe return-value wrapper not establish honest token accounting?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [ERC-20 specification](https://eips.ethereum.org/EIPS/eip-20)
