# Module 06 — Smart Contract Security

**Course:** Ethereum 0–100, Edition 1  
**Prerequisite:** [Testing and Debugging](../module05-testing-and-debugging/README.md)

## Purpose

Study how contract interactions, authority mistakes, and accounting bugs turn expected behavior into exploitable behavior. Reproduce failures with local fixtures.

## Learning objectives

1. Explain how a recipient can call back before the original invocation returns.
2. Identify the actor authorized to change each state.
3. Separate asset balance from the liabilities owed to users.

## Chapters

| Lesson | Textbook | Exercise specification |
|---|---|---|
| 6.1 | [Reentrancy](lesson01-reentrancy.md) | [Exercise](exercises/lesson01/lesson01.md) |
| 6.2 | [Authorization and griefing](lesson02-authorization-and-griefing.md) | [Exercise](exercises/lesson02/lesson02.md) |
| 6.3 | [Accounting and review](lesson03-accounting-and-review.md) | [Exercise](exercises/lesson03/lesson03.md) |

## Workflow

1. Read the textbook chapter.
2. Predict before you run the example.
3. Complete the exercise from its written requirements and save your evidence.
4. Review your answers and repeat any experiment you cannot explain.
5. Submit the [100-point assessment](assessment.md) after all three lessons.

## Lab Escrow connection

Write Lab Escrow’s threat model and prove that a callback or invalid caller cannot take someone else’s funds.

**Status:** Not started

**Next module:** [Tokens and Composition](../module07-tokens-and-composition/README.md)
