# 🧱 BitForge Protocol

**Bitcoin-Backed Liquidity Engine for the Stacks Ecosystem**

---

## Overview

**BitForge** is a **Bitcoin-native DeFi protocol** designed to unlock liquidity from dormant BTC holdings without requiring users to sell their Bitcoin. By combining **over-collateralized stablecoin minting**, **autonomous liquidity pools**, and **on-chain risk management**, BitForge bridges Bitcoin’s immutability with the programmability of Stacks Layer 2.

Through its vault-based architecture and built-in AMM, BitForge transforms Bitcoin into a yield-bearing asset while maintaining a fully decentralized, auditable, and composable financial infrastructure.

---

## ✨ Key Features

* **Over-Collateralized Stablecoin Minting**
  Users deposit BTC as collateral to mint USD-pegged stable tokens, maintaining exposure to BTC price movements while accessing liquid capital.

* **Bitcoin-Backed Liquidity Pools**
  Dual-sided AMM pools enable efficient swaps between BTC and stablecoins, generating yield through trading fees.

* **Dynamic Risk Management**
  Oracle-driven collateral ratios, liquidation thresholds, and built-in safety rails ensure stability and protect against market volatility.

* **Stacked Security Model**
  Leveraging **Stacks Layer 2**, every operation anchors to Bitcoin’s security guarantees, ensuring immutability and finality.

* **Composable Architecture**
  Designed for integration with other Stacks DeFi primitives and cross-chain bridges, enabling modular financial innovation.

---

## ⚙️ System Overview

The BitForge protocol consists of four primary subsystems:

| Subsystem                     | Function                                         | Key Operations                                      |
| ----------------------------- | ------------------------------------------------ | --------------------------------------------------- |
| **Vault System**              | Manages user BTC deposits and stablecoin minting | Deposit collateral, mint/burn stablecoins           |
| **Liquidity Engine**          | Automated market maker for BTC–stablecoin swaps  | Add/remove liquidity, swap assets                   |
| **Oracle Layer**              | Feeds real-time BTC/USD price data               | Owner updates oracle price                          |
| **Risk & Liquidation Module** | Ensures system solvency                          | Monitors collateralization and liquidation triggers |

---

## 🧩 Contract Architecture

```
BitForge Protocol
│
├── Initialization
│   ├── (initialize)
│   └── (update-price)
│
├── Vault Module
│   ├── (deposit-collateral)
│   ├── (mint-stablecoin)
│   ├── (burn-stablecoin)
│   └── (get-vault-details)
│
├── Liquidity Module
│   ├── (add-liquidity)
│   ├── (remove-liquidity)
│   ├── (get-pool-details)
│   └── (get-lp-details)
│
├── Oracle & Risk
│   ├── (update-price)
│   ├── (get-collateral-ratio)
│   └── (validate-price) [private]
│
└── Internal Helpers
    ├── (transfer-balance)
    ├── (calculate-collateral-ratio)
    ├── (calculate-lp-tokens)
    ├── (sqrt)
    └── (check-collateral-requirement)
```

---

## 🔒 Core Parameters

| Constant                   | Description                              | Value         |
| -------------------------- | ---------------------------------------- | ------------- |
| `MINIMUM-COLLATERAL-RATIO` | Required collateral ratio for minting    | 150%          |
| `LIQUIDATION-RATIO`        | Ratio threshold triggering liquidation   | 130%          |
| `POOL-FEE-RATE`            | AMM trading fee                          | 0.3%          |
| `PRECISION`                | Decimal precision for ratio calculations | 1e6           |
| `MINIMUM-DEPOSIT`          | Minimum BTC collateral                   | 0.01 BTC      |
| `MAX-MINT-AMOUNT`          | Cap on total stablecoin mint             | 10,000 USD    |
| `MAX-PRICE`                | Oracle price upper bound                 | 1,000,000 USD |

---

## 📊 Data Structures

### 1. **Collateral Vaults**

Stores per-user collateral and minting data.

```clarity
{ 
  btc-locked: uint, 
  stablecoin-minted: uint, 
  last-update-height: uint 
}
```

### 2. **Liquidity Providers**

Tracks user liquidity positions in the AMM pool.

```clarity
{
  pool-tokens: uint,
  btc-provided: uint,
  stable-provided: uint
}
```

### 3. **Protocol State**

| Variable              | Type | Description                         |
| --------------------- | ---- | ----------------------------------- |
| `oracle-price`        | uint | Current BTC/USD price               |
| `total-supply`        | uint | Circulating stablecoin supply       |
| `pool-btc-balance`    | uint | Total BTC in liquidity pool         |
| `pool-stable-balance` | uint | Total stablecoins in liquidity pool |

---

## 🔁 Example Data Flow

**Stablecoin Minting Flow:**

```
[User Wallet] → [Vault System] → [Oracle Validation] → [Stablecoin Mint]
     |                |                    |
  Deposit BTC   Verify 150% ratio     Mint stable tokens
```

1. **User deposits BTC** into their personal vault.
2. **Oracle** provides the BTC/USD price feed.
3. Protocol verifies the **collateralization ratio ≥ 150%**.
4. User mints stablecoins against their collateral.
5. Collateral and minting data are recorded on-chain.

---

## 🧮 Example Operations

**1. Initialize the Protocol**

```clarity
(contract-call? .bitforge initialize u65000) ;; Set initial BTC/USD price
```

**2. Deposit Collateral**

```clarity
(contract-call? .bitforge deposit-collateral u2000000)
```

**3. Mint Stablecoins**

```clarity
(contract-call? .bitforge mint-stablecoin u5000)
```

**4. Provide Liquidity**

```clarity
(contract-call? .bitforge add-liquidity u100000 u5000)
```

**5. Query Vault Details**

```clarity
(contract-call? .bitforge get-vault-details tx-sender)
```

---

## 🧠 Security & Design Considerations

* **Over-Collateralization:**
  Ensures system solvency and price stability under market stress.

* **Oracle Validation:**
  Prevents manipulation by enforcing value bounds (`validate-price`).

* **Permissioned Initialization:**
  Only the contract owner can initialize or update oracle data.

* **Slippage and Fee Protection:**
  AMM operations include bounds for precision and fair-value swaps.

* **Composability:**
  Designed to integrate with Stacks-based protocols (e.g., Arkadiko, ALEX) for enhanced liquidity routing.

---

## 🧰 Future Extensions

* **Automated Liquidation Module:**
  Enable third-party liquidators to maintain system health.

* **Cross-Chain BTC Collateralization:**
  Integration with Bitcoin L1 bridges (e.g., sBTC, XLink).

* **Dynamic Fee Markets:**
  Adaptive AMM fee adjustments based on volatility and liquidity depth.

* **Governance Layer:**
  Community-driven parameter tuning using a DAO model.

---

## 📜 License

This protocol is open-source under the **MIT License**.
Developers and researchers are encouraged to audit, extend, and integrate BitForge into the broader Bitcoin DeFi ecosystem.
