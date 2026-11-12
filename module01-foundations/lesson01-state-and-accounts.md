# Lesson 1.1 — State and accounts

**Course:** Ethereum 0–100, Edition 1  
**Module:** [01 — Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Explain what an Ethereum state transition changes.
2. Distinguish an account address from a private key and a wallet.
3. Inspect an account at a specific point in the chain.

## 1.1.1 — Build a model of state, history, and execution

Ethereum is a network for agreeing on an ordered history of transactions and the state they produce. A **transaction** is an authorized request for an operation, such as transferring ETH or invoking a contract. ETH is the network's native asset; its smallest unit is wei, with `10^18` wei in one ETH. A **node** is software that exposes and participates in a chain view. The course uses a local node so you can inspect the mechanics directly.

**State** is the information currently used to evaluate the next transaction. **History** records how the chain arrived there. If you inspect Alice's balance, you learn one value at the selected block. You have not yet learned every transfer or fee that produced it. Several different histories can lead to the same final balance.

A smart contract is a program whose deployed code and persistent data are associated with an account. The Ethereum Virtual Machine (EVM) executes that code. Ordinary contract execution happens when a transaction or another call reaches it; deploying a contract does not start an independent background process. A deadline in storage will not trigger itself when the clock passes it. Someone must submit a transaction that checks the deadline and changes state.

The word **decentralized** describes how a network's participants validate and agree on history. It does not remove the need to specify what a program does. In these labs Anvil is one local node providing an execution environment, so you can inspect the mechanics before studying distributed agreement.

## 1.1.2 — State before and after execution

Think of a block as a batch of transactions executed in a definite order. Starting with the parent block’s resulting state, execution applies each transaction in sequence. An ETH transfer changes the sender’s and recipient’s balances. It also consumes a sender nonce and charges an execution fee. A smart contract call can additionally change persistent storage.

An account can have a balance, nonce, code, and storage. In this course, an externally owned account (EOA) is a plain key-controlled account used to authorize transactions. A conventional contract account has deployed code that executes when called. More advanced account designs exist, but the local labs use this deliberately small account model.

## 1.1.3 — Address, wallet, and signer

An address is a 20-byte identifier, commonly displayed as 40 hexadecimal digits prefixed by `0x`. A wallet helps manage signing credentials and construct requests. The private key authorizes signatures. Anyone with RPC access can inspect a public address, even if they cannot sign as that account.

When Anvil starts, it supplies funded accounts for development. Compare a funded account and an unused address after completing [local setup](../setup.md):

```bash
cast balance --rpc-url "$RPC_URL" "$ALICE"
cast code --rpc-url "$RPC_URL" "$ALICE"
cast balance --rpc-url "$RPC_URL" 0x000000000000000000000000000000000000bEEF
```

On a fresh course chain, the first balance is positive, Alice’s code is `0x`, and the unused balance is zero. Empty code alone cannot distinguish a plain EOA from an unused address. Record the chain ID and block height with measurements. Two networks can use the same address string with different histories and balances.

## 1.1.4 — Interpret an account observation

The three Cast commands ask the node questions; they do not send an ETH transfer. `--rpc-url` chooses the endpoint, and the final address identifies the account being inspected. `cast code` returns deployed runtime bytecode as hexadecimal data. `0x` means no bytes were returned, not the integer balance zero.

| Observation | What it establishes | What it leaves unknown |
|---|---|---|
| Positive balance | The address holds native ETH at the queried state | Who controls it or why it was funded |
| Empty code | There is no ordinary runtime bytecode there at that state | Whether someone owns a corresponding private key |
| An address string | A possible account identifier | Its balance or role on a different chain |

A private key is secret signing material; it is not something you query from the public account. The wallet is software that helps use that authority. The address can be shared to receive funds without sharing signing authority. Anvil deliberately supplies known local keys for testing, which is a development convenience rather than the normal custody model.

For Lab Escrow, an account balance answers “how much ETH is here?” Storage answers “which obligations does the program recognize?” Keep those questions separate from the first lesson onward.

## 1.1.5 — Lab Escrow connection

Lab Escrow will have a contract address and will hold ETH at that address. Its storage will track who has a claim on each deposit. The account balance provides a total for the address. It cannot identify the rightful recipient of any individual deposit.

## Self-review

Explain why a contract does not refund itself merely because its stored deadline passed. What would you need beyond a balance read to identify a deposit's rightful recipient?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum accounts](https://ethereum.org/developers/docs/accounts/)
