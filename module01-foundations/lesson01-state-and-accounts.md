# Lesson 1.1 — State and accounts

**Course:** Ethereum 0–100, Edition 1  
**Module:** [01 — Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Explain what an Ethereum state transition changes.
2. Distinguish an account address from a private key and a wallet.
3. Inspect an account at a specific point in the chain.

## 1.1.1 — State before and after execution

Think of a block as a batch of transactions executed in a definite order. Starting with the parent block’s resulting state, execution applies each transaction in sequence. An ETH transfer changes the sender’s and recipient’s balances. It also consumes a sender nonce and charges an execution fee. A smart contract call can additionally change persistent storage.

An account can have a balance, nonce, code, and storage. A plain externally owned account (EOA) is controlled by a signing key. A conventional contract account executes deployed code when called. Some EOAs can delegate execution under EIP-7702. Here we use plain EOAs and ordinary Solidity deployments so each lab has a clear starting model.

## 1.1.2 — Address, wallet, and signer

An address is a 20-byte identifier, commonly displayed as 40 hexadecimal digits prefixed by `0x`. A wallet helps manage signing credentials and construct requests. The private key authorizes signatures. Anyone with RPC access can inspect a public address, even if they cannot sign as that account.

When Anvil starts, it supplies funded accounts for development. Compare a funded account and an unused address after completing [local setup](../setup.md):

```bash
cast balance --rpc-url "$RPC_URL" "$ALICE"
cast code --rpc-url "$RPC_URL" "$ALICE"
cast balance --rpc-url "$RPC_URL" 0x000000000000000000000000000000000000bEEF
```

On a fresh course chain, the first balance is positive, Alice’s code is `0x`, and the unused balance is zero. Empty code alone cannot distinguish a plain EOA from an unused address. Record the chain ID and block height with measurements. Two networks can use the same address string with different histories and balances.

## 1.1.3 — Lab Escrow connection

Lab Escrow will have a contract address and will hold ETH at that address. Its storage will track who has a claim on each deposit. The account balance provides a total for the address. It cannot identify the rightful recipient of any individual deposit.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum accounts](https://ethereum.org/developers/docs/accounts/)
