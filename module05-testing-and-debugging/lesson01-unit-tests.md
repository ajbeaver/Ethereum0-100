# Lesson 5.1 — Unit tests

**Course:** Ethereum 0–100, Edition 1  
**Module:** [05 — Testing and Debugging](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Write isolated Forge tests for success and rejection.
2. Use cheatcodes to control caller, balance, and time.
3. Assert both the intended effect and the absence of unwanted effects.

## 5.1.1 — An executable expectation

A test arranges state, acts, then asserts what changed. In the Counter project from Lesson 3.1, install the Foundry test library from the project root using `forge install foundry-rs/forge-std --no-git`. Keep the installed revision with the project if you need someone to reproduce the test later.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.30;

import {Test} from "forge-std/Test.sol";
import {Counter} from "../src/Counter.sol";

contract CounterTest is Test {
    Counter counter;

    function setUp() public {
        counter = new Counter(7);
    }

    function testIncrement() public {
        counter.increment();
        assertEq(counter.value(), 8);
    }
}
```

Save this as `test/Counter.t.sol` and run `forge test`. Forge runs each test in isolated state with a fresh setup. Build the case from a known state even if a nearby test seems to create the right fixture.

## 5.1.2 — Control one variable at a time

`vm.prank(alice)` sets the caller for the next applicable call. Apply it just before the call you want to test. `vm.deal(alice, amount)` sets the test balance, and `vm.warp(deadline)` changes the test timestamp. To verify a custom error with arguments, pass the exact expected bytes to `vm.expectRevert(abi.encodeWithSelector(...))` before the target call.

A passing revert assertion establishes the call failed with the expected error. Also assert the relevant state is unchanged. For a rejected escrow release, inspect both the deposit status and the payee’s credit.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Foundry writing tests](https://getfoundry.sh/forge/writing-tests)
