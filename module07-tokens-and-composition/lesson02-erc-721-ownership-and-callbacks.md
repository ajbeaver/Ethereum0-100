# Lesson 7.2 — ERC-721 ownership and callbacks

**Course:** Ethereum 0–100, Edition 1  
**Module:** [07 — Tokens and Composition](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Contrast token IDs with fungible balances.
2. Identify owner, token-specific approval, and operator approval.
3. Explain the contract receiver check in a safe transfer.

## 7.2.1 — Identity rather than quantity

An ERC-721 contract records ownership for unique token IDs. The asset is identified by chain, contract address, and token ID. `balanceOf(owner)` counts holdings. `ownerOf(tokenId)` identifies the owner and rejects invalid IDs. `approve` delegates one token, and `setApprovalForAll` delegates management of the owner’s tokens in that contract. A token-specific approval is cleared when ownership changes.

## 7.2.2 — The receiver is part of execution

A safe transfer to a contract checks that its receiver hook accepts the NFT. The receiver returns the required selector, `0x150b7a02`, from `onERC721Received`. A rejected hook reverts the transfer. The callback can execute code while the transfer is in progress. Design your own calling contract with that callback boundary in mind.

Suppose escrow locks NFT #17, then assigns a release claim. Its obligations would refer to asset IDs and owners instead of amounts and aggregate wei. Settlement must account for both the transfer and receiver acceptance. Custody and the right to claim are distinct.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [ERC-721 specification](https://eips.ethereum.org/EIPS/eip-721)
