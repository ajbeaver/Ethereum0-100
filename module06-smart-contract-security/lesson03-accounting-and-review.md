# Lesson 6.3 — Accounting and review

**Course:** Ethereum 0–100, Edition 1  
**Module:** [06 — Smart Contract Security](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Separate asset balance from the liabilities owed to users.
2. Check solvency across successful and reverted transitions.
3. Write a finding with a reproducible symptom and a regression test.

## 6.3.1 — Follow the value

Let A be active principal, C unpaid credits, and B the vault’s ETH balance. Opening with value v increases A and B by v. Settlement moves v from A to C. Withdrawal reduces C and B by v. A reverted action leaves these quantities unchanged. Track per-user amounts as well as totals because an accounting identity can hold while the wrong user receives a credit.

Suppose B = 9, A = 5, and C = 3, in the same integer unit. The surplus is 1. Releasing a deposit of 2 leaves B at 9, changes A to 3, and raises C to 5. Withdrawing 2 leaves B at 7 and C at 3. The surplus stays 1. Receiving unsolicited ETH can increase it. Surplus alone establishes no user claim.

## 6.3.2 — Make the ledger explain every state change

A balance is an asset; a credit is an obligation. Active escrow principal is also an obligation, even though its eventual recipient is not settled yet. Counting the same amount in both active principal and unpaid credit after settlement would double-count liabilities. Removing it from both would erase an obligation.

| Action | Change in B | Change in A | Change in C |
|---|---:|---:|---:|
| Open with v | +v | +v | 0 |
| Release or refund v | 0 | -v | +v |
| Withdraw v | -v | 0 | -v |
| Reverted action | 0 | 0 | 0 |
| Unsolicited ETH v | +v | 0 | 0 |

Each row is a relationship you can test around a call. The table does not decide the recipient: record-level and user-level assertions establish that separately. It also explains why rejecting a transfer must preserve credit. Clearing credit without a completed payment would reduce C without reducing B, creating apparent surplus by destroying a user's claim.

An invariant of `B >= A + C` detects undercollateralization. Surplus `B - A - C` should remain unchanged by ordinary supported actions. Unexpected surplus growth can therefore be a symptom even when the basic inequality still passes.

## 6.3.3 — From finding to regression

Write the preconditions, exact calls, expected result, observed result, and impact. A report that says “accounting is broken” omits the evidence needed to reproduce it. Include the smallest path that breaks the rule. Pair the fix with a test that fails on the faulty version and passes on the corrected version.

## 6.3.4 — Write a review finding someone can rerun

A useful finding has a concrete trigger and consequence. For example: “After crediting Bob 7, a rejecting recipient causes the function to return successfully with Bob's credit at zero and no payment.” This identifies the precondition, action, observed state, and broken promise. The general phrase “missing return check” is an implementation clue, but the observable lost claim explains impact.

Retain the initial ledger, exact caller, recipient behavior, and transaction/test result. Show the expected ledger alongside the actual one. Then construct a regression that fails on the old behavior and checks both preserved credit and payment outcome after the fix.

Prioritize findings by what an actor can cause under reachable conditions. An unusual private helper name is not a security defect by itself. A one-line mistake that destroys another user's entitlement is. The purpose of review is to connect code paths to violated contracts, not merely collect suspicious syntax.

## Self-review

Why can an increase in surplus indicate a lost claim rather than a benefit? Which assertions are needed in addition to aggregate solvency to catch a payment credited to the wrong owner?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity security considerations](https://docs.soliditylang.org/en/latest/security-considerations.html)
