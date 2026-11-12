# Module 03 — Final Assessment

**Scope:** Lessons 3.1–3.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** When does a constructor execute?

A. On every getter call  
B. Once during creation  
C. On first ordinary transfer  
D. When the ABI is downloaded  

**2.** What does successful initialization install?

A. Solidity source  
B. ABI JSON  
C. Runtime code  
D. A new RPC server  

**3.** Where does a public state variable’s value persist?

A. Chain storage for that address  
B. The client’s terminal  
C. The compiler log  
D. The JSON-RPC request ID  

**4.** What does a mapping return for an unwritten uint256 key with uint256 values?

A. A revert  
B. Zero  
C. A generated ID  
D. A random storage word  

**5.** Which local reference can update a mapping struct in place?

A. A `storage` reference  
B. A copied `memory` struct  
C. ABI return bytes  
D. A calldata byte offset  

**6.** What can enumerate a standard Solidity mapping by itself?

A. Its public getter  
B. Its key type  
C. Its length property  
D. No built-in iterator  

**7.** Who does the callee see as `msg.sender` during an ordinary contract-to-contract call?

A. The original wallet  
B. The caller contract  
C. The block proposer  
D. The RPC operator  

**8.** What unit does `msg.value` use?

A. ETH  
B. gwei  
C. wei  
D. gas  

**9.** What happens to writes in a reverted top-level call?

A. They roll back  
B. They persist as pending writes  
C. They convert to logs  
D. Only the first write persists  

**10.** Which function declaration can receive positive call value?

A. `external payable`  
B. `external view`  
C. `external pure`  
D. `external` with no payable modifier  

## Part B — Independent practice (50 points)

### Assignment 1 — Contracts and deployment (10 points)

In `assignment01/`, build a `StepCounter` with constructor arguments for starting count and a fixed step stored in state. Deploy two instances using different steps and demonstrate divergent results after two increments each.

### Assignment 2 — Types and storage (15 points)

In `assignment02/`, create a `TicketBook` with explicit Missing, Open, and Closed states. Expose creation, closing, and lookup. Demonstrate missing versus closed, and show that changing one ticket leaves a second untouched.

### Assignment 3 — Callers and errors (25 points)

In `assignment03/`, implement a `TipJar` that records positive ETH contributions per immediate caller. Reject zero with a custom error. Demonstrate two contributions from one account, one from another, and an unchanged total after a zero-value failure. Keep the jar local and add no withdrawal method for this assessment.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Explain how one contract artifact can correspond to multiple addresses with different state.
2. Why is a default enum value a useful part of a record schema?

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
