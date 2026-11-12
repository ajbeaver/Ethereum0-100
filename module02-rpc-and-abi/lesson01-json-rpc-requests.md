# Lesson 2.1 — JSON-RPC requests

**Course:** Ethereum 0–100, Edition 1  
**Module:** [02 — RPC and ABI](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Read a JSON-RPC request and interpret its response.
2. Decode hex quantities without floating-point arithmetic.
3. Distinguish missing results from RPC and transport errors.

## 2.1.1 — Method, parameters, and response

JSON-RPC is a request-response protocol. The request names a method and provides parameters in the order that method expects. An `id` lets the caller match a response with its request. HTTP success can carry a JSON-RPC `error`. Treat HTTP transport, JSON parsing, protocol success, and application meaning as distinct steps.

```bash
curl --silent --show-error --fail-with-body "$RPC_URL" \
  -H 'Content-Type: application/json' \
  --data '{"jsonrpc":"2.0","id":7,"method":"eth_chainId","params":[]}'
```

On the course chain the result is `"0x7a69"`. Decode it with `int("0x7a69", 16)` in Python. A zero quantity is `"0x0"`. Byte data uses whole bytes. Empty code or calldata is `"0x"`. A 32-byte hash can begin with zero bytes that must be preserved.

## 2.1.2 — Read one request layer by layer

The shell runs `curl`, a command-line HTTP client. The URL chooses the node. The content-type header tells that endpoint the body is JSON. Inside the body, `jsonrpc` names the protocol version, `method` selects the operation, `params` supplies its arguments, and `id` labels this request. The ID is not a chain ID, block number, or transaction nonce.

A successful response to the example has this shape:

```json
{"jsonrpc":"2.0","id":7,"result":"0x7a69"}
```

The node returns a hexadecimal string because Ethereum RPC quantities use that representation. The response ID relates it to request 7; the result decodes to 31337. The same HTTP exchange could instead return a protocol error object. Checking only whether Curl exited successfully would miss that distinction.

## 2.1.3 — Quantities and bytes answer different questions

A quantity encodes an integer without unnecessary leading zero digits. Byte data preserves byte positions and therefore needs two hexadecimal digits per byte. Converting a hash to an integer and printing it back can discard meaningful leading zero bytes, even if its numerical value is unchanged.

Run this independent Python example:

```python
height = int("0x10", 16)
chain_id = int("0x7a69", 16)
print(height, chain_id)
print(hex(height + 1))
```

It prints `16 31337` and then `0x11`. The second argument 16 tells `int` the input's base. The integer can be used for arithmetic or sorting; `hex` converts it back to the RPC-style notation needed for a later request.

## 2.1.4 — Pinning a read

`eth_getBalance` expects an address followed by a block parameter. Using `"latest"` asks the node to resolve the head each time. If you fetch three balances while the head advances, the results can refer to three different blocks. Pin one block number for a coherent lab snapshot. When canonicality matters, record and recheck the block hash as well.

`eth_getTransactionReceipt` can return a successful JSON-RPC envelope containing `null`. That means the endpoint has no receipt to return for the hash at that time. The application must decide whether to poll, reconcile chain state, or report missing data.

## 2.1.5 — Missing is an application state

A `null` receipt is not a failed receipt. It says this endpoint did not supply one. The transaction could still be pending, unknown to that node, or absent from its current history. A JSON-RPC error says the request itself failed at the protocol/application boundary. An HTTP timeout says the caller did not obtain a timely transport result. None authorizes a tool to invent a successful balance of zero.

For related reads, capture one block height and query all values there. If a reorganization could affect the interpretation, capture and recheck its hash too. This is how raw remote answers become a coherent observation suitable for an indexer or reconciliation report.

## Self-review

Which of the URL, request ID, chain ID result, and block parameter selects which thing? Why must a missing receipt not be converted into a mined failure?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC](https://ethereum.org/developers/docs/apis/json-rpc/)
