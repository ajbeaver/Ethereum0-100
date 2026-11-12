# Lesson 7.2 — ERC-721 ownership and callbacks

**Course:** Ethereum 0–100, Edition 1  
**Module:** [07 — Tokens and Composition](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Contrast token IDs with fungible balances.
2. Identify owner, token-specific approval, and operator approval.
3. Explain the contract receiver check in a safe transfer.

## 7.2.1 — Identity rather than quantity

An ERC-721 contract records ownership for unique token IDs. `balanceOf(owner)` counts holdings. `ownerOf(tokenId)` identifies the owner and rejects invalid IDs. `approve` delegates one token, and `setApprovalForAll` delegates management of the owner’s tokens in that contract. A token-specific approval is cleared when ownership changes.

## 7.2.2 — Identify the asset and the authority separately

Token ID 17 is unique within one ERC-721 contract's namespace, not across all networks and contracts. Two contracts can both have a valid token 17 with unrelated owners and meaning. The full asset identity is therefore `(chain, contract, token ID)`.

A fungible balance answers “how many base units does Alice own?” NFT ownership answers “who owns this specific ID?” `balanceOf` counts Alice's NFTs in that ledger, but the count does not identify which ones. General enumeration is not guaranteed merely by the base ownership interface.

A token-specific approval lets an address transfer that token while the approval remains valid. Operator approval is broader: it permits managing the owner's tokens in that contract according to the interface. These grants must be read in the context of the current owner. A stale record of an approval from an earlier ownership period is not permanent authority over the asset.

## 7.2.3 — The receiver is part of execution

A safe transfer to a contract checks that its receiver hook accepts the NFT. The receiver returns the required selector, `0x150b7a02`, from `onERC721Received`. A rejected hook reverts the transfer. The callback can execute code while the transfer is in progress. Design your own calling contract with that callback boundary in mind.

## 7.2.4 — A safe transfer checks an interface, not intent

The receiver check helps avoid sending an NFT to an ordinary contract that cannot participate in that interface.

A receiver returning the correct selector can still contain arbitrary logic. It can attempt calls back into the sender's application during the transfer. “Safe” therefore refers to a specific receiver-acceptance mechanism, not proof that every surrounding state transition is secure.

Suppose an application first transfers an NFT and only afterward clears the owner's claim. The receiver hook can observe that still-active claim while control is external. The same reasoning as an ETH callback applies, even though the asset is an ID rather than an amount. Establish the application's effects before the interaction and ensure a rejected transfer restores the claim through rollback.

An NFT escrow would need to track custody and the distinct right to claim each asset identity. Summing wei liabilities would not express its obligations. This comparison is useful design reasoning, but changing the capstone to NFTs now would introduce a different custody protocol before its native-ETH path is complete.

## Self-review

Why does a correct receiver selector not prove a trustworthy recipient? Which part of an NFT escrow's accounting would replace aggregate amount-based reasoning?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [ERC-721 specification](https://eips.ethereum.org/EIPS/eip-721)
