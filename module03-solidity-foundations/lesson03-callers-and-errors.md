# Lesson 3.3 — Callers and errors

**Course:** Ethereum 0–100, Edition 1  
**Module:** [03 — Solidity Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Use msg.sender and msg.value to enforce function requirements.
2. Describe when a revert rolls back writes.
3. Create a readable public interface with custom errors.

## 3.3.1 — Inputs supplied by execution

`msg.sender` is the immediate caller of the current call; `msg.value` is the wei attached to it. A Solidity function that accepts attached ETH must be payable. Trace a relay call to see why both values belong to the current call frame.

Store an authority deliberately. A constructor that sets an owner from `msg.sender` will assign ownership to a factory if a factory deploys it. An explicit constructor argument can make ownership predictable across deployment paths.

## 3.3.2 — Trace authority through a call chain

Consider `Alice -> Relay -> Meter`. In Meter's call frame, `msg.sender` is Relay. Alice initiated the transaction, but Relay is making the immediate call. Authorization based on the immediate caller can therefore allow a contract wallet or reject an unapproved intermediary deliberately. Do not assume every useful caller is an EOA.

`msg.value` belongs to that frame too. Relay can receive ETH without forwarding it to Meter, or forward a chosen amount. Being payable permits a function to receive value; it does not require positive value or establish who should receive credit. Those are separate checks in the function's contract.

In the authorization fragment, `immutable` means the operator is selected during construction and cannot later be reassigned by ordinary Solidity code. `version` is mutable storage, changing only when the authorized path succeeds. Put the fragment inside a contract declaration to compile it; the lesson focuses on this interface boundary, not a complete deployable application.

## 3.3.3 — An authorization boundary

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

## 3.3.4 — A revert defines an execution boundary

If `Unauthorized` is raised before the increment, the version is unchanged. Even if code made an earlier write in the same reverted call, that write would not survive the revert. This rollback is an EVM execution property; it is different from an ordinary Python exception, which does not undo prior mutations.

The boundary matters with nested calls. A failed inner call rolls back that inner call's changes. If an outer caller catches the failure or uses a low-level call and continues, the whole transaction need not fail. Module 04 develops that case; for the direct calls here, inspect the top-level result and getter together.

Custom errors give clients structured failure data: a selector identifies the signature and ABI words describe arguments such as the actual caller. They are not an access-control mechanism themselves. The comparison against `operator` enforces the rule; the error communicates its rejection.

A useful observation sequence is: read version, simulate the unauthorized call, confirm rejection, then read version again. Repeat an authorized operation using a transaction and a successful receipt before claiming persistent progress. That comparison reinforces the earlier distinction between simulation and committed execution without requiring deliberately mined failing transactions yet.

## Self-review

Which address does a contract observe when a relay calls it? Why do a custom error and an authorization check serve different responsibilities?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity functions and errors](https://docs.soliditylang.org/en/latest/contracts.html#functions)
