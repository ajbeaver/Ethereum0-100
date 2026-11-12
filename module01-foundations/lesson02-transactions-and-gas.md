# Lesson 1.2 — Transactions and gas

**Course:** Ethereum 0–100, Edition 1  
**Module:** [01 — Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Trace an ETH transfer from request to receipt.
2. Calculate actual execution fees and balance changes in wei.
3. Distinguish gas limit, fee caps, and receipt status.

## 1.2.1 — The transaction lifecycle

A transaction specifies intent such as sender authorization, destination, value, calldata, nonce, and gas terms. A signed transaction can be accepted into a node’s pending pool. When included in a block, it executes against the state left by earlier transactions. Its receipt records the outcome. A submitted hash identifies the request. The receipt tells you whether it executed successfully in a particular block.

For a type-2 transaction, the gas price actually paid is capped by `maxFeePerGas`. It consists of the block base fee and a priority fee, with the tip limited by both the priority cap and the remaining room under the total cap. The base fee is burned on Ethereum L1. The priority fee goes to the block fee recipient. For a simple type-2 ETH transfer, the execution fee is `gasUsed × effectiveGasPrice`.

## 1.2.2 — A transfer you can reconcile

Start with [local setup](../setup.md). Use two fresh Anvil accounts. In a quiet local chain, capture both balances immediately before and after the send:

```bash
cast balance --rpc-url "$RPC_URL" "$ALICE"
cast balance --rpc-url "$RPC_URL" "$BOB"
cast send --rpc-url "$RPC_URL" --from "$ALICE" --unlocked --value 1ether "$BOB"
cast balance --rpc-url "$RPC_URL" "$ALICE"
cast balance --rpc-url "$RPC_URL" "$BOB"
```

A simple transfer to a plain EOA with empty calldata uses 21,000 gas. One ETH is `10^18` wei. Calculate Alice’s loss as value plus actual fee and Bob’s gain as value. If the sender is also the fee recipient or other transactions intervene, account for those flows too.

## 1.2.3 — Failure and ordering

A transaction that is rejected before inclusion has no receipt and incurs no on-chain execution fee. An included transaction whose execution reverts pays for gas used and consumes its nonce. Its reverted contract writes and ETH transfers are rolled back. A transaction’s gas limit bounds consumption. Increasing the limit does not make an authorization check pass.

The nonce of a plain sender orders its transactions. If two competing transactions use the same sender nonce, only one can occupy that position in the same canonical history.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum transactions](https://ethereum.org/developers/docs/transactions/)
