# Lesson 3.3 — Callers and errors

**Course:** Ethereum 0–100, Edition 1  
**Module:** [03 — Solidity Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Use msg.sender and msg.value to enforce function requirements.
2. Describe when a revert rolls back writes.
3. Create a readable public interface with custom errors.

## 3.3.1 — Inputs supplied by execution

`msg.sender` is the immediate caller of the current call. If Alice calls contract A and A calls contract B, B sees A as its caller. A direct call from Alice sees Alice. `msg.value` is the wei attached to the current call. A Solidity function that accepts attached ETH must be payable.

Store an authority deliberately. A constructor that sets an owner from `msg.sender` will assign ownership to a factory if a factory deploys it. An explicit constructor argument can make ownership predictable across deployment paths.

## 3.3.2 — An authorization boundary

```solidity
error Unauthorized(address caller);
address public immutable operator;
uint256 public version;

constructor(address initialOperator) {
    operator = initialOperator;
}

function advance() external {
    if (msg.sender != operator) revert Unauthorized(msg.sender);
    version += 1;
}
```

The custom error’s selector and encoded arguments describe the failure. If the top-level call reverts, its writes and logs roll back. A successful `cast call` of `advance()` simulates the update but leaves the version unchanged. To observe actual persistence, send from the authorized account and read the getter after a successful receipt.

A failing Cast send may be rejected during gas estimation. To distinguish that case from mined failure, look for an actual transaction hash and receipt. Use a failing simulation for early labs, then write explicit revert tests in Module 05.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity functions and errors](https://docs.soliditylang.org/en/latest/contracts.html#functions)
