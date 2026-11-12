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

## 2.2.2 — A simulation is execution with discarded effects

A contract function can calculate, read storage, and attempt writes during an `eth_call`. The node executes it against a selected state and discards changes afterward. That is why simulation is more general than “calling a getter,” and why a successful simulation cannot be used as evidence that a real balance changed.

The ability to set `from` without a signature follows from this boundary. Asking “what would happen if Alice called?” is different from authorizing “Alice is calling now.” A client must not present a passing simulation as proof that the user controls the specified account.

In the example, `0x1` is one wei. Bob is a plain EOA in the local fixture, so there is no function return value to encode. Empty byte data is a reasonable successful call result. Save Bob's balance before and after in a quiet chain; it should remain equal. The absence of a return value is separate from the absence of an effect in committed state.

## 2.2.3 — Simulate a transfer to an EOA

```bash
cast rpc --rpc-url "$RPC_URL" eth_call \
  "{\"from\":\"$ALICE\",\"to\":\"$BOB\",\"value\":\"0x1\"}" latest
cast balance --rpc-url "$RPC_URL" "$BOB"
```

The successful return for this call is empty byte data. There is no transaction hash, and Bob’s recorded balance is unchanged. Simulating a transaction and estimating its gas are related but distinct requests. `eth_estimateGas` seeks a usable gas amount in the given context. It can fail if the requested execution reverts or the call parameters are invalid.

## 2.2.4 — Why context matters

A release simulated with Alice as caller can pass when the same operation from Bob fails. A refund simulated before the deadline can fail while the same action passes in a later block. Record the sender, value, calldata, target, and block context before comparing simulations.

## 2.2.5 — Context is part of the experiment

A useful comparison holds all but one input fixed:

| Input | Why it matters |
|---|---|
| Target address | Selects the account/code receiving the call |
| Calldata | Selects the requested operation and arguments |
| Sender | Can determine authorization and user-specific storage |
| Attached value | Can satisfy or violate a payment requirement |
| Block context | Supplies the state and execution environment for the simulation |

For example, two calls with identical calldata but different senders can legitimately differ. Two calls from Alice at different blocks can differ because a deposit was settled between them. Recording only the function name is insufficient to reproduce either result.

Gas estimation also runs in a context. A larger gas limit can accommodate more work, but it cannot turn a false role check into a true one. If estimation reports a revert, inspect the function's requirements before treating the result as a resource shortage.

Simulation is valuable as an early check before broadcasting. It reduces avoidable mistakes while leaving a remaining ordering risk: the actual transaction executes after other transactions may have changed state. Receipt inspection and post-state checks are therefore still necessary even after a successful dry run.

## Self-review

Why can a state-changing function succeed under `eth_call` without updating a getter afterward? Which inputs must match before two simulation results can be compared meaningfully?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC, eth_call](https://ethereum.org/developers/docs/apis/json-rpc/#eth_call)
