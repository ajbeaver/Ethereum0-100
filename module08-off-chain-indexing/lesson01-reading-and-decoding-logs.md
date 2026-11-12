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

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC, eth_getLogs](https://ethereum.org/developers/docs/apis/json-rpc/#eth_getlogs)
