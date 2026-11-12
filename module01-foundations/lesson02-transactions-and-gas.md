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

## 1.2.2 — Separate amount, work, and price

ETH value is the amount you intend to transfer. **Gas** measures execution resources. **Gas price** converts the gas charged into wei. These are different units. A gas limit of 50,000 does not mean you send 50,000 wei or pay for every unit in that limit. The receipt's `gasUsed` records the amount charged for the executed transaction.

For a fee example, suppose a simple transfer uses 21,000 gas at an effective price of 2 gwei per gas. One gwei is 1,000,000,000 wei, so its execution fee is:

```text
21,000 × 2,000,000,000 = 42,000,000,000,000 wei
```

Sending 1 ETH would reduce Alice's balance by `1,000,042,000,000,000,000` wei under the quiet-chain assumptions above. Bob receives exactly `1,000,000,000,000,000,000` wei. The difference is a fee, not an unexplained lost transfer. Keep calculations as integers; floating-point rounding is unnecessary when the unit is already indivisible.

For type-2 pricing, a useful expression is `min(maxFeePerGas, baseFeePerGas + maxPriorityFeePerGas)`, provided the transaction is eligible for inclusion at that block's base fee. The caps describe willingness to pay, while the receipt describes what was actually charged. A fee cap below the base fee can keep a transaction pending rather than make execution cheaper.

## 1.2.3 — A transfer you can reconcile

Start with [local setup](../setup.md). Use two fresh Anvil accounts. In a quiet local chain, capture both balances immediately before and after the send:

```bash
cast balance --rpc-url "$RPC_URL" "$ALICE"
cast balance --rpc-url "$RPC_URL" "$BOB"
cast send --rpc-url "$RPC_URL" --from "$ALICE" --unlocked --value 1ether "$BOB"
cast balance --rpc-url "$RPC_URL" "$ALICE"
cast balance --rpc-url "$RPC_URL" "$BOB"
```

A simple transfer to a plain EOA with empty calldata uses 21,000 gas. One ETH is `10^18` wei. Calculate Alice’s loss as value plus actual fee and Bob’s gain as value. If the sender is also the fee recipient or other transactions intervene, account for those flows too.

## 1.2.4 — Read the receipt as an execution record

The send command may wait for a receipt and print its fields. Save the transaction hash, then inspect it with `cast receipt --rpc-url "$RPC_URL" "$TX"`, setting `TX` to your run's hash. Look for status, gas used, effective gas price, and block identifiers. A receipt status of success establishes EVM execution success; later lessons explain why a program can still perform the wrong business operation.

The **nonce** is a sequence number for the sender's transactions. It helps distinguish a second operation from a replacement attempt. If execution reverts after inclusion, that position is still consumed: the chain processed the attempt even though its internal state changes did not survive.

Compare this with a rejected request before inclusion. There is no mined execution to pay for, and no receipt proving one occurred. A terminal error message alone cannot tell you which stage failed. The practical habit is to ask: was the request accepted, was it included, and what did its receipt establish?

## 1.2.5 — Failure and ordering

A transaction that is rejected before inclusion has no receipt and incurs no on-chain execution fee. An included transaction whose execution reverts pays for gas used and consumes its nonce. Its reverted contract writes and ETH transfers are rolled back. Increasing a gas limit does not make an authorization check pass: a failed condition is different from insufficient execution resources.

The nonce of a plain sender orders its transactions. If two competing transactions use the same sender nonce, only one can occupy that position in the same canonical history.

## Self-review

Why can Alice lose ETH while Bob receives none after an included reverted transaction? Which receipt fields let you calculate the actual execution fee rather than a maximum possible fee?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum transactions](https://ethereum.org/developers/docs/transactions/)
