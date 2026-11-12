# Module 06 — Final Assessment

**Scope:** Lessons 6.1–6.3  
**Total:** 100 points

Complete all three lessons first. Record predictions before running experiments. Keep written work in `assessment-answers.md` and put each assignment's code or evidence in its specified file or folder.

## Part A — Knowledge assessment (30 points)

Choose one answer per question. Each question is worth 3 points.

**1.** When does a reentrant callback execute?

A. In the next block only  
B. Before the original external call returns  
C. After finality  
D. During compilation  

**2.** Which order gives a callback the already-updated credit?

A. Interactions, checks, effects  
B. Effects after interactions  
C. Checks, effects, interactions  
D. Calls before all checks  

**3.** Why fund the vault with an honest deposit in the attack test?

A. To demonstrate an available balance can cover a duplicate payment  
B. To sign as the attacker  
C. To prevent nested calls  
D. To change the ABI  

**4.** What does `tx.origin` identify?

A. The immediate caller  
B. The original transaction sender  
C. The recipient of the current call  
D. The target deployer  

**5.** What can block an all-or-nothing push payout?

A. One reverting recipient  
B. A successful balance query  
C. A receipt event filter  
D. Using sequential IDs  

**6.** Which design limits the effects of a single user’s failed withdrawal?

A. One global loop  
B. One shared credit  
C. Per-user credit and withdrawal  
D. Retrying from a different signer with no checks  

**7.** What happens to total liabilities during successful settlement?

A. They double  
B. They decrease by the full deposit  
C. They stay constant while changing category  
D. They are determined by gas used  

**8.** What happens to credit when payout failure is propagated as a revert?

A. It is restored  
B. It disappears  
C. It transfers to the block proposer  
D. It turns into calldata  

**9.** What does unsolicited ETH normally increase?

A. Per-user credit  
B. Active principal  
C. Contract balance without a matching liability  
D. A previous transaction’s value  

**10.** Which report lets another engineer verify a finding?

A. A severity word alone  
B. Preconditions, calls, expected and observed effects  
C. A screenshot of source only  
D. The compiler’s name  

## Part B — Independent practice (50 points)

### Assignment 1 — Reentrancy (10 points)

In `assignment01/`, test a malicious receiver that attempts withdrawal twice through two different public wrappers. Share the withdrawal logic and demonstrate a single payment and an unchanged second user’s credit.

### Assignment 2 — Authorization and griefing (15 points)

In `assignment02/`, create a local credit fixture with two recipients, one rejecting ETH. Prove the other user can withdraw even after the failed withdrawal and that neither can direct the other’s credit.

### Assignment 3 — Accounting and review (25 points)

In `assignment03/`, review your SharedCredits implementation. Submit a table of liabilities before and after a sequence involving two users and a failing payout, plus tests for correct payee, correct totals, and preservation of credit on failure. Use a deliberately faulty variant to show one test can detect a regression.

## Part C — Design reasoning (20 points)

Answer in your own words. Each question is worth 10 points.

1. Why can a vault appear solvent in happy-path tests and still be exploitable through a callback?
2. How does a role-by-action matrix reveal an authorization hole that line-by-line reading can miss?

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
