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

## 4.1.2 — Event evidence and state

The example emits a reading without persisting it in storage. External software can query logs. Ordinary Solidity execution cannot query historical receipt logs. Logs emitted in reverted execution do not survive. A receipt’s emitting address matters because a separate contract can emit an event with the same signature.

Use an address filter and the event signature for the Lab Escrow indexer. Publish enough fields to identify the deposit and the transition without requiring a second read that might observe a newer state.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity events](https://docs.soliditylang.org/en/latest/contracts.html#events)
