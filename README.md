# Ethereum 0–100 — Course Edition 1

A terminal-first course in Ethereum execution, Solidity, and smart-contract engineering. You start with a local ETH transfer and finish with Lab Escrow, a small ETH escrow with a Python event indexer.

## Course components

1. **Textbook.** Mental models, terminology, annotated examples, and engineering decisions.
2. **Lesson exercises.** Predict behavior, investigate it, then implement from written requirements.
3. **Module assessments.** Knowledge checks, independent practical assignments, and design reasoning, totaling 100 points each.
4. **Capstone.** Build up the skills for Lab Escrow in each module. Bring them together in Module 10.

## Start here

Use [Local setup](setup.md), then open [Lesson 1.1](module01-foundations/lesson01-state-and-accounts.md). The course assumes basic terminal familiarity. The final modules use Python functions, dictionaries, JSON, and SQLite. Read the lesson, predict before running commands, and keep your exercise implementation and written answers beside its specification. Start each submission from empty files.

Run transaction labs on loopback Anvil using its disposable accounts. Keep the RPC URL explicit in each command. Each lab states its required starting state. Starting a fresh Anvil process clears earlier deployments and local state. Save addresses and transaction hashes alongside observations that depend on them.

## Course textbook structure

| Module | Subject | Capstone checkpoint |
|---|---|---|
| 01 | [Foundations](module01-foundations/README.md) | Describe the chain state that holds Lab Escrow’s ETH and identify the transactions that can change it. |
| 02 | [RPC and ABI](module02-rpc-and-abi/README.md) | Establish how the indexer reads chain evidence and how a client builds contract calls. |
| 03 | [Solidity Foundations](module03-solidity-foundations/README.md) | Represent deposits with typed records and explicit state transitions. |
| 04 | [Contract Interfaces](module04-contract-interfaces/README.md) | Design a payable escrow interface and the events its indexer will consume. |
| 05 | [Testing and Debugging](module05-testing-and-debugging/README.md) | Define the tests that will gate Lab Escrow’s release, refund, and withdrawal paths. |
| 06 | [Smart Contract Security](module06-smart-contract-security/README.md) | Write Lab Escrow’s threat model and prove that a callback or invalid caller cannot take someone else’s funds. |
| 07 | [Tokens and Composition](module07-tokens-and-composition/README.md) | Evaluate which design and accounting rules would change if Lab Escrow accepted tokens. |
| 08 | [Off-chain Indexing](module08-off-chain-indexing/README.md) | Build a replayable SQLite index keyed by chain, deployment, and canonical block evidence. |
| 09 | [Networks and Operations](module09-networks-and-operations/README.md) | Guard Lab Escrow tooling against the wrong chain, stale manifests, and ambiguous transaction state. |
| 10 | [Lab Escrow Capstone](module10-lab-escrow-capstone/README.md) | Deliver a tested local contract and a verifiable event projection with a clean-room demo. |

Keep assessed work in `assessment-answers.md` and separate source files named by the assignment. Mark an exercise complete once you can reproduce its evidence and explain your results.
