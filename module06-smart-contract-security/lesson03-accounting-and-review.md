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

## 6.3.2 — From finding to regression

Write the preconditions, exact calls, expected result, observed result, and impact. A report that says “accounting is broken” omits the evidence needed to reproduce it. Include the smallest path that breaks the rule. Pair the fix with a test that fails on the faulty version and passes on the corrected version.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity security considerations](https://docs.soliditylang.org/en/latest/security-considerations.html)
