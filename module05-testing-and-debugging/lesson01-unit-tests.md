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

Save this as `test/Counter.t.sol` and run `forge test`. The next section explains why each test can expect its own initial Counter state.

## 5.1.2 — Understand the test environment

`CounterTest is Test` inherits assertion helpers and the `vm` cheatcode interface from forge-std. The test fixture's `counter` variable refers to the freshly deployed Counter. `setUp` initializes it before each ordinary test so increments in one test do not prepare or contaminate another.

`assertEq(counter.value(), 8)` has an independent expectation: constructor value 7 plus one increment. If the test calculated its expected value by calling the same increment implementation again, it could reproduce the defect on both sides. Begin with a small requirement whose expected result you can calculate by hand.

Forge's in-process test execution does not need the Anvil node used for terminal deployment labs. Cheatcodes let the test control caller, time, and balances deterministically. They are testing facilities, not Solidity functions available to an arbitrary production contract.

## 5.1.3 — Control one variable at a time

`vm.prank(alice)` sets the caller for the next applicable call. Apply it just before the call you want to test. `vm.deal(alice, amount)` sets the test balance, and `vm.warp(deadline)` changes the test timestamp. To verify a custom error with arguments, pass the exact expected bytes to `vm.expectRevert(abi.encodeWithSelector(...))` before the target call.

A passing revert assertion establishes the call failed with the expected error. Also assert the relevant state is unchanged. For a rejected escrow release, inspect both the deposit status and the payee’s credit.

## 5.1.4 — Scope the action you intend to test

A prank affects the next applicable external call, so even an innocent getter between prank and action can change which call receives the intended sender. Read all setup values first, establish the expected failure, then place caller control immediately before the target operation. Inspect the trace if the observed caller surprises you.

For a timed record, one useful test progression is: construct an active record, select the wrong actor, expect its exact authorization error, attempt the update, then assert the state is still active and the stored value unchanged. A different test controls time while keeping the actor valid. Changing both variables at once makes it harder to identify which guard is responsible.

## 5.1.5 — A revert expectation is only one assertion

A broad “some revert happened” test can pass because the record is missing, not because authorization works. Exact custom-error bytes establish the intended path more narrowly. State assertions establish absence of unwanted effects. For a successful change, inspect both the new record value and any relevant aggregate rather than assuming a successful call implies a correct business result.

The goal is a suite organized around promises. For Lab Escrow those promises include who can settle, when settlement is possible, who receives credit, and what failure preserves. Tests named for those behaviors remain useful when internal helper functions are refactored.

## Self-review

Why can a getter change the meaning of a pending prank? What separate facts do an exact revert assertion and an unchanged-state assertion establish?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Foundry writing tests](https://getfoundry.sh/forge/writing-tests)
