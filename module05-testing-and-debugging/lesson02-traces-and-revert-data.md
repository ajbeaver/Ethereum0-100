# Lesson 5.2 — Traces and revert data

**Course:** Ethereum 0–100, Edition 1  
**Module:** [05 — Testing and Debugging](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Read a call tree and identify the first relevant failure.
2. Distinguish a custom error from a panic and string error.
3. Turn a trace into a minimal regression test.

## 5.2.1 — Follow the failure through nested calls

Run `forge test -vvvv --match-test testName` from a Forge project to see the calls for a focused test. Start at the top and note each callee, function, value, and outcome. The withdrawal example below distinguishes the original failure from the outer contract’s response.

Follow both the failing frame and the outer frame that handles or propagates it. The failure path matters as much as the final bytes.

Imagine a withdrawal whose recipient rejects ETH. The call tree may show the vault invoking the receiver, the receiver reverting, the low-level call returning false to the vault, and the vault reverting with `PayoutFailed`. The outer error describes the vault's response. It does not mean the receiver itself raised that error.

A trace is useful because it retains nesting. Follow the caller/callee relationship and attached value into the first frame relevant to your hypothesis. An earlier irrelevant call in the same test is not automatically the cause. Likewise, a frame that bubbles error bytes unchanged may report the same selector without originating the failure.

Run the focused command with your real test name in place of `testName`. Keep the initial fixture and one triggering action small. A long integration trace can establish overall behavior but is harder to use as a minimal explanation of a single wrong guard.

## 5.2.2 — Decode the first four bytes

`Error(string)` starts with `0x08c379a0`. `Panic(uint256)` starts with `0x4e487b71`, with a code such as `0x11` for checked arithmetic overflow or underflow. Custom errors use selectors derived from their canonical signatures. A revert can return empty data.

```bash
cast sig 'Unauthorized(address)'
cast sig 'PayoutFailed()'
```

## 5.2.3 — Interpret error bytes with a schema

The first four bytes distinguish ordinary ABI error signatures. `Unauthorized(address)` adds one encoded address word, whereas a no-argument custom error can consist of just its selector. `Panic(uint256)` supplies a numerical reason; arithmetic code `0x11` is a clue to inspect checked arithmetic. It is not proof that a particular public input caused it.

String errors carry dynamic ABI data. Empty revert bytes contain no encoded reason to decode. A client should not manufacture one; use the trace and known code path to investigate. Return bytes are also controlled by callees, so error formatting is diagnostic evidence rather than an authentication mechanism.

## 5.2.4 — Preserve the failure as a regression

Write down the valid starting state, the exact actor and timestamp, the attempted call, and the incorrect observable result. Add a deterministic test expressing the correct expectation and establish that it fails on the faulty code. Then fix the smallest relevant cause and rerun that test and the surrounding suite.

For a wrong-time defect, include the exact boundary in the regression. For a wrong-caller defect, use distinct addresses and assert the relevant state is preserved. Capturing only a screenshot of a long trace loses the executable setup needed to reproduce the finding later.

## Self-review

Why can the final error differ from the recipient's original error? Which parts of a trace must be retained to turn the observation into a reproducible test?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity error handling](https://docs.soliditylang.org/en/latest/control-structures.html#error-handling-assert-require-revert-and-exceptions)
