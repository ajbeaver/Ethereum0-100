# Module 09 — Final Assessment

**Scope:** Lessons 9.1–9.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** What helps protect ordinary signed transactions from replay across distinct chain IDs?

A. Contract name  
B. Chain ID in signed data  
C. RPC response ID  
D. Event signature  

**2.** What catches many stale deployments after an Anvil restart with the same chain ID?

A. Amount formatting  
B. Deploy receipt and canonical block check  
C. Wallet label  
D. Base fee cap alone  

**3.** What should be hashed for a manifest check against live code?

A. Actual runtime bytes at the recorded address  
B. The README title  
C. The ABI file alone  
D. The nonce as a string  

**4.** Where does a rollup execute its own user transactions?

A. In its own execution environment  
B. In every user’s wallet only  
C. In SQLite only  
D. At compile time  

**5.** What proves a destination claim executed?

A. The source receipt by itself  
B. The source chain ID  
C. Destination execution evidence  
D. The ABI’s event name  

**6.** What may contribute to total L2 fees in addition to execution?

A. L1 data cost  
B. The sender’s token symbol  
C. The byte length of the wallet label  
D. The RPC request ID  

**7.** Which fact proves a transaction was included?

A. A successful ABI encode  
B. A receipt and its inclusion evidence  
C. A version banner  
D. A simulated return  

**8.** What can a retry with a new nonce do?

A. Create a distinct valid operation  
B. Always replace the old operation  
C. Always return the same hash  
D. Change only the JSON-RPC ID  

**9.** Which step should precede a write?

A. Guessing a contract address  
B. Verifying chain and target  
C. Deleting the deployment record  
D. Treating an absent receipt as success  

**10.** What can help reconcile a timeout without a returned hash?

A. Relevant on-chain state and sender nonce  
B. Token display name alone  
C. Bytecode size alone  
D. Shell prompt colors  

## Part B — Independent practice (50 points)

### Assignment 1 — Network and deployment identity (10 points)

Create `assignment01/` with a deployment manifest for two Counter instances and a verifier. Test correct selection, a swapped address, and an altered block hash. Include tool versions and construction settings.

### Assignment 2 — Layer 2 and bridge semantics (15 points)

Create `assignment02.md` with a synthetic two-chain trace including a source reorg, source reinclusion, and destination completion. Design a status table keyed by both network identities and message ID. State exactly which evidence permits each transition.

### Assignment 3 — Transaction operations (25 points)

In `assignment03/`, create an operational script or command transcript that deploys and checks a Counter. Demonstrate a successful increment, a stale manifest after restart, and a retry of a read operation. Write a decision table for interrupted state-changing requests.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Why does a matching source hash fail to identify one unique deployment?
2. How can two networks both run familiar Solidity code yet require different monitoring and confirmation policies?

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
