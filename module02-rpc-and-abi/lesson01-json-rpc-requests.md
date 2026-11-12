# Lesson 2.1 — JSON-RPC requests

**Course:** Ethereum 0–100, Edition 1  
**Module:** [02 — RPC and ABI](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Read a JSON-RPC request and interpret its response.
2. Decode hex quantities without floating-point arithmetic.
3. Distinguish missing results from RPC and transport errors.

## 2.1.1 — Read a JSON-RPC request and response

JSON-RPC is a request-response protocol carried here over HTTP. Treat transport, JSON parsing, protocol success, and application meaning as distinct steps. Start by following one request through those layers.

```bash
curl --silent --show-error --fail-with-body "$RPC_URL" \
  -H 'Content-Type: application/json' \
  --data '{"jsonrpc":"2.0","id":7,"method":"eth_chainId","params":[]}'
```

The shell runs `curl`, a command-line HTTP client. The URL chooses the node. The content-type header tells that endpoint the body is JSON. Inside the body, `jsonrpc` names the protocol version, `method` selects the operation, `params` supplies arguments in the order the method expects, and `id` labels this request. The ID is not a chain ID, block number, or transaction nonce.

A successful response to the example has this shape:

```json
{"jsonrpc":"2.0","id":7,"result":"0x7a69"}
```

The node returns a hexadecimal string because Ethereum RPC quantities use that representation. The response ID relates it to request 7; the result decodes to 31337. The same HTTP exchange could instead return a protocol error object. Checking only whether Curl exited successfully would miss that distinction.

## 2.1.2 — Quantities and bytes answer different questions

A quantity encodes an integer without unnecessary leading zero digits; zero is `"0x0"`. Byte data preserves byte positions and therefore needs two hexadecimal digits per byte; empty code or calldata is `"0x"`. Converting a 32-byte hash to an integer and printing it back can discard meaningful leading zero bytes, even if its numerical value is unchanged.

Run this independent Python example:

```python
height = int("0x10", 16)
chain_id = int("0x7a69", 16)
print(height, chain_id)
print(hex(height + 1))
```

It prints `16 31337` and then `0x11`. The second argument 16 tells `int` the input's base. The integer can be used for arithmetic or sorting; `hex` converts it back to the RPC-style notation needed for a later request.

## 2.1.3 — Pinning a read

`eth_getBalance` expects an address followed by a block parameter. Using `"latest"` asks the node to resolve the head each time. If you fetch three balances while the head advances, the results can refer to three different blocks. Pin one block number for a coherent lab snapshot. When canonicality matters, record and recheck the block hash as well.

## 2.1.4 — Missing is an application state

`eth_getTransactionReceipt` can return a successful envelope with `null` as its result. That is not a failed receipt: it says this endpoint did not supply one. The transaction could still be pending, unknown to that node, or absent from its current history. A JSON-RPC error says the request itself failed at the protocol/application boundary. An HTTP timeout says the caller did not obtain a timely transport result. None authorizes a tool to invent a successful balance of zero.

The application must decide whether missing evidence calls for polling, reconciliation, or a missing-data report. Combined with the pinned-read policy above, that decision turns remote answers into coherent observations.

## Self-review

Which of the URL, request ID, chain ID result, and block parameter selects which thing? Why must a missing receipt not be converted into a mined failure?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC](https://ethereum.org/developers/docs/apis/json-rpc/)
