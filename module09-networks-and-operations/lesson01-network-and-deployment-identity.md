# Lesson 9.1 — Network and deployment identity

**Course:** Ethereum 0–100, Edition 1  
**Module:** [09 — Networks and Operations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Validate network identity before a write.
2. Explain why chain ID and an address are necessary but sometimes insufficient.
3. Record enough deployment evidence to reproduce a lab.

## 9.1.1 — Separate network, node, and deployment

Your RPC URL selects an endpoint. Its chain ID describes the network domain of its transactions. A deployment identifies a particular creation event and resulting code in the history reported by that endpoint. For example, switching Terminal B to a second Anvil process may leave the same familiar development addresses visible. It can still point at a different Counter or a chain without Counter.

## 9.1.2 — Identify the actual destination

A chain ID helps bind signed transactions to a network. The same address can exist on multiple networks with different balances or code. Even two local nodes can share chain ID 31337 and expose different histories. For the lab, bind a deployment record to chain ID, contract address, deployment transaction, deployment block and hash, and observed runtime-code hash.

```bash
cast chain-id --rpc-url "$RPC_URL"
cast code --rpc-url "$RPC_URL" "$COUNTER"
```

Code presence alone is weak evidence. A different deployment may return the same bytecode. Check the recorded creation receipt and its canonical block alongside the code hash. Validate the manifest before simulating or broadcasting writes.

## 9.1.3 — Reproduce the build

Record the compiler version, optimizer and EVM target, source revision or source hash, and constructor inputs. A compiler artifact’s deployed bytecode can contain placeholders for immutables or linked libraries. For your manifest, hash the actual observed runtime bytes. Comparing them to the build artifact can require accounting for those values.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [EIP-155 replay protection](https://eips.ethereum.org/EIPS/eip-155)
