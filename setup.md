# Local setup — Ethereum 0–100

Start here to install the tools, even if Ethereum development is new to you. Use macOS or Linux directly, or Ubuntu through WSL on Windows. Commands in this course use Bash-compatible shell syntax; macOS's default zsh also supports the examples.

You need an internet connection for installation and the first compiler/library downloads. The labs run on your own computer with free tools and local test ETH. You do not need a wallet, exchange account, paid RPC service, or real ETH.

**Course network:** local Anvil, chain ID 31337.

**Solidity examples:** compiler 0.8.30, with a Cancun EVM target.

**Python:** 3.12 or newer for the later indexing labs.

## 1. Choose your terminal

### macOS

Open **Terminal** from Applications → Utilities. Check the installer prerequisites:

```bash
curl --version
git --version
```

If Git prompts you to install Apple's Command Line Tools, complete that installation. You can also start it with `xcode-select --install`, then reopen Terminal and check Git again.

### Linux

Open your **Terminal** application. On Ubuntu/Debian, install the tools used to download Foundry and its Git-based lab dependencies:

```bash
sudo apt update
sudo apt install curl git unzip ca-certificates
```

Other distributions should install the equivalent tools using their own package manager. `sudo` may request your password without displaying typed characters.

### Windows: use Ubuntu in WSL

WSL runs a Linux environment on Windows, allowing the course commands to work as written. On Windows 11 or Windows 10 version 2004+ (build 19041+):

1. Open **PowerShell as administrator** from the Start menu.
2. Run:

```powershell
wsl --install -d Ubuntu
```

3. Restart Windows if prompted. Open **Ubuntu** from the Start menu and finish its first-run setup, including creating a Linux username and password.
4. In Ubuntu, follow the **Linux** prerequisite commands above.

From now on, run course commands in **Ubuntu**, not PowerShell. Install Foundry and Python inside Ubuntu too. Keep Ethereum lab projects in your Linux home folder for this route.

See [Microsoft's WSL installation guide](https://learn.microsoft.com/en-us/windows/wsl/install) if WSL is already installed, the command fails, or your Windows version needs a different procedure. Foundry also supports [Git Bash](https://gitforwindows.org/) as an installer environment, but this guide uses WSL to keep the full course workflow consistent. Foundryup does not run directly in PowerShell or Command Prompt; see the [official Foundry installation guide](https://getfoundry.sh/introduction/installation/).

## 2. Install Foundry

Foundry supplies **Forge** (builds and tests contracts), **Cast** (interacts with Ethereum nodes), and **Anvil** (runs a local development node).

In macOS Terminal, Linux Terminal, or Ubuntu in WSL, run the official bootstrap command. It downloads and executes Foundry's installer:

```bash
curl -L https://foundry.paradigm.xyz | bash
```

Close and reopen that terminal so the installer's command-path changes take effect. Then install the stable toolchain:

```bash
foundryup
```

Check that all three commands are available:

```bash
forge --version
cast --version
anvil --version
git --version
```

Each should print a version without a command-not-found error. Save these versions with your lab evidence. Later, `foundryup` updates the toolchain; record the new versions if you update during the course. Rust is not required when using the prebuilt Foundry installation.

If `foundryup` is missing, check the installer's final instructions and reopen the correct shell. If downloads or installation fail, use [Foundry troubleshooting](https://www.getfoundry.sh/help/troubleshooting) rather than skipping verification. Do not run the installer with `sudo`.

## 3. Install Python for the indexing labs

Use the [Python course's installation and environment guide](https://github.com/ajbeaver/Python0-100/blob/main/setup.md). Follow the section for your current environment: **macOS**, **Linux**, or **Linux inside WSL**. You do not need to download or complete the Python course just to install Python.

Install the interpreter using that guide, then return here. When an Ethereum exercise needs Python, create its `.venv` in that exercise's own project root using the guide's macOS/Linux commands. Do not reuse an environment copied from another project.

Check the interpreter in the same shell where you will run the Ethereum labs:

```bash
python3 --version
python3 -c "import sqlite3; print('SQLite', sqlite3.sqlite_version)"
```

Expect Python 3.12+ and a SQLite version. The required indexer uses the standard library, so no pip packages or separate SQLite server are needed. If a later Python command says `python` rather than `python3`, activate that lab's environment first.

## 4. Get the course

From a writable directory in your macOS/Linux terminal or Ubuntu home directory:

```bash
git clone https://github.com/ajbeaver/Ethereum0-100.git
cd Ethereum0-100
ls
```

If you already have the course, enter its existing folder instead of cloning another copy. The course root contains `README.md`, `setup.md`, and the module folders. Use a text editor for source files. Create coursework beside each exercise specification as directed; the public course provides requirements rather than completed solutions.

## 5. Start the local node

Open **Terminal A** (Ubuntu on Windows) and run:

```bash
anvil --host 127.0.0.1 --port 8545 --chain-id 31337
```

Leave it running. Anvil prints addresses and private keys for its prefunded development accounts. These are publicly known disposable accounts: use them only with this local node. Do not fund them on a public network.

Open **Terminal B** in the same operating-system environment. Run:

```bash
export RPC_URL=http://127.0.0.1:8545
cast chain-id --rpc-url "$RPC_URL"
cast rpc --rpc-url "$RPC_URL" eth_accounts
cast block-number --rpc-url "$RPC_URL"
```

Expect chain ID `31337`, a list of accounts, and block number `0` for a fresh idle node. `export` sets a shell variable in that terminal; repeat it in a new terminal. RPC means the request/response interface used to talk to the node. No public-network connection is needed for these requests.

Copy the first two account addresses from your Anvil run into the following commands, replacing the placeholder text and keeping the quotes:

```bash
export ALICE='PASTE_FIRST_ADDRESS'
export BOB='PASTE_SECOND_ADDRESS'
cast balance --rpc-url "$RPC_URL" "$ALICE"
```

A fresh Anvil account should have a positive balance, displayed in wei. `--unlocked` in later exercises asks the local node to send from its available accounts. Keep `--rpc-url` explicit so a command clearly names its intended node.

If Cast cannot connect, confirm Terminal A is still running and both terminals use the same URL/port. If port 8545 is occupied, stop your old lab node or select a free port in both Anvil's `--port` and Terminal B's `RPC_URL`.

## 6. Reset an experiment

In Terminal A, press **Ctrl-C** to stop Anvil, then run the node command again. With these commands, restarting starts fresh chain state: old deployments and transaction hashes no longer describe the new run. Repeat Terminal B's chain/account checks and record new deployment evidence. The lesson specifies when to reset and when to retain state.

You are ready for [Lesson 1.1](module01-foundations/lesson01-state-and-accounts.md).

## References

- [Foundry installation](https://getfoundry.sh/introduction/installation/)
- [Foundry troubleshooting](https://www.getfoundry.sh/help/troubleshooting)
- [Microsoft WSL installation](https://learn.microsoft.com/en-us/windows/wsl/install)
- [Python course setup](https://github.com/ajbeaver/Python0-100/blob/main/setup.md)
- [Foundry Anvil](https://getfoundry.sh/anvil)
- [Foundry Cast](https://getfoundry.sh/cast)
- [Ethereum JSON-RPC](https://ethereum.org/developers/docs/apis/json-rpc/)
