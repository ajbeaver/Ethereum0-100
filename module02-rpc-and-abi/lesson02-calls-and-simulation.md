# Lesson 2.2 — Calls and simulation

**Course:** Ethereum 0–100, Edition 1  
**Module:** [02 — RPC and ABI](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Distinguish a simulated message call from a broadcast transaction.
2. Explain why sender and block context change the result.
3. Collect evidence for an execution failure without assuming it was mined.

## 2.2.1 — Two ways to ask the node to execute

`eth_call` simulates a call against a chosen block context and returns data or a failure. Any simulated writes are discarded. A simulation can run a state-changing function and even include a value or caller. It does not need a signature for that caller because it never authorizes a real state change.

A broadcast transaction needs sender authorization. On local Anvil, `--unlocked` asks the node to use one of its own accounts. A pending transaction can later execute against a different state from the simulation. Another transaction may change a balance, deadline condition, allowance, or contract state first.

## 2.2.2 — Simulate a transfer to an EOA

```bash
cast rpc --rpc-url "$RPC_URL" eth_call \
  "{\"from\":\"$ALICE\",\"to\":\"$BOB\",\"value\":\"0x1\"}" latest
cast balance --rpc-url "$RPC_URL" "$BOB"
```

The successful return for this call is empty byte data. There is no transaction hash, and Bob’s recorded balance is unchanged. Simulating a transaction and estimating its gas are related but distinct requests. `eth_estimateGas` seeks a usable gas amount in the given context. It can fail if the requested execution reverts or the call parameters are invalid.

## 2.2.3 — Why context matters

A release simulated with Alice as caller can pass when the same operation from Bob fails. A refund simulated before the deadline can fail while the same action passes in a later block. Record the sender, value, calldata, target, and block context before comparing simulations.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC, eth_call](https://ethereum.org/developers/docs/apis/json-rpc/#eth_call)
