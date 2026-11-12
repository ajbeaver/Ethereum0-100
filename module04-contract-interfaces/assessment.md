# Module 04 — Final Assessment

**Scope:** Lessons 4.1–4.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** What identifies an ordinary event signature?

A. A four-byte calldata selector  
B. A 32-byte topic-zero hash  
C. The sender nonce  
D. The gas limit  

**2.** Where is a non-indexed uint256 normally encoded?

A. Receipt status  
B. Event data  
C. Block number  
D. Transaction nonce  

**3.** Can a Solidity contract query its historical receipt logs as storage?

A. Yes, with a getter  
B. Yes, with a mapping  
C. Yes, within one block  
D. No  

**4.** What is an ordinary payable call’s attached ETH visible as?

A. `tx.origin`  
B. `msg.value`  
C. `block.number`  
D. `gasleft()`  

**5.** How does a failed low-level `call` report failure to its caller?

A. Boolean false  
B. A guaranteed caller revert  
C. A nonce reset  
D. A new receipt  

**6.** Why separate obligations from address balance?

A. Account balances are private  
B. Obligations can identify payees and forced ETH can create surplus  
C. All ETH immediately burns  
D. Balance always equals the most recent deposit  

**7.** Which combination yields a unique boundary outcome?

A. Release < deadline and refund >= deadline  
B. Release <= deadline and refund >= deadline  
C. Release > deadline and refund < deadline  
D. Release at all times and refund at all times  

**8.** What should a released record do on a second release?

A. Credit again  
B. Change its payer  
C. Reject  
D. Reset to Missing  

**9.** Which variable supplies execution block time?

A. `msg.value`  
B. `block.timestamp`  
C. `tx.origin`  
D. `block.chainid`  

**10.** What does the Active state alone authorize?

A. Any caller to settle  
B. The caller to skip deadline checks  
C. No actor without the additional guards  
D. The RPC endpoint to change payer  

## Part B — Independent practice (50 points)

### Assignment 1 — Events and receipts (10 points)

In `assignment01/`, build a `StatusFeed` emitting two event types for the same ID. Produce both receipts and a raw `eth_getLogs` query that selects only one event signature.

### Assignment 2 — Payable calls (15 points)

In `assignment02/`, implement a `SharedCredits` variant supporting a positive partial withdrawal. Demonstrate an exact withdrawal, a partial withdrawal, and an attempted overdraft against two independent credits. Account for gas when a sender’s own balance is used as evidence.

### Assignment 3 — State machines (25 points)

In `assignment03/`, implement a nonpayable `TimedApproval` with a requester, a distinct reviewer, and a deadline. Only the reviewer can approve before the deadline and only the requester can expire at or after it. Use distinct events and prove terminal transitions cannot repeat.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. How would you design an event so an indexer can answer which participant created a deposit?
2. Describe how one rejecting recipient could affect a batch push payout and how per-user withdrawal changes that failure.

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
