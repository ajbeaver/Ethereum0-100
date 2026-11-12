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

## 10.1.2 — Translate the interface into state ownership

Each deposit retains its original principal and parties even after settlement. Its terminal status records what happened; it does not mean the record should be erased. The aggregate active principal counts only active deposits. Unpaid credit is tracked per credit owner and summarized in the unpaid total.

Opening 7 wei creates an active obligation for 7. Releasing it changes which party may claim that amount without moving ETH out of the vault. Withdrawal later consumes that owner's credit and transfers the assets. Keeping these steps separate is what lets a recipient rejection fail one withdrawal without undoing the deposit's settlement history.

A successful open consumes the next ID; a reverted open should not leave a gap caused by committed state. Use the state's Missing value to reject unknown IDs, and do not equate a default mapping record with an active zero-value deposit. The positive-value rule and zero-state convention work together.

## 10.1.3 — Event contract

Use these event signatures and indexed fields. All other fields go into data.

| Event signature | Indexed fields | Meaning |
|---|---|---|
| `Opened(uint256,address,address,uint256,uint256)` | id, payer, payee | Data is amount and deadline |
| `Released(uint256,address,uint256)` | id, payee | Data is credited amount |
| `Refunded(uint256,address,uint256)` | id, payer | Data is credited amount |
| `Withdrawn(address,address,uint256)` | credit owner, recipient | Data is amount withdrawn |

Emit one corresponding event per successful operation. If withdrawal sends to a recipient that calls back into another state-changing path, event order must still permit faithful replay. Emit the withdrawal event after accounting effects and before the external call. A failed transfer reverts the event along with the accounting changes. State which functions may be called during a callback in your threat model.

## 10.1.4 — Decide what a callback is allowed to observe

Withdrawal clears the caller's credit and reduces the unpaid total before the external call. At that moment its accounting must already describe the post-withdrawal obligation. A nested withdrawal must be rejected according to the required protection, while other entry points must still preserve their own role, state, and accounting contracts.

Event ordering communicates this same causal sequence to the indexer. If the recipient callback legitimately opens a new deposit, that nested Opened event should occur after the outer withdrawal has already been journaled. Otherwise a replay consumer can see a different operation order from the ledger effects it is expected to reproduce.

A revert removes both the prepared withdrawal event and its accounting changes. Emitting before the external call therefore does not publish a successful payment from a failed transaction. This combines the earlier event-rollback and checks-effects-interactions models into one contract.

### Build an acceptance argument, not extra features

Map every table row to success, rejection, and unchanged-state tests. Include the exact deadline, repeated settlement, rejecting recipient, callback attempts, and independent users. Keep the required native-ETH scope, no history loops, and no global administrator. Adding upgrades or asset support would create new authority and accounting questions rather than improve completion of this specification.

The capstone requirements are now the authoritative interface. Earlier small examples are building blocks; inspect any reused code against these exact preconditions and event fields before treating it as compatible.

## Self-review

Why does settlement change liabilities without changing the vault balance? What event order should an indexer see when withdrawal calls a recipient that opens another deposit?

## Practice

Work through the [exercise specification](exercises/lesson01/lesson01.md). Continue with the next chapter in the [module index](README.md) after reviewing your work.

## Further reading

- [Solidity contracts and events](https://docs.soliditylang.org/en/latest/contracts.html)
