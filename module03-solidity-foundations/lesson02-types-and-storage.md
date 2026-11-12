# Lesson 3.2 — Types and storage

**Course:** Ethereum 0–100, Edition 1  
**Module:** [03 — Solidity Foundations](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Choose value and reference types for on-chain records.
2. Predict the behavior of a mapping’s default entry.
3. Distinguish storage references from memory copies.

## 3.2.1 — Model the record

Solidity has fixed-width integers, addresses, booleans, structs, enums, arrays, and mappings. Use integer wei for ETH amounts. Ordinary arithmetic in Solidity 0.8 checks overflow and underflow. A deliberate `unchecked` block disables those checks for its arithmetic. A narrower integer type also requires attention to explicit conversion, which can discard high bits.

```solidity
enum Status { Missing, Active, Settled }
struct Entry {
    uint256 amount;
    Status status;
}
mapping(uint256 => Entry) internal entries;
```

A mapping conceptually returns a default value for an unwritten key. For `Entry`, this is amount zero and status `Missing`, because the first enum member is zero. A mapping does not expose a list of keys. If the application needs history, define a discovery mechanism such as sequential IDs or creation events.

## 3.2.2 — Alias and copy

Inside a function, `Entry storage live = entries[id];` creates a reference to persistent state. A write to `live.amount` updates that entry. `Entry memory copy = entries[id];` copies the struct for temporary use. A write to `copy.amount` affects only that memory object. Read-only external inputs for reference types can often use `calldata`.

```solidity
Entry storage live = entries[id];
Entry memory preview = live;
preview.amount += 5;
live.amount += 2;
```

For an initial amount of 10, the preview holds 15 and the mapping holds 12. The semantics resemble parts of Python’s aliasing model, with explicit data locations and persistent state shared across transactions.

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity types and data locations](https://docs.soliditylang.org/en/latest/types.html)
