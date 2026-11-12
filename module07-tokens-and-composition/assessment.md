# Module 07 — Final Assessment

**Scope:** Lessons 7.1–7.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** Where is an ERC-20 balance recorded?

A. In the account’s native ETH field  
B. In the token contract’s ledger  
C. In the block nonce  
D. In the wallet display only  

**2.** Which actor’s allowance is consumed by transferFrom?

A. The recipient’s allowance over the spender  
B. The caller’s allowance granted by the owner  
C. The validator’s allowance  
D. The owner’s allowance granted by the recipient  

**3.** How many base units represent 2.5 units with six decimals?

A. 25  
B. 2,500  
C. 250,000  
D. 2,500,000  

**4.** What identifies an NFT within an ERC-721 contract?

A. Display symbol  
B. Native balance  
C. Token ID  
D. Block gas limit  

**5.** What happens to token-specific approval on transfer?

A. It clears  
B. It stays with the previous owner’s wallet forever  
C. It converts to operator approval  
D. It becomes a balance  

**6.** Which method can check a contract recipient’s acceptance?

A. `onERC721Received`  
B. `eth_chainId`  
C. `ownerOf`  
D. `gasleft`  

**7.** What can a stable proxy address hide?

A. A changed implementation  
B. A transaction hash with zero bytes  
C. The definition of wei  
D. The chain ID in a signed transaction  

**8.** What can break a vault that credits a requested amount?

A. A matching actual transfer  
B. A fee reducing actual receipt  
C. A read-only balance query  
D. A correct revert  

**9.** What is the capstone’s supported asset?

A. Every ERC-20  
B. ERC-721 only  
C. Native ETH  
D. Any contract with a symbol  

**10.** What should a dependency review record?

A. Only display metadata  
B. Assumptions and failure behavior  
C. Only compiler version  
D. Only number of functions  

## Part B — Independent practice (50 points)

### Assignment 1 — ERC-20 balances and allowances (10 points)

In `assignment01/`, test that two spenders have independent allowances from one owner. Include an approval replacement and two delegated transfers with different recipients.

### Assignment 2 — ERC-721 ownership and callbacks (15 points)

In `assignment02/`, build a Forge receiver fixture that accepts one ID and rejects another. Test a compliant safe transfer against it and prove that rejection leaves ownership unchanged. Record the ERC-721 implementation and revision used.

### Assignment 3 — External dependencies (25 points)

In `assignment03/`, build a dependency test double that returns false for one transfer and reverts on another. Prove your caller does not grant credit after either failure and handles an ordinary successful case. Submit a short support policy.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Why should a deposit request record the token address and chain ID alongside its amount?
2. How would an NFT custody service distinguish one owner holding three NFTs from three units of a fungible token?

## Grading

| Component | Points |
|---|---:|
| Knowledge assessment | 30 |
| Assignment 1 | 10 |
| Assignment 2 | 15 |
| Assignment 3 | 25 |
| Design reasoning | 20 |
| **Total** | **100** |

For each practical assignment, award 60% for satisfying the requirements, 30% for reproducible evidence, and 10% for clarity. For each design answer, award 6 points for a correct model and 4 for applying it to the scenario.

**90–100:** Mastered  
**75–89:** Passed, with targeted review  
**Below 75:** Repeat the relevant lessons and reassess

**Status:** Not submitted
