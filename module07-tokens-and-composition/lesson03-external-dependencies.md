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

## 7.3.2 — Write assumptions as statements you can challenge

A caller may assume that transferring v units delivers v units, that balanceOf reflects current holdings, that an oracle value is recent, or that code at a known address will remain behaviorally stable. Each is a separate assumption. A dependency can satisfy its function signatures while violating one of them.

For a fee-taking token, accepting a request for 100 and receiving 97 creates a three-unit shortfall if the vault grants a claim for 100. Measuring the difference helps only when balance reporting is trustworthy; rebasing can also change holdings after that measurement.

An allowlist of supported assets is a statement of intended scope, not a mathematical guarantee. It should connect each accepted behavior to tests and a reason the integration is designed to handle it. A generic “ERC-20 supported” claim is too broad if the implementation only works with exact-transfer fixed balances.

## 7.3.3 — Observe the actual balance change

Supporting that class of fee-taking token requires comparing the actual before and after balance under a documented asset policy. That policy must still cover balance reporting and later changes such as rebasing. A simpler integration can reject assets with unsupported behavior.

The course capstone accepts only native ETH. Write this decision as part of its threat model and list the interfaces that would change for token support. Keep price feeds and upgradeable dependencies out of the capstone’s critical path.

## 7.3.4 — Failure behavior belongs in the integration contract

Use local test doubles that each violate one assumption: return false, omit return data, reject the call, deliver less than requested, or attempt a callback. These fixtures let you ask whether the adapter rejects unsupported behavior or preserves accounting under supported behavior. They do not establish trust in an arbitrary real token.

A dependency review should record who can change its behavior. A proxy's address may remain fixed while an upgrade changes logic. Pinning an address alone does not freeze that trust relationship. An oracle can remain callable while its data is stale; availability is not freshness.

For Lab Escrow, the narrow native-ETH policy removes token return conventions, transfer fees, and oracle prices from the deposit path. The recipient callback boundary still exists at withdrawal and must be tested. Narrow scope makes the security argument smaller; it does not remove the need for one.

**Design principle:** Treat every external call as a boundary with explicit assumptions, supported failures, and observable accounting consequences. Expand asset support only when you can state and verify the new contract.

## Self-review

What assumption remains unproven after measuring a token's received balance difference? Why can a stable dependency address still expose changing behavior?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity external calls and security](https://docs.soliditylang.org/en/latest/security-considerations.html)
