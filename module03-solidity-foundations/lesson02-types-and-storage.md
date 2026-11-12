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

## 3.2.2 — Defaults must have an application meaning

For a missing ID, the mapping returns the default struct values; it does not raise an exception saying “no such deposit.” Reserving enum value zero as `Missing` lets application code distinguish absent records from active ones. An amount of zero alone would be ambiguous if zero were otherwise a valid application value.

Mapping lookup also does not tell you which keys were ever written. A sequential ID scheme can support bounded discovery, while creation events support off-chain history. Neither requires adding an unbounded loop to every state-changing function.

### Integer widths express a domain

An unsigned `uint8` holds 0 through 255; `uint256` admits a much larger range. Checked arithmetic prevents an ordinary overflow from silently wrapping, but explicit narrowing is a different operation. Converting a value to a smaller unsigned width can discard high bits, so validate before converting when preservation matters.

For escrow amounts, use integer wei and avoid a narrow type selected only because a classroom example is small. The model should express the real supported amount domain. Later, the indexer must preserve that domain too: a correct Solidity amount can exceed SQLite's signed 64-bit integer range.

## 3.2.3 — Alias and copy

Inside a function, `Entry storage live = entries[id];` creates a reference to persistent state. A write to `live.amount` updates that entry. `Entry memory copy = entries[id];` copies the struct for temporary use. A write to `copy.amount` affects only that memory object. Read-only external inputs for reference types can often use `calldata`.

```solidity
Entry storage live = entries[id];
Entry memory preview = live;
preview.amount += 5;
live.amount += 2;
```

For an initial amount of 10, the preview holds 15 and the mapping holds 12. The semantics resemble parts of Python’s aliasing model, with explicit data locations and persistent state shared across transactions.

## 3.2.4 — Data location changes what assignment means

A **value type**, such as `uint256`, normally supplies a copied value when assigned. A **reference type**, such as a struct or array, also requires reasoning about where its data lives. `storage` persists between transactions; `memory` holds temporary data for the execution; `calldata` is the read-only input area for an external call's arguments.

In the fragment, `live` names the mapping entry itself. Updating it changes persistent storage if the transaction succeeds. `preview` receives a memory copy. Updating that copy does not write it back automatically. A function that computes a preview and forgets to assign it to storage may run successfully while retaining the old state.

The fragment belongs inside a function in a contract declaring `Entry` and `entries`; it is not a standalone Solidity file. Its assumed initial amount of 10 must be established by that contract's setup before comparing results. Treat these assumptions as part of the example rather than expecting an unwritten mapping key to begin at 10.

## Self-review

Why does changing a memory preview not update a mapping? What additional rule makes a default mapping record recognizable as missing?

## Practice

Work through the [exercise specification](exercises/lesson02/lesson02.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity types and data locations](https://docs.soliditylang.org/en/latest/types.html)
