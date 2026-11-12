# Lesson 8.1 — Reading and decoding logs

**Course:** Ethereum 0–100, Edition 1  
**Module:** [08 — Off-chain Indexing](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Filter logs by contract address, topic, and bounded block range.
2. Decode static topics and data with Python.
3. Order logs for deterministic replay.

## 8.1.1 — Query only the relevant history

`eth_getLogs` accepts a filter with `address`, `topics`, `fromBlock`, and `toBlock`. Block numbers are RPC quantities. Topic constraints match positions, and `null` can leave a topic unconstrained. Start at the deployment block. Large historical requests can time out or hit provider limits, so fetch bounded chunks and retain the requested range with the response.

For the ReadingFeed example, use `cast keccak 'ReadingSet(uint256,address,uint256)'` for topic zero. Set `FEED` to the deployed address and `TOPIC` to the hash:

```bash
cast rpc --rpc-url "$RPC_URL" eth_getLogs \
  "{\"address\":\"$FEED\",\"fromBlock\":\"0x0\",\"toBlock\":\"latest\",\"topics\":[\"$TOPIC\"]}"
```

Use this broad range only for the short local chain. A real indexer should choose explicit chunk boundaries.

## 8.1.2 — Decode the known schema

For ReadingSet, parse topic one as an integer ID, and extract the last 20 bytes of topic two as the address. Decode the single 32-byte data word as an unsigned integer. Validate word lengths and signature before parsing. Sort by block number, transaction index, and log index after decoding the hex quantities. Lexicographic sorting puts `0x10` before `0x2`.

In Python, `urllib.request` and `json` suffice to call a local endpoint. Keep transport, decoding, and state projection separate so you can replay saved logs in a unit test.

## 8.1.3 — Turn a remote log into a validated record

Retrieval, decoding, and projection are different operations. Retrieval obtains a response from a node. Decoding checks the known event schema and converts bytes to typed values. Projection applies those values to a derived application state. Separating them lets a saved response drive a deterministic test without a running node.

For ReadingSet, require topic zero plus exactly two indexed arguments and one 32-byte data word. Extract an address from the final 20 bytes of its topic, while checking the expected padding for the static schema. Reject malformed lengths rather than slicing whatever bytes happen to be present.

Here is an isolated integer-word example:

```python
word = "0x" + "0" * 62 + "2a"
raw = bytes.fromhex(word[2:])
print(len(raw))
print(int.from_bytes(raw, byteorder="big"))
```

The output is 32 and 42. Removing `0x` supplies hexadecimal digits to `bytes.fromhex`; big-endian means the highest-order byte comes first. This example establishes only integer decoding, not that the word came from the intended contract.

## 8.1.4 — Select and order evidence deliberately

An address filter prevents another contract with the same signature from entering the intended stream. Topic positions identify indexed arguments, so a filter for writer belongs in its declared position, not wherever a matching byte string appears. Pin explicit starting and ending heights for each accepted range.

Decode block number, transaction index, and log index to integers before ordering. A textual hexadecimal comparison can put block `0x10` before `0x2`, incorrectly applying later events first. The transaction hash identifies a transaction, but it does not replace chain position as the ordering rule.

An empty result can be a valid observation of no matching events in the requested range. It still needs a successful transport and protocol response. A timeout must not be turned into an empty range and checkpointed as processed. Store enough request context to distinguish the two.

In Lab Escrow, order determines meaning: an open must precede settlement, which must precede withdrawal of the resulting credit. A decoder that produces correct individual amounts can still create an impossible state if the application sorts the logs incorrectly.

## Self-review

Which checks establish a log's schema, and which establish its intended source? Why should an empty successful range advance progress while a timed-out request must not?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC, eth_getLogs](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getlogs)
