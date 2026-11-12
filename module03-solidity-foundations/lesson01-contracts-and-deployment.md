# Lesson 3.1 — Contracts and deployment

**Course:** Ethereum 0–100, Edition 1  
**Module:** [03 — Solidity Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Compile, deploy, and call a minimal Solidity contract.
2. Distinguish creation code from runtime code.
3. Identify which constructor inputs determine initial state.

## 3.1.1 — Source, bytecode, and address

A Solidity source file describes one or more contracts. Forge invokes the Solidity compiler and stores artifacts under `out/`. The ABI describes interactions and the bytecode provides executable instructions. Contract creation executes initialization code. Its returned runtime code is installed at the new address. The constructor runs during initialization and can write initial storage.

Create a project using [local setup](../setup.md#4-start-a-solidity-project-in-module-03). Write this textbook example in `src/Counter.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.30;

contract Counter {
    uint256 public value;

    constructor(uint256 initialValue) {
        value = initialValue;
    }

    function increment() external {
        value += 1;
    }
}
```

The compiler supplies a getter for `value`. The constructor gives each deployment its own initial storage. Two Counter deployments can have identical runtime code and independent values.

## 3.1.2 — Deploy and inspect

From the project root with Anvil running:

```bash
forge build
forge create --rpc-url "$RPC_URL" --from "$ALICE" --unlocked --broadcast \
  src/Counter.sol:Counter --constructor-args 7
```

Set `COUNTER` to the deployed address printed by Forge. Then run:

```bash
cast code --rpc-url "$RPC_URL" "$COUNTER"
cast call --rpc-url "$RPC_URL" "$COUNTER" 'value()(uint256)'
cast send --rpc-url "$RPC_URL" --from "$ALICE" --unlocked "$COUNTER" 'increment()'
cast call --rpc-url "$RPC_URL" "$COUNTER" 'value()(uint256)'
```

The getter reads 7 before the increment and 8 afterward. The deployed code bytes stay the same. Capture both the deployment transaction hash and address in your lab notes.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity contracts](https://docs.soliditylang.org/en/latest/contracts.html)
