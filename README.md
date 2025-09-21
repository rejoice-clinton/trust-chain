# TrustChain Protocol

**Advanced Decentralized Trust Scoring & Identity Verification for the Bitcoin Layer 2 Ecosystem**

TrustChain provides a decentralized, cryptographically-verifiable framework for measuring participant reliability across Bitcoin Layer 2 networks. It establishes **immutable trust profiles** that evolve based on verifiable on-chain activities, enabling **reputation portability** across Lightning Network routing nodes, Bitcoin-backed lending protocols, decentralized exchanges, and any application requiring verified participant trustworthiness—while preserving **privacy and user sovereignty**.

---

## 🌐 System Overview

| Feature                     | Description                                                                                                      |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Decentralized Identity**  | Each participant registers a DID (Decentralized Identifier) bound to their Stacks principal.                     |
| **Dynamic Trust Scoring**   | Multi-dimensional scoring with configurable multipliers for different on-chain actions.                          |
| **Reputation Decay**        | Automatic, periodic trust-score decay to reflect time-based reliability.                                         |
| **Auditability**            | Complete on-chain reputation history for transparency and cryptographic proof.                                   |
| **Portability**             | Trust profiles are reusable across DeFi protocols, Lightning Network interactions, and P2P Bitcoin transactions. |
| **Administrative Controls** | Configurable system parameters and action definitions controlled by contract owner.                              |

TrustChain is ideal for:

* **Lightning Network nodes**: reward consistent routing and uptime.
* **Bitcoin-collateralized lending**: assess borrower trustworthiness.
* **Decentralized exchanges**: mitigate counterparty risk without centralized KYC.

---

## ⚙️ Contract Architecture

The protocol is implemented as a single **Clarity smart contract** on the Stacks blockchain.

### Key Components

* **State Variables**

  * `contract-owner` – Principal with administrative privileges.
  * `contract-active` – Global enable/disable switch.
  * `decay-rate` / `decay-period` – Parameters controlling automatic reputation decay.
  * `starting-reputation` – Initial trust score for new identities.

* **Core Maps**

  * `identities` – Registry of participant profiles and reputation scores.
  * `reputation-actions` – Configurable action types with multipliers and descriptions.
  * `reputation-history` – Immutable log of all score changes and decay events.

* **Error Constants**
  Standardized error codes for consistent failure handling (e.g., `ERR-IDENTITY-NOT-FOUND`, `ERR-UNAUTHORIZED`).

---

## 🔄 Data Flow

1. **Identity Creation**

   * A user calls `create-identity` with a valid DID.
   * Contract stores the identity with the configured `starting-reputation`.

2. **Action Registration (Admin)**

   * Owner defines valid trust-earning actions using `add-reputation-action` or `update-reputation-action`.

3. **Reputation Update**

   * A participant performs a verifiable action (e.g., Lightning routing).
   * Contract verifies the action type, checks decay requirements, applies decay if needed, then increments the score using the action’s multiplier.

4. **Decay Mechanism**

   * Time-based decay automatically lowers trust if the required number of blocks (`decay-period`) has passed.
   * Can be triggered manually with `decay-reputation` or automatically during score updates.

5. **Verification & Query**

   * External protocols call `verify-reputation` or `get-reputation` to check if a user meets a minimum threshold or to retrieve full trust profile data.

---

## 🛠️ Administrative Functions

| Function                                             | Purpose                                                                                   |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `set-contract-owner`                                 | Transfer ownership to a new principal.                                                    |
| `set-contract-active`                                | Pause/resume contract operations.                                                         |
| `set-decay-parameters`                               | Update decay rate (0–100%) and block period.                                              |
| `set-starting-reputation`                            | Adjust initial score for new identities.                                                  |
| `add-reputation-action` / `update-reputation-action` | Manage action types, multipliers, and descriptions.                                       |
| `initialize-reputation-actions`                      | Preload standard Bitcoin ecosystem actions (e.g., Lightning routing, BTC loan repayment). |

---

## 📊 Read-Only Query Interface

* `get-reputation (principal)` – Returns current trust score.
* `get-full-identity (principal)` – Full profile including DID, score, timestamps, and activity metrics.
* `verify-reputation (principal, threshold)` – Boolean check if participant meets a minimum reputation requirement.
* `get-reputation-action (action-type)` – Retrieves action configuration.
* `get-reputation-history (principal, tx-id)` – Fetches immutable audit records.
* `get-contract-parameters` – Returns all current configuration parameters.

---

## 🏗️ Deployment & Initialization

1. **Deploy the Contract**
   Deploy the Clarity contract to the Stacks blockchain.

2. **Initialize Actions**
   Call `initialize-reputation-actions` to bootstrap common Bitcoin Layer 2 trust actions:

   * `lightning-routing`
   * `btc-lending-repay`
   * `layer2-validation`
   * `channel-maintenance`
   * `protocol-governance`

3. **Set Optional Parameters**
   Adjust decay rate/period or starting reputation as needed via admin functions.

---

## 🔒 Security & Design Considerations

* **User Sovereignty** – Identities are tied to user principals; no external custodian.
* **Immutable Audit Trail** – All reputation changes logged on-chain.
* **Dynamic Decay** – Prevents inactive users from maintaining perpetual trust.
* **Permissioned Administration** – Only the contract owner can modify core parameters or actions.

---

## 📜 License

MIT License – open for community collaboration and integration into broader Bitcoin Layer 2 ecosystems.

---

**TrustChain** brings a **trust layer** to the Bitcoin second layer, enabling **privacy-preserving reputation** and **secure identity verification** without central authorities.
