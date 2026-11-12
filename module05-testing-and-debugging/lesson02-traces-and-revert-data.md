# Lesson 5.2 — Traces and revert data

**Course:** Ethereum 0–100, Edition 1  
**Module:** [05 — Testing and Debugging](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Read a call tree and identify the first relevant failure.
2. Distinguish a custom error from a panic and string error.
3. Turn a trace into a minimal regression test.

## 5.2.1 — Follow the nested calls

Run `forge test -vvvv --match-test testName` from a Forge project to see the calls for a focused test. Start at the top and note each callee, function, value, and outcome. If a withdrawal fails inside a recipient, the trace may show an inner revert followed by a custom error in the outer contract. Determine which contract detected each failure.

An error can be bubbled up unchanged by another contract. A matching selector helps identify bytes. It cannot by itself prove their origin. Trace context identifies the call frame that returned the data.

## 5.2.2 — Decode the first four bytes

`Error(string)` starts with `0x08c379a0`. `Panic(uint256)` starts with `0x4e487b71`, with a code such as `0x11` for checked arithmetic overflow or underflow. Custom errors use selectors derived from their canonical signatures. A revert can return empty data.

```bash
cast sig 'Unauthorized(address)'
cast sig 'PayoutFailed()'
```

When you find a failure, record the smallest setup that reproduces it. Add assertions around the actual symptom before changing implementation. A reproducible trace helps connect failed behavior to the exact call path.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity error handling](https://docs.soliditylang.org/en/latest/control-structures.html#error-handling-assert-require-revert-and-exceptions)
