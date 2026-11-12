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

For this signature, the selector is unchanged across both inputs. The next 32 bytes encode the unsigned integer in big-endian order, left-padded with zeros. `42` ends in `2a`. `256` ends in `0100`. Calldata also admits zero bytes. Include them when counting encoded length.

## 2.3.2 — What the target receives

The target address is a transaction field. The address is separate from the calldata. Different contracts can interpret identical payloads differently. Four-byte selectors can collide. A match in a selector database is a clue. Use verified source, a trusted ABI, and the code at the intended address to understand behavior.

Dynamic arguments such as strings have a head word that points to data in the tail. For a function call, offsets in the encoded argument tuple are relative to the start of the argument encoding, immediately after the selector. The tail for a string stores a byte length and padded UTF-8 bytes. Try `cast calldata 'setLabel(string)' 'hi'` and identify the offset, length, and two data bytes.

## Practice

Work through the [exercise specification](exercises/lesson03/lesson03.md). Complete the [module assessment](assessment.md) once all three exercises are ready for review.

## Further reading

- [Solidity ABI specification](https://docs.soliditylang.org/en/latest/abi-spec.html)
