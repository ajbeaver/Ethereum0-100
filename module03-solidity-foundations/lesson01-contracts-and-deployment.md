# Lesson 3.1 — Contracts and deployment

**Course:** Ethereum 0–100, Edition 1  
**Module:** [03 — Solidity Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Compile, deploy, and call a minimal Solidity contract.
2. Distinguish creation code from runtime code.
3. Identify which constructor inputs determine initial state.

## 3.1.1 — Source, bytecode, and address

A Solidity source file describes one or more contracts. Forge invokes the compiler and stores artifacts under `out/`, including an ABI for interactions and bytecode for execution. This lesson follows that source through initialization and deployment, distinguishing the artifacts as they become relevant.

### Create the lesson project

Foundry installation and local-node checks are covered in [local setup](../setup.md). Project layout and compiler configuration belong to this lesson. From the course root, create a separate project for the textbook example:

```bash
cd module03-solidity-foundations/exercises/lesson01
mkdir counter-demo
cd counter-demo
mkdir -p src test
cat > foundry.toml <<'TOML'
[profile.default]
src = "src"
test = "test"
solc = "0.8.30"
evm_version = "cancun"
TOML
```

Run these commands in Bash/zsh, or Ubuntu in WSL on Windows. Paste the configuration block through the final `TOML` line. Start with a new `counter-demo` directory; if it already exists from an earlier attempt, inspect it or choose a different name rather than overwriting your work.

The **project root** is now `counter-demo/`, containing `foundry.toml`, `src/`, and `test/`. Forge reads this configuration when you run commands here. `solc` pins the Solidity compiler to 0.8.30; `evm_version` targets Cancun. Forge may download that compiler on the first build, so the first build needs internet access. Later builds can reuse the downloaded compiler. Module 05 introduces the Forge test library.

Use a separate project directory for independent assignments, following the same layout and configuration. Keep this textbook demonstration separate from your exercise implementation.

Write this textbook example in `src/Counter.sol`:

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

Read the example before deploying it: distinguish the stored variable, its generated getter, the constructor, and the update method.

## 3.1.2 — Read the Solidity example as a program

The SPDX line declares the source license. `pragma solidity 0.8.30` restricts the compiler accepted by this source; the project's configuration tells Forge which compiler to select. `contract Counter` defines a contract type. `uint256` is an unsigned 256-bit integer, so `value` cannot represent negative numbers.

A state variable declared outside functions belongs to persistent contract storage. `public` asks the compiler to expose a getter named `value()`. It does not make the variable directly assignable by a caller. `external` makes `increment` callable through the contract's external interface. Its body changes storage, so a committed update requires a transaction.

The constructor accepts `initialValue` during creation and assigns it to the new instance's storage. That parameter is not a permanent second storage variable. This is why creation needs an argument while later increments do not.

## 3.1.3 — Separate three artifacts of deployment

**Source** is what the developer writes. **Creation code** runs to initialize a deployment. **Runtime code** is the program installed at the created address and used by later calls. `cast code` queries that installed runtime, not the source file or constructor execution history.

Two deployments constructed with 7 and 12 can have the same runtime code while their getters return different values. Storage is specific to each address. Modifying a local source file afterward does not replace the code already deployed. Build and deploy again if you intend to test changed code, then use the newly returned address.

In the deployment command below, `src/Counter.sol:Counter` names the file and contract type to deploy. `--constructor-args 7` supplies the initialization input. `--broadcast` requests an actual submission rather than only a deployment simulation, and `--unlocked` uses a local node account for authorization.

A deployed address alone is not sufficient evidence for repeating the lab: retain its chain context and receipt. The next read must use that exact address and endpoint. If the getter returns nothing, first check whether the selected chain actually has code at that address before debugging the arithmetic.

## 3.1.4 — Deploy and inspect

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

## Self-review

Which parts of the example exist after the constructor finishes: runtime code, constructor parameter name, and stored value? Why can compiling a changed source file leave an old deployed Counter unchanged?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity contracts](https://docs.soliditylang.org/en/latest/contracts.html)
