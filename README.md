# 🎓 National ExamChain - Decentralized Exam Distribution

Full-stack blockchain-based exam distribution platform with encrypted IPFS storage and NFT-based access control.

**Status:** ✅ Production Ready | **Version:** 1.0.0 | **Chain:** Polygon Amoy

## 📚 Documentation Index

Start here based on your role:

- 🚀 **[QUICKSTART.md](QUICKSTART.md)** - Quick deployment overview (5 min read)
- 📋 **[DEPLOYMENT.md](DEPLOYMENT.md)** - Complete step-by-step deployment guide (15 min read)
- ✅ **[CHECKLIST.md](CHECKLIST.md)** - Pre-deployment verification checklist
- 📝 **[SUBMISSION.md](SUBMISSION.md)** - Project features and technical details
- 👨‍⚖️ **[JUDGES_GUIDE.md](JUDGES_GUIDE.md)** - How to test and demo the app

## 🎯 Quick Facts

| Aspect | Details |
|--------|---------|
| **Architecture** | Smart contracts (Hardhat) + Client-side encryption + IPFS + Next.js API |
| **Security** | AES-256 encryption, SHA-256 key hashing, NFT-based access |
| **Chain** | Polygon Amoy Testnet |
| **Storage** | IPFS (Pinata) for encrypted files |
| **Database** | Prisma ORM with SQLite (PostgreSQL ready) |
| **Frontend** | Next.js 14 + React 18 + Tailwind CSS 4 |
| **Wallet** | MetaMask / WalletConnect integration |

---

# 📖 Original Setup Guide

Full-stack decentralized EdTech security platform:

- Smart contracts (Hardhat)
- Secure client-side encryption + IPFS upload
- Next.js dashboard with Wagmi + RainbowKit wallet integration

## 1) Install

```bash
npm install
```

## 2) Environment setup

Copy `.env.example` to `.env.local` and fill values:

```bash
cp .env.example .env.local
```

Required:

- `DATABASE_URL` (for local Prisma SQLite, default: `file:./prisma/dev.db`)
- `NEXT_PUBLIC_EDU_CONTRACT_ADDRESS`
- `NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID`
- `NEXT_PUBLIC_PINATA_JWT` (recommended)
- `NEXT_PUBLIC_PINATA_API_KEY`
- `NEXT_PUBLIC_PINATA_SECRET`
- `AMOY_RPC_URL` / `MUMBAI_RPC_URL`
- `DEPLOYER_PRIVATE_KEY`

Pinata note:

- If you use key/secret, the key must include pinning scope for `pinJSONToIPFS`.
- If you see `NO_SCOPES_FOUND`, create a new key/JWT in Pinata with pinning permissions.

## 3) Deploy contract

Local:

```bash
npm run deploy
```

Amoy:

```bash
npm run deploy:amoy
```

After deployment, set:

- `NEXT_PUBLIC_EDU_CONTRACT_ADDRESS`
- `CONTRACT_ADDRESS`

## 4) Grant roles (admin/examination centre)

Set in env:

- `ADMIN_WALLETS` (comma separated)
- `EXAM_CENTER_WALLETS` (comma separated)

Run:

```bash
npm run grant:roles -- --network amoy
```

Compatibility note:

- Legacy `TEACHER_WALLETS` and `STUDENT_WALLETS` are still accepted.

## 5) Start app

Initialize database once:

```bash
npm run db:generate
npm run db:push
```

Then start app:

```bash
npm run dev
```

Open `http://localhost:3000`.

## Useful scripts

- `npm run build` — Next production build
- `npm test` — Hardhat tests
- `npm run compile` — Hardhat compile
- `npm run db:generate` — Generate Prisma client
- `npm run db:push` — Create/update local SQLite schema

## Audit records

The app now stores:

- Admin uploads (`UploadRecord`) with CID, release time, wallet, tx hash, and content ID when resolved.
- Examination centre access events (`AccessRecord`) for successful downloads and denied/failed access attempts.

API endpoints:

- `POST /api/audit/uploads` and `GET /api/audit/uploads`
- `POST /api/audit/accesses` and `GET /api/audit/accesses`
