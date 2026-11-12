# Lesson 2.3 — ABI and calldata

**Course:** Ethereum 0–100, Edition 1  
**Module:** [02 — RPC and ABI](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Encode a function call from its canonical signature and arguments.
2. Locate a selector and static words in a calldata payload.
3. Explain why the target address and ABI are separate inputs.

## 2.3.1 — Turning names into bytes

The ABI defines how clients encode arguments, decode return values, and interpret logs. The first four bytes of ordinary function calldata are the first four bytes of Ethereum Keccak-256 applied to the canonical function signature. Parameter names and spaces are omitted. Use `uint256` as the canonical spelling for `uint`. Return types are excluded.

```bash
cast sig 'setValue(uint256)'
cast calldata 'setValue(uint256)' 42
cast calldata 'setValue(uint256)' 256
```

Compare the two payloads: the selector stays fixed, while the argument for 256 ends in `0100`. The following breakdown explains the word layout and its padding.

## 2.3.2 — Decode the static example by position

The ABI is a shared encoding agreement, not a textual function call sent to the network. For `setValue(uint256)`, the payload is 4 bytes of selector followed by one 32-byte word. It therefore contains 36 bytes, or 72 hexadecimal digits after `0x`. The transaction destination is not part of those 36 bytes.

| Region | Size | Purpose |
|---|---:|---|
| Selector | 4 bytes | Identifies the requested function signature |
| First argument | 32 bytes | Encodes the value in the specified integer type |

`42` is hexadecimal `2a`; zeros pad the rest of its big-endian word and count toward its encoded length. Changing the integer changes the argument bytes, while keeping the selector fixed. Changing the parameter type can change the selector even if the visible numerical argument stays the same. Names such as `newValue` do not participate in the canonical signature.

Return values use ABI type encoding too, but ordinary returned data does not begin with the function selector. A client needs the output schema to interpret it. This is why Cast's read syntax can include return types such as `value()(uint256)` even though those return types are absent from selector computation.

## 2.3.3 — What the target receives

The target address is a transaction field. The address is separate from the calldata. Different contracts can interpret identical payloads differently. Four-byte selectors can collide. A match in a selector database is a clue. Use verified source, a trusted ABI, and the code at the intended address to understand behavior.

For a dynamic argument, a head word points to data in a tail. Try `cast calldata 'setLabel(string)' 'hi'`, then trace the offset, length, and content as described next.

## 2.3.4 — Follow one dynamic offset

For `setLabel(string)` with the text `hi`, the argument portion has three words: an offset, a byte length, and padded content. The offset is 32 because the tail begins after the one-word head. The length is 2 because `hi` occupies two UTF-8 bytes. Its content begins with `6869` and is padded to the word boundary. Including the selector gives 100 bytes total.

Offsets are measured in bytes from the argument tuple's start, not in hexadecimal characters and not from the first byte of the selector. This distinction prevents a common four-byte decoding error. Strings containing non-ASCII characters can use more UTF-8 bytes than visible characters; the ABI length counts bytes.

The encoding is not self-describing. A 32-byte word does not announce whether it means an integer or an offset. Use a trusted schema for the intended target. Ethereum Keccak-256 also differs from the standardized SHA3-256 exposed as `hashlib.sha3_256`; use Cast here rather than substituting an almost-identical hash name.

## Self-review

Why does changing a return type not change an ordinary function selector? In the `hi` example, where does the offset begin counting and what does the length measure?

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity ABI specification](https://docs.soliditylang.org/en/latest/abi-spec.html)
