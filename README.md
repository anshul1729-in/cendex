# Cendex — Non-Custodial Solana DEX Swap Engine

> **"A non-custodial decentralized exchange aggregator built on Solana, featuring Jupiter API v6 route optimization, client-side VersionedTransaction signing, NextAuth session management, and Prisma PostgreSQL wallet state persistence."**

[![Next.js](https://img.shields.io/badge/Next.js-14_App_Router-black?style=flat-square&logo=next.js)](https://nextjs.org/)
[![Solana](https://img.shields.io/badge/Solana-Web3.js-9945FF?style=flat-square&logo=solana)](https://solana.com/)
[![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?style=flat-square&logo=prisma)](https://prisma.io/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)

---

## 🌟 Key Engineering Features

- **Jupiter Aggregator v6 Integration:** Fetches optimal multi-hop trade routing across all Solana Automated Market Makers (Raydium, Orca, Meteora, Phoenix) via the Jupiter Quote & Swap API with dynamic slippage configuration.
- **Client-Side VersionedTransaction Signing:** Deserializes raw base64 Solana `VersionedTransaction` payloads, binds local user keypair signatures, and serializes raw binary wire transactions.
- **Helius High-Throughput RPC Dispatch:** Broadcasts raw transactions directly to **Helius RPC nodes** with `skipPreflight` and automated transaction retry policies (`maxRetries = 2`) for rapid block commitment.
- **Custody-Free Wallet Key Management:** Generates and binds dedicated Solana keypairs per user session, enabling one-click token swaps directly from the web interface without requiring third-party browser extensions.
- **Full-Stack Next.js 14 Architecture:** Built with Next.js App Router, React Context token providers, NextAuth (Google OAuth), Prisma ORM, and PostgreSQL.

---

## 🏗️ Swap Execution Flow

```text
User initiates swap (e.g., SOL ──► USDC)
         │
         ▼
[ Next.js Client ]
         │ 1. Request Quote
         ▼
[ Jupiter v6 API (/v6/quote) ] ──► Returns optimal route across AMMs
         │
         ▼
[ Next.js Route Handler (/api/swap) ]
         │ 2. Request serialized transaction from Jupiter (/v6/swap)
         ├──► Receives Base64 VersionedTransaction payload
         │
         │ 3. Deserialize buffer into VersionedTransaction
         ├──► Sign transaction with user's Solana Keypair
         │
         │ 4. Broadcast via Helius RPC
         ▼
[ Helius Solana RPC ] ──► Broadcasts raw transaction to Solana Validators
         │
         ▼
[ Solana Blockchain ] ──► Transaction confirmed & committed to ledger
```

---

## 🛠️ Tech Stack

- **Framework:** Next.js 14 (App Router)
- **Blockchain:** `@solana/web3.js`, Jupiter Aggregator API (`/v6/swap`), bs58
- **Authentication:** NextAuth.js (Google Provider)
- **Database & ORM:** PostgreSQL, Prisma ORM
- **Styling:** Tailwind CSS

---

## 📂 Project Structure

```text
cendex/
├── app/
│   ├── api/
│   │   ├── auth/              # NextAuth route handlers
│   │   ├── swap/              # Jupiter swap creation & Helius transaction broadcast
│   │   └── tokens/            # Token price and balance querying
│   ├── components/            # Swap widget, token select modals, balance cards
│   ├── dashboard/             # User dashboard & transaction history
│   ├── lib/                   # Auth configurations, constants & token definitions
│   └── page.tsx               # Landing page & live swap interface
├── prisma/
│   └── schema.prisma          # User & SolWallet database models
├── .env.example               # Environment variables template
└── tailwind.config.ts         # Design tokens
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+)
- PostgreSQL database
- Helius RPC API key (or free public Solana RPC)
- Google OAuth Client ID & Secret

### 1. Clone & Install
```bash
git clone https://github.com/anshul1729-in/cendex.git
cd cendex
npm install
```

### 2. Configure Environment
Create a `.env` file from `.env.example`:
```bash
cp .env.example .env
```
Fill in your configuration:
```env
GOOGLE_CLIENT_ID="your_google_client_id"
GOOGLE_CLIENT_SECRET="your_google_client_secret"
DATABASE_URL="postgresql://user:password@localhost:5432/cendex"
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your_nextauth_secret"
HELIUS_RPC_URL="https://mainnet.helius-rpc.com/?api-key=your_helius_key"
```

### 3. Initialize Database
```bash
npx prisma db push
```

### 4. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.
