# Module 08 — Final Assessment

**Scope:** Lessons 8.1–8.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** Which two criteria identify the intended event stream?

A. Sender and gas price  
B. Contract address and event signature topic  
C. Chain height and receipt status alone  
D. Symbol and ABI filename  

**2.** How should `0x2` and `0x10` be ordered as positions?

A. `0x2` first  
B. `0x10` first  
C. They tie  
D. They cannot be compared  

**3.** Why chunk historical queries?

A. To force mining  
B. To limit response size and enable retry  
C. To change topics  
D. To skip block verification  

**4.** What prevents the same log identity from creating duplicate rows?

A. HTTP status  
B. Unique constraint  
C. A different ABI  
D. String sorting  

**5.** Which type can preserve decimal uint256 values without a 64-bit overflow?

A. SQLite INTEGER only  
B. Float  
C. Canonical decimal TEXT  
D. One-byte binary  

**6.** When should a checkpoint commit?

A. In the same transaction as its journal changes  
B. Before network fetch  
C. Before log inserts  
D. Only when a log exists  

**7.** What should the indexer compare at a checkpoint?

A. Block hashes  
B. Display labels  
C. Pending pool lengths  
D. Gas limits alone  

**8.** What must happen to a projection built from orphaned logs?

A. It must be kept as current  
B. It must be reconciled or rebuilt  
C. It must change contract addresses automatically  
D. It must increment the nonce  

**9.** How should a missing deployment after reset be handled?

A. Keep indexing the old address silently  
B. Assume all state equals zero  
C. Require a valid deployment record  
D. Treat all old events as final  

**10.** Which condition justifies processing an empty range as complete?

A. The RPC query and block evidence succeeded  
B. A timeout occurred  
C. The chain ID matched once  
D. A new deployment file was found without checks  

## Part B — Independent practice (50 points)

### Assignment 1 — Reading and decoding logs (10 points)

Write `assignment01.py` to consume a saved JSON log fixture containing two emitters and scrambled order. Filter and decode only one ReadingFeed stream. Include tests for malformed data and an unrelated signature.

### Assignment 2 — Persistence and replay (15 points)

Build `assignment02.py` to import two ranges, one empty. Inject an exception before the database commit, rerun, and demonstrate that journal and checkpoint recover together. Rebuild a projection after the second run.

### Assignment 3 — Reorg recovery (25 points)

Create a regression fixture in `assignment03/` with a common prefix, two competing suffixes, and a duplicated log delivery. Prove the rebuilt journal and projection match only the current canonical branch.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Why separate RPC fetching from deterministic decoding?
2. Why keep raw event evidence after building a convenient status table?

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
