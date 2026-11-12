# Lesson 4.3 — State machines

**Course:** Ethereum 0–100, Edition 1  
**Module:** [04 — Contract Interfaces](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Specify allowed transitions with guards and observable effects.
2. Distinguish roles from states.
3. Make deadline boundaries mutually exclusive and exhaustive.

## 4.3.1 — Design the transition table first

For Lab Escrow, a payer opens a deposit for a named payee and a future deadline. The payer can release it before that deadline. At or after the deadline, the payer can refund it. Settlement credits one party and ends the deposit’s active state.

| Current state | Action | Guard | Next state | Credit |
|---|---|---|---|---|
| Missing | Open with new ID | Positive ETH and valid parties and deadline | Active | None |
| Active | Release | Payer and timestamp < deadline | Released | Payee |
| Active | Refund | Payer and timestamp >= deadline | Refunded | Payer |

Both terminal states reject further settlement. The strict comparison on release and inclusive comparison on refund give the exact deadline a single outcome. An ID’s state and its payer authorization answer different questions. An active record can still reject the caller.

## 4.3.2 — Timestamp and repeatability

`block.timestamp` is the executing block’s time in Unix seconds. It is a protocol input for broad deadline windows. In tests, control it and examine both sides of the boundary. Avoid treating it as a random seed or as a precision wall clock. A repeat transaction can execute later than its original simulation.

A state machine should explain what a second release attempt does, even if it comes from the correct payer. Mark settlement before recording any resulting credit. Design a distinct event for each outcome, then write tests that prove a rejected transition leaves state and credit intact.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity global variables](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#block-and-transaction-properties)
