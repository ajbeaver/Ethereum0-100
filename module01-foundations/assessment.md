# Module 01 — Final Assessment

**Scope:** Lessons 1.1–1.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** Which field records native ETH held by an account?

A. ABI  
B. Balance  
C. Selector  
D. Signature  

**2.** How long is an Ethereum address in bytes?

A. 4  
B. 8  
C. 20  
D. 32  

**3.** Which operation can an observer perform without Alice’s key?

A. Sign as Alice  
B. Export Alice’s key  
C. Approve an Alice transaction  
D. Query Alice’s balance  

**4.** What determines a simple type-2 transfer’s actual execution fee?

A. Gas limit × value  
B. Gas used × effective gas price  
C. Nonce × base fee  
D. Max fee cap × amount  

**5.** What remains true for a transaction included with status zero?

A. All writes persist  
B. The recipient receives attached ETH  
C. The sender pays execution fees  
D. Its nonce can immediately be reused  

**6.** What does the receipt status answer?

A. Whether a key is backed up  
B. Whether a block is finalized  
C. The sender’s balance before execution  
D. Whether the included top-level execution succeeded  

**7.** What uniquely identifies a particular block at a height?

A. Gas used  
B. Block hash  
C. Chain tip height  
D. Base fee  

**8.** A matching block hash at a receipt’s height establishes what?

A. The receipt was finalized  
B. The RPC currently agrees about its inclusion block  
C. The chain cannot reorganize  
D. The transfer used 21,000 gas  

**9.** Which component computes the state resulting from transactions?

A. Wallet UI  
B. Solidity editor  
C. Execution client  
D. Block explorer website  

**10.** Which statement describes Anvil’s usual immediate mining?

A. It proves mainnet finality  
B. It is a convenience of the local simulator  
C. It disables transaction execution  
D. It requires public validators  

## Part B — Independent practice (50 points)

### Assignment 1 — State and accounts (10 points)

Create `account-snapshot.md` for Alice and two accounts you did not use in the exercise. Include chain ID, exact block, code, balance, and how you selected each address. Identify which conclusions follow from the observations alone.

### Assignment 2 — Transactions and gas (15 points)

In `fee-reconciliation.md`, compare transfers of 0.01 and 0.02 ETH between plain Anvil accounts. Verify both equations with observed receipt values. Explain why doubling the value does not by itself double gas usage.

### Assignment 3 — Blocks and finality (25 points)

Create `history-audit.md` for a run of three consecutive transfers. Capture each receipt and block, verify the two parent links between those blocks, then capture a missing receipt from a known prior run after restart. Describe what a cached record from the first run would get wrong.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Why does a ledger need internal deposit records even when its ETH balance is public?
2. What should an application show after submission and again after a receipt arrives? Explain why those states need separate labels.

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
