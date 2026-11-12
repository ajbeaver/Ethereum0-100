# Module 05 — Final Assessment

**Scope:** Lessons 5.1–5.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** When is `setUp()` used in an ordinary Forge test suite?

A. Before each test  
B. After each test  
C. Once per compiler version  
D. Once per chain ID  

**2.** Which cheatcode sets a test caller for one call?

A. `vm.deal`  
B. `vm.prank`  
C. `vm.warp`  
D. `vm.roll`  

**3.** What should accompany a revert expectation for a rejected transition?

A. A production deployment  
B. A changed ABI  
C. An assertion that relevant state stayed intact  
D. A larger gas limit  

**4.** Which command shows detailed traces for focused Forge tests?

A. `cast sig`  
B. `forge test -vvvv --match-test testName`  
C. `cast chain-id`  
D. `forge clean`  

**5.** Which error form can represent checked underflow?

A. Receipt `null`  
B. Event data  
C. `Panic(uint256)`  
D. Sender nonce  

**6.** What establishes the frame returning particular revert data?

A. Selector alone  
B. The call tree  
C. Compiler version alone  
D. Block number alone  

**7.** What does a fuzz counterexample give you?

A. A deployment address  
B. Concrete input that broke the property  
C. A proof all other inputs pass  
D. A new ERC standard  

**8.** Which technique naturally explores order-dependent effects across several actions?

A. ABI encoding  
B. One getter assertion  
C. Invariant testing with action sequences  
D. Compiler formatting  

**9.** Which liability remains after release but before withdrawal?

A. No obligation  
B. The credited amount  
C. Both the same active principal and the same credit  
D. The block base fee  

**10.** Which solvency relationship admits unsolicited ETH?

A. Balance equals zero  
B. Balance must be less than liabilities  
C. Balance >= liabilities  
D. Balance must equal last deposit  

## Part B — Independent practice (50 points)

### Assignment 1 — Unit tests (10 points)

In `assignment01/`, create a two-account credit registry with one operator and three tests. Cover a successful credit, a rejected credit from another actor, and independence of two recipients’ credit records.

### Assignment 2 — Traces and revert data (15 points)

In `assignment02/`, reproduce two distinct failures in your TimedRecord contract, one wrong caller and one wrong time. Save focused tests and compare the selectors and call contexts. If both paths currently share an error, give each a distinct error first.

### Assignment 3 — Fuzzing and invariants (25 points)

In `assignment03/`, test a two-user credit vault with a fuzzed sequence of deposits and partial withdrawals. Keep an expected amount per user in the test, check after each step, and include deterministic cases for zero, full, and over-limit withdrawal.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Why can an overly broad expected-revert assertion hide a regression in authorization?
2. Which evidence would tell you whether a failed withdrawal reached the recipient at all?

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
