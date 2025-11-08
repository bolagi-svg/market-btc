# 🟧 MarketBTC — Bitcoin-Native Decentralized Marketplace

### A fully decentralized commerce protocol built on **Stacks**, enabling **trustless Bitcoin-settled** transactions, brand reputation, and on-chain auctions.

---

## 📘 Overview

**MarketBTC** is a Bitcoin-secured decentralized marketplace protocol powered by **Stacks**.
It enables merchants to create verified brands, list products, conduct direct or auction-based sales, and receive customer feedback — all while ensuring settlement through Bitcoin’s finality layer.

This smart contract suite establishes a **trust-minimized, brand-verifiable commerce system** designed for the next era of Bitcoin DeFi.

---

## ⚙️ System Overview

The MarketBTC protocol operates as a **self-contained marketplace contract** that governs:

* **Brand Registration & Verification** — Merchants register brands; admin verifies authenticity.
* **Product Listings** — Direct-sale or auction-based listings with transparent pricing and availability.
* **Auctions** — Fully on-chain bidding and escrow with automatic finalization and payout logic.
* **Customer Reviews** — Immutable, reputation-driven feedback stored directly on-chain.
* **Bitcoin Settlement** — All transfers occur through Stacks’ native STX, settling with Bitcoin security.

---

## 🧱 Contract Architecture

```
market-btc.clar
│
├── Constants
│   ├── contract-owner          → Contract deployer address
│   ├── platform-fee            → Global fee rate (2.5%)
│   ├── Error codes             → Standardized Clarity errors for validation
│
├── Data Variables
│   ├── product-counter         → Sequential ID generator for listings
│
├── Maps
│   ├── Brands                  → Brand registry
│   ├── Products                → Product metadata store
│   ├── Auctions                → Auction state, bids, and expiry tracking
│   ├── Reviews                 → Buyer feedback and reputation records
│
├── Public Functions
│   ├── Brand Management
│   │   ├── register-brand()
│   │   └── verify-brand()
│   │
│   ├── Direct Sales
│   │   ├── list-product()
│   │   └── purchase-product()
│   │
│   ├── Auctions
│   │   ├── create-auction()
│   │   ├── place-bid()
│   │   └── end-auction()
│   │
│   ├── Reviews
│   │   └── add-review()
│
└── Read-only Accessors
    ├── get-product()
    ├── get-brand()
    ├── get-auction()
    └── get-review()
```

---

## 🔄 Data Flow

**1. Brand Onboarding**
Merchants register a brand → Admin verifies → Brand can list products.

**2. Listing Flow**
A verified brand lists a product → Buyers can directly purchase (if non-auction) or place bids (if auction).

**3. Purchase / Settlement**
Upon purchase:

* Platform fee (2.5%) → contract owner
* Remaining amount → brand (merchant)
* Product marked unavailable

**4. Auction Flow**

* Seller creates auction → Bidders place increasing bids
* Highest bidder locked → Previous bidder refunded
* On expiry → funds distributed automatically, auction closed

**5. Reputation / Reviews**
Post-purchase, buyers can submit immutable on-chain reviews (1–5 stars + comment).

---

## 🧩 Key Design Features

| Component                    | Description                                                                    |
| ---------------------------- | ------------------------------------------------------------------------------ |
| **Bitcoin Settlement Layer** | Uses Stacks to anchor transactions to Bitcoin for finality.                    |
| **Trustless Escrow**         | Bids and purchases settle on-chain without intermediaries.                     |
| **Brand Verification**       | Admin-controlled verification ensures marketplace credibility.                 |
| **Transparent Fees**         | Flat 2.5% platform fee automatically applied to each sale.                     |
| **Composability**            | Other contracts can integrate to query listings or brands via read-only calls. |

---

## 🔐 Security & Trust Model

* **No centralized custody:** All transactions happen via Clarity STX transfers.
* **Immutable listings and reviews:** Product and feedback data are on-chain, auditable, and censorship-resistant.
* **Owner authority is minimal:** Only limited to brand verification.
* **Escrow via contract logic:** Ensures fair settlement between buyers and sellers.

---

## 🧠 Example Usage (Simplified)

```clarity
;; Register a brand
(contract-call? .market-btc register-brand "Satoshi Goods")

;; List a product for sale
(contract-call? .market-btc list-product "Trezor Case" "Hard shell BTC wallet case" u1000000)

;; Purchase a product
(contract-call? .market-btc purchase-product u1)

;; Create auction
(contract-call? .market-btc create-auction "Rare Bitcoin Art" "Unique NFT-backed art" u5000000 u20)

;; Place bid
(contract-call? .market-btc place-bid u1 u5500000)

;; End auction
(contract-call? .market-btc end-auction u1)
```

---

## 🧾 Read-Only Queries

```clarity
(contract-call? .market-btc get-brand tx-sender)
(contract-call? .market-btc get-product u1)
(contract-call? .market-btc get-auction u1)
(contract-call? .market-btc get-review u1 tx-sender)
```

---

## 🪙 Parameters

| Parameter              | Description                                    | Default      |
| ---------------------- | ---------------------------------------------- | ------------ |
| `platform-fee`         | Fee applied to each transaction (basis points) | `u25` (2.5%) |
| `min auction duration` | Minimum number of blocks per auction           | `u10`        |

---

## 🧭 Deployment Notes

* Deploy the contract via the Stacks CLI or Clarinet.
* Ensure `contract-owner` is set at deploy time (auto-assigned to `tx-sender`).
* Brands must be verified by the contract owner before listing products.
* Platform fees accumulate in the owner address and can fund DAO treasury or maintenance.

---

## 📄 License

This project is open-sourced under the **MIT License**.
Use freely, modify responsibly, and always **verify on-chain logic** before mainnet deployment.
