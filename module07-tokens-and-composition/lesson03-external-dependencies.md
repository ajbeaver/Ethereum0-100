# Lesson 7.3 — External dependencies

**Course:** Ethereum 0–100, Edition 1  
**Module:** [07 — Tokens and Composition](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Identify trust boundaries at external addresses.
2. Reason about adversarial returns and callbacks.
3. Choose a narrow asset support policy and test it.

## 7.3.1 — Interfaces describe inputs and outputs

An interface makes code callable. It cannot guarantee that the callee implements honest accounting, stays available, or has fixed behavior. A proxy can expose a stable address while changing implementation. A token may charge transfer fees. An oracle may return stale data. Record what the caller needs from each dependency and how it fails when that assumption breaks.

## 7.3.2 — Observe the actual balance change

A fee-taking token can receive a request to transfer 100 units and deliver 97 to the vault. Crediting 100 would overstate the claim. If supporting that class of token, compare the actual before and after balance under a documented asset policy. Balance differences alone cannot make an arbitrary malicious token trustworthy. A simpler policy can reject assets with unsupported behavior.

The course capstone accepts only native ETH. Write this decision as part of its threat model and list the interfaces that would change for token support. Keep price feeds and upgradeable dependencies out of the capstone’s critical path.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity external calls and security](https://docs.soliditylang.org/en/latest/security-considerations.html)
