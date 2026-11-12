# Local setup

**Environment:** macOS, zsh, Foundry, Python 3.  
**Course network:** local Anvil, chain ID 31337.  
**Solidity examples:** compiler 0.8.30, with a Cancun EVM target for predictable bytecode.

## 1. Check tools

```bash
command -v anvil cast forge python3
anvil --version
cast --version
forge --version
python3 --version
```

If you need Foundry, follow its [official installation guide](https://getfoundry.sh/introduction/installation/). Forge may download the pinned compiler on the first build. Module 05 introduces Forge's test library.

## 2. Start the local node

In Terminal A:

```bash
anvil --host 127.0.0.1 --port 8545 --chain-id 31337
```

Anvil prints public addresses for its unlocked, prefunded development accounts. Use the first two addresses as `ALICE` and `BOB`. Leave Terminal A open while working.

In Terminal B:

```bash
export RPC_URL=http://127.0.0.1:8545
cast chain-id --rpc-url "$RPC_URL"
cast rpc --rpc-url "$RPC_URL" eth_accounts
cast block-number --rpc-url "$RPC_URL"
```

Expect chain ID `31337`. Set the two addresses in Terminal B, using the addresses from your run:

```bash
export ALICE='PASTE_FIRST_ADDRESS'
export BOB='PASTE_SECOND_ADDRESS'
cast balance --rpc-url "$RPC_URL" "$ALICE"
```

Balances are in wei. These development accounts are publicly known. Use them only with this local node. `--unlocked` sends via the node's available accounts, which suits Anvil exercises.

## 3. Reset an experiment

Stop your Anvil process with Ctrl-C, then repeat step 2. Recheck addresses, balances, and chain ID. If the port is occupied, use a free port in both terminals and verify that your shell points to that process.

## 4. Start a Solidity project in Module 03

In a new lab directory inside an exercise folder:

```bash
mkdir -p src test
cat > foundry.toml <<'TOML'
[profile.default]
src = "src"
test = "test"
solc = "0.8.30"
evm_version = "cancun"
TOML
```

Run `forge build` from that directory once you have written a contract in `src/`. Use a separate project for an independent assignment if you want clean build artifacts and deployments.

## References

- [Foundry Anvil](https://getfoundry.sh/anvil)
- [Foundry Cast](https://getfoundry.sh/cast)
- [Ethereum JSON-RPC](https://ethereum.org/developers/docs/apis/json-rpc/)
