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

## 4.3.2 — A transition has a precondition and a postcondition

A **precondition** describes when an action is allowed: a record exists, it is active, the caller is the payer, and the deadline rule holds. A **postcondition** describes the resulting state and accounting. A function name such as `release` is not a substitute for either description.

At deadline 1,000, the time cases are:

| Executing timestamp | Release time condition | Refund time condition |
|---|---|---|
| 999 | True | False |
| 1,000 | False | True |
| 1,001 | False | True |

These conditions partition time without overlap or a gap. Authorization and active-state checks still apply independently. At timestamp 1,000, an unrelated address is not entitled to refund merely because the time predicate is true.

A terminal state is an application decision: Released and Refunded have no outgoing settlement transitions. A repeated release must reject even if the payer is still correct and the clock is still before the deadline. Otherwise one principal can create multiple credits.

## 4.3.3 — Timestamp and repeatability

`block.timestamp` is the executing block’s time in Unix seconds. It is a protocol input for broad deadline windows. In tests, control it and examine both sides of the boundary. Avoid treating it as a random seed or as a precision wall clock. A repeat transaction can execute later than its original simulation.

A state machine should explain what a second release attempt does, even if it comes from the correct payer. Mark settlement before recording any resulting credit. Design a distinct event for each outcome, then write tests that prove a rejected transition leaves state and credit intact.

## 4.3.4 — Translate the model into an implementation order

Start a function by validating the current record and actor. Check the applicable time boundary. Then change the record and totals consistently and emit the corresponding event. The model says which effects must become visible together; a revert should leave none of them partially committed.

There is no need to write the entire escrow now. A small timed record can teach the same mechanism with one status and one authorized actor. Later exercises add value and event requirements while preserving the transition model.

The same table becomes a test plan. For each action, include a permitted case, wrong actor, wrong state, and deadline boundary. Check the destination state and the resulting credit, not only whether a call returned. A state label could change correctly while the amount was credited to the wrong person.

A stored deadline also does not schedule a function. Execution sees `block.timestamp` when a transaction actually runs. A refund request can arrive later than intended, and a release simulated before the deadline can execute afterward. The contract evaluates the execution context rather than a promise made by the client earlier.

## Self-review

Why are state, role, and time three separate checks? How does one terminal-state check prevent a valid payer from creating a duplicate credit?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity global variables](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#block-and-transaction-properties)
