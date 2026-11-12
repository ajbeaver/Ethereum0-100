# Module 10 — Final Assessment

**Scope:** Lessons 10.1–10.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** Who can release a deposit in this specification?

A. Any caller  
B. Payee  
C. Payer before the deadline  
D. Payer only after the deadline  

**2.** When can a refund first succeed?

A. At the deadline  
B. One second before the deadline  
C. Only after a withdrawal  
D. At open  

**3.** Who selects the withdrawal recipient?

A. The deployer  
B. The credit owner  
C. The block proposer  
D. The original payee of any deposit  

**4.** What information distinguishes logs from different histories at the same height?

A. Block hash  
B. Decimal precision  
C. Sender balance only  
D. Compiler path  

**5.** What should a duplicate withdrawal delivery do?

A. Subtract credit again  
B. Advance the deposit ID  
C. Leave projection unchanged  
D. Delete the database  

**6.** Which evidence lets you compare the projection and getters fairly?

A. Same checked block  
B. Only the latest state from every request  
C. Same contract filename  
D. Same wall-clock second  

**7.** What preserves a claim after a recipient rejects ETH?

A. Committing the credit deletion  
B. Ignoring the Boolean  
C. Reverting the withdrawal  
D. Retrying from a different owner  

**8.** What catches a wrong payee with correct total credit?

A. Aggregate balance alone  
B. Per-user credit assertions  
C. Compiler warnings alone  
D. A later block height  

**9.** What is expected after an unsolicited ETH transfer?

A. An automatic payer credit  
B. A new active deposit  
C. A higher surplus with unchanged liabilities  
D. A lower contract balance  

**10.** Which proves the demo is reproducible?

A. A screenshot with no commands  
B. A clean start with recorded commands and checked results  
C. An address from an older Anvil run  
D. A successful build alone  

## Part B — Independent practice (50 points)

### Assignment 1 — Escrow contract specification (10 points)

Submit `src/LabEscrow.sol`, `foundry.toml`, and a focused test for every required transition. Cover the exact deadline and prove repeated settlement cannot duplicate credit. Include the canonical signatures and indexed field order.

### Assignment 2 — Indexer and reconciliation (15 points)

Submit the indexer, tests, and sample manifest generated from a local run. Demonstrate two identical imports, a replaced suffix following an Anvil snapshot, and a failed check after a full reset. Include at least one automated comparison against a deterministic expected projection.

### Assignment 3 — Final verification and handoff (25 points)

Submit the complete project and run its clean-room instructions. Include named tests for wrong callers, exact boundaries, failed payout, callback, solvency, and duplicate or replaced logs. In `demo.md`, show receipts and pinned state for both settlement paths, a full replay with unchanged results, and a stale-deployment failure after reset. Add a compact table mapping each requirement to its test or demo evidence.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Explain how pull payments isolate a rejecting recipient from both settlement and withdrawals by other users.
2. Why do journal, projection, and checkpoint need a single commit boundary?

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
