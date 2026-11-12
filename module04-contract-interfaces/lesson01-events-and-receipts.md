# Lesson 4.1 — Events and receipts

**Course:** Ethereum 0–100, Edition 1  
**Module:** [04 — Contract Interfaces](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Read an event from a transaction receipt.
2. Separate indexed topics from event data.
3. Design events that support reconstruction of contract activity.

## 4.1.1 — A durable observation

A successful transaction can emit logs. Each log carries an emitting address, topics, and byte data. For an ordinary non-anonymous event, topic zero is the full Keccak-256 hash of the canonical event signature. Indexed static parameters occupy additional topics. Non-indexed parameters are ABI encoded in the data section.

```solidity
event ReadingSet(uint256 indexed id, address indexed writer, uint256 value);

function report(uint256 id, uint256 value) external {
    emit ReadingSet(id, msg.sender, value);
}
```

```bash
cast keccak 'ReadingSet(uint256,address,uint256)'
```

Compare this hash with topic zero on a receipt from a contract containing the fragment. IDs and writers can be filtered by topic position. A value in data can be decoded after the log is retrieved. Indexed dynamic values use a hash of a special encoding, so a string placed only in a topic cannot be recovered from that topic.

## 4.1.2 — Read a log as a typed record

The event declaration gives meaning to otherwise unlabelled bytes. `indexed` selects fields for topic-based filtering; it does not mean those fields are the only durable part of the log. For ReadingSet, a successful emission has three topics and one data word:

| Location | Meaning |
|---|---|
| Topic 0 | Full hash of `ReadingSet(uint256,address,uint256)` |
| Topic 1 | ID as a 32-byte unsigned word |
| Topic 2 | Writer address left-padded to a word |
| Data | Value as one 32-byte unsigned word |

Compare this with function calldata: its selector uses four bytes, while this event signature topic uses the full 32-byte hash. Both depend on canonical parameter types, but they occupy different encodings and serve different purposes.

The Solidity fragment must be placed inside a contract. Its report function records an event but contains no storage assignment. Calling it successfully can leave storage untouched while adding a log to the receipt. This is why “an event was emitted” and “a stored value was updated” are separate claims requiring separate evidence.

## 4.1.3 — Event evidence and state

External software can query logs, while ordinary Solidity execution cannot query historical receipt logs. Logs emitted in reverted execution do not survive. A receipt’s emitting address matters because a separate contract can emit an event with the same signature.

Use an address filter and the event signature for the Lab Escrow indexer. Publish enough fields to identify the deposit and the transition without requiring a second read that might observe a newer state.

## 4.1.4 — Design for a reader who arrives later

A useful event tells an indexer enough to interpret the transition at that historical moment. If an event says only “changed ID 5,” the indexer must make an additional state read. A read at latest may return a later state, producing an inconsistent reconstruction. Including the transition's amount and relevant actors avoids that ambiguity.

Event design is still a contract promise. Emitting a plausible event does not force the implementation to make a matching storage change. Tests should compare both effects, and reconciliation later compares the reconstructed view with on-chain getters.

For indexed dynamic fields, the topic is a hash rather than recoverable string content. That can support matching a known value, but it does not provide the original text to a general decoder. Choose indexed fields around actual filter needs, and retain non-indexed data when the value itself must be reconstructed.

Lab Escrow's event stream will become input to another program. Stable field meanings, emitting address checks, and exact signatures are therefore part of the interface just as much as function arguments. They should be tested and documented before the indexer relies on them.

## Self-review

Why can ReadingSet exist without a corresponding stored reading? Which bytes distinguish a function selector from an event signature topic?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity events](https://docs.soliditylang.org/en/latest/contracts.html#events)
