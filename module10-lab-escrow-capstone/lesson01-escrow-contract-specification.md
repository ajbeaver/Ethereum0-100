# Lesson 10.1 — Escrow contract specification

**Course:** Ethereum 0–100, Edition 1  
**Module:** [10 — Lab Escrow Capstone](README.md)  
**Estimated study time:** 30–50 minutes

## Learning objectives

1. Translate a transition table into a small public interface.
2. Preserve exact principal and credit accounting across settlement.
3. Emit a complete event record for off-chain replay.

## 10.1.1 — Roles and interface

Build `src/LabEscrow.sol` in a new Foundry project. Deposit IDs start at 1 and increase by one on successful open. Each deposit stores payer, payee, amount in wei, deadline as a Unix timestamp, and state. Use Missing, Active, Released, and Refunded. Keep records queryable after settlement.

| Method | Preconditions | Effects |
|---|---|---|
| `open(address payee,uint256 deadline)` payable | Positive value, nonzero payee distinct from payer, deadline in the future | Create Active record, increase active principal, return ID |
| `release(uint256 id)` | Active, caller is payer, timestamp < deadline | Mark Released, move principal to payee credit |
| `refund(uint256 id)` | Active, caller is payer, timestamp >= deadline | Mark Refunded, move principal to payer credit |
| `withdraw(address payable recipient)` | Caller has positive credit and recipient is nonzero | Clear caller credit, reduce unpaid total, send exact credit and revert on failure |

Provide getters for deposit details, per-user credit, active principal, and unpaid credits. Use custom errors with clear semantics. Protect withdrawal from nested withdrawal, update effects before interaction, and review callbacks into all other entry points. Avoid a receive function, global admin, upgradeability, or loops over deposit history.

## 10.1.2 — Event contract

Use these event signatures and indexed fields. All other fields go into data.

| Event signature | Indexed fields | Meaning |
|---|---|---|
| `Opened(uint256,address,address,uint256,uint256)` | id, payer, payee | Data is amount and deadline |
| `Released(uint256,address,uint256)` | id, payee | Data is credited amount |
| `Refunded(uint256,address,uint256)` | id, payer | Data is credited amount |
| `Withdrawn(address,address,uint256)` | credit owner, recipient | Data is amount withdrawn |

Emit one corresponding event per successful operation. If withdrawal sends to a recipient that calls back into another state-changing path, event order must still permit faithful replay. Emit the withdrawal event after accounting effects and before the external call. A failed transfer reverts the event along with the accounting changes. State which functions may be called during a callback in your threat model.

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity contracts and events](https://docs.soliditylang.org/en/latest/contracts.html)
