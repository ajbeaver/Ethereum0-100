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

## 2.1.2 — Pinning a read

`eth_getBalance` expects an address followed by a block parameter. Using `"latest"` asks the node to resolve the head each time. If you fetch three balances while the head advances, the results can refer to three different blocks. Pin one block number for a coherent lab snapshot. When canonicality matters, record and recheck the block hash as well.

`eth_getTransactionReceipt` can return a successful JSON-RPC envelope containing `null`. That means the endpoint has no receipt to return for the hash at that time. The application must decide whether to poll, reconcile chain state, or report missing data.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Ethereum JSON-RPC](https://ethereum.org/developers/docs/apis/json-rpc/)
