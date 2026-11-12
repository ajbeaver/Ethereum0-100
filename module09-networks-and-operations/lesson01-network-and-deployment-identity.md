# Lesson 9.1 — Network and deployment identity

**Course:** Ethereum 0–100, Edition 1  
**Module:** [09 — Networks and Operations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Validate network identity before a write.
2. Explain why chain ID and an address are necessary but sometimes insufficient.
3. Record enough deployment evidence to reproduce a lab.

## 9.1.1 — Separate network, node, and deployment

The RPC URL selects an endpoint, the chain ID identifies a transaction domain, and a deployment is tied to a creation event and resulting code in that endpoint’s history. For example, switching Terminal B to a second Anvil process may leave the same familiar development addresses visible. It can still point at a different Counter or a chain without Counter.

## 9.1.2 — Identify the actual destination

A chain ID helps bind signed transactions to a network. The same address can exist on multiple networks with different balances or code. Even two local nodes can share chain ID 31337 and expose different histories. For the lab, bind a deployment record to chain ID, contract address, deployment transaction, deployment block and hash, and observed runtime-code hash.

```bash
cast chain-id --rpc-url "$RPC_URL"
cast code --rpc-url "$RPC_URL" "$COUNTER"
```

Code presence alone is weak evidence. A different deployment may return the same bytecode. Check the recorded creation receipt and its canonical block alongside the code hash. Validate the manifest before simulating or broadcasting writes.

## 9.1.3 — Identity consists of several independent checks

No single identifier establishes all the others. The following cases explain why the destination checks above must be used together.

Two fresh Anvil processes can deliberately reuse chain ID 31337 and deterministic accounts. A familiar address or matching chain ID therefore does not prove that you selected the same lab run. A deployment made by the same sender at the same nonce may even reproduce an address while differing in constructor inputs or surrounding history.

| Check | Mismatch it can expose |
|---|---|
| Chain ID | A clearly different configured network |
| Code at address | Missing deployment or unexpected executable bytes |
| Creation receipt and block hash | Deployment absent from the currently reported history |
| Constructor/build record | Wrong initialization or source configuration |

These checks establish consistency with recorded expectations through the endpoint you use. They do not make a malicious endpoint truthful. For the course's loopback node, that trust boundary is explicit and controlled.

## 9.1.4 — Reproduce the build

Record the compiler version, optimizer and EVM target, source revision or source hash, and constructor inputs. A compiler artifact’s deployed bytecode can contain placeholders for immutables or linked libraries. For your manifest, hash the actual observed runtime bytes. Comparing them to the build artifact can require accounting for those values.

## 9.1.5 — A manifest connects build evidence to runtime evidence

A **deployment manifest** is a structured record, not a set of shell variables left over in one terminal. It should let a new process know which network, address, creation event, and code hash it is supposed to use. Keep the fields machine-readable and validate them before a state-changing command.

Hash actual runtime bytes with Ethereum Keccak when recording the runtime-code hash. If a contract includes constructor-set immutable values or linked library addresses, the observed code can contain deployment-specific bytes. A naive comparison against an unresolved build template would report a mismatch for a legitimate deployment.

The manifest is evidence of intended identity, not signing authority. Do not store a private key in it just because a local tool also needs a signer. Anvil's unlocked accounts keep this lab's authorization path explicit without mixing secrets into public deployment records.

Lab Escrow's indexer and write tooling should agree on the same manifest. Otherwise a correct index could describe deployment A while an operator mistakenly writes to deployment B. Detecting that mismatch before the write is more useful than explaining it from inconsistent receipts afterward.

## Self-review

Why can two nodes share both chain ID and a familiar address while still representing different lab runs? Which manifest fields distinguish build configuration from observed deployment identity?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [EIP-155 replay protection](https://eips.ethereum.org/EIPS/eip-155)
