# Module 02 — Final Assessment

**Scope:** Lessons 2.1–2.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** Which field correlates a JSON-RPC response to its request?

A. `method`  
B. `id`  
C. `params`  
D. `gas`  

**2.** Which is empty RPC byte data?

A. `"0x0"`  
B. `"0x"`  
C. `"0x00"`  
D. `null`  

**3.** Which case can be a successful RPC response?

A. TCP connection refused  
B. Invalid JSON body  
C. A receipt lookup returning `null`  
D. A response with only an `error` object  

**4.** What does `eth_call` persist?

A. All writes  
B. ETH transfers only  
C. Logs only  
D. No chain state changes  

**5.** What can change between a successful preflight and inclusion?

A. The target contract’s storage  
B. The number of bytes in an address  
C. The transaction’s signed calldata  
D. The integer definition of wei  

**6.** What happens to a sender nonce after a simulation?

A. It increments  
B. It decrements  
C. It stays unchanged  
D. It becomes the block number  

**7.** What determines the ordinary function selector?

A. Keccak-256 prefix of the canonical signature  
B. Sender nonce  
C. Hash of the first argument  
D. ABI return type  

**8.** How many bytes encode one static uint256 argument?

A. 4  
B. 20  
C. 32  
D. 256  

**9.** What does the head word for a top-level string argument contain?

A. Its first 32 characters in all cases  
B. The target address  
C. An offset into the argument encoding  
D. The transaction hash  

**10.** Which field selects the contract that receives calldata?

A. `to`  
B. `nonce`  
C. `gasUsed`  
D. `effectiveGasPrice`  

## Part B — Independent practice (50 points)

### Assignment 1 — JSON-RPC requests (10 points)

Save `pinned-reads.md` containing Alice’s and Bob’s balances queried before and after a transfer at two pinned heights. Requery the earlier height and explain why the earlier balances remain useful.

### Assignment 2 — Calls and simulation (15 points)

Write `call-audit.md` comparing two simulated transfers with different values from Bob to Alice and one real transfer. Capture enough starting and ending state to independently verify which one changed balances.

### Assignment 3 — ABI and calldata (25 points)

Produce `abi-audit.md` with encodings for `grant(address,uint256)` and `grant(address,bytes)` using Alice’s address. Pick integer 4096 and bytes `0x1234`. Annotate each word and account for total length and padding.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. How would you prove two separately queried balances refer to the same observed chain state?
2. Why would a service require a real receipt before marking an escrow as funded after simulation?

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
