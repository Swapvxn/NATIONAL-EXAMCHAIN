# 🏆 National ExamChain - Hackathon Submission

## Project Overview

**National ExamChain** is a secure, decentralized exam distribution and access control platform built on Polygon blockchain with encrypted IPFS storage.

---

## ✨ Core Features Implemented

### 1. **Decentralized Exam Management**
- ✅ Smart contract (`EduAccessControl.sol`) for exam content management
- ✅ NFT-based access control (centres must hold access NFT)
- ✅ Time-locked paper releases (papers inaccessible until release time)
- ✅ Admin role-based access control

### 2. **Secure File Encryption**
- ✅ Client-side AES-256 encryption (files never stored unencrypted)
- ✅ SHA-256 hashing for decryption keys (keys never stored raw)
- ✅ Per-paper key validation (prevents single key from unlocking all papers)
- ✅ Encrypted file upload to IPFS via Pinata

### 3. **Wallet Integration**
- ✅ MetaMask/WalletConnect integration via Wagmi
- ✅ RainbowKit UI for seamless wallet connection
- ✅ Admin & Examination Centre role switching
- ✅ Automatic wallet permission checking

### 4. **Paper Management Dashboard**

#### Admin Features:
- ✅ Upload exam papers with encryption
- ✅ Set release timestamps
- ✅ View published papers with approval counts
- ✅ Grant NFT access to examination centres
- ✅ View upload history with transaction tracking
- ✅ Real-time upload progress (Encrypt → Upload → Register)
- ✅ Session key generation and secure display
- ✅ Error handling with user-friendly messages

#### Centre Features:
- ✅ Global decryption key input (once per session)
- ✅ View all released papers (from blockchain + persistent storage)
- ✅ Download button only enabled with valid key
- ✅ Invalid key detection and blocking
- ✅ Downloaded papers move to History section
- ✅ Persistent paper storage (papers never disappear)
- ✅ View download history with timestamps
- ✅ Access requirement indicators

### 5. **Audit & Compliance**
- ✅ Prisma ORM with SQLite database
- ✅ `UploadRecord` tracks all uploads (contentId, ipfsCid, releaseAt, adminWallet, txHash)
- ✅ `AccessRecord` tracks all access attempts (contentId, centreWallet, keyHash, status, timestamp)
- ✅ Status tracking: SUCCESS, DENIED_NO_NFT, DENIED_LOCKED, DENIED_NO_KEY, FAILED
- ✅ API endpoints for retrieving audit logs
- ✅ Secure logging of every download attempt

### 6. **Modern UI/UX**
- ✅ Glassmorphism design system (frosted glass effects)
- ✅ Dark theme with emerald/cyan/purple accent colors
- ✅ Smooth animations (Framer Motion)
- ✅ Responsive grid layout
- ✅ Loading states with skeletons
- ✅ Toast notifications (Sonner)
- ✅ Animated counters and progress bars
- ✅ Glow effects and hover interactions

### 7. **API Endpoints**
- ✅ `POST /api/audit/uploads` - Store upload records
- ✅ `GET /api/audit/uploads` - Retrieve upload history
- ✅ `POST /api/audit/accesses` - Log access attempts
- ✅ `GET /api/audit/accesses` - Retrieve access logs with filters
- ✅ `GET /api/audit/papers` - List all released papers persistently

### 8. **Security Features**
- ✅ Wallet connection required for sensitive operations
- ✅ Published papers hidden until wallet connected
- ✅ Per-paper key validation prevents unauthorized downloads
- ✅ Decryption failures logged and rejected
- ✅ NFT ownership validation on-chain
- ✅ Time-lock validation on-chain
- ✅ Key hash stored (never raw keys)

### 9. **Error Handling & Recovery**
- ✅ User-friendly error messages (not raw tech errors)
- ✅ Network error detection
- ✅ MetaMask rejection handling
- ✅ Insufficient funds detection
- ✅ IPFS timeout handling
- ✅ Visual error banners with details
- ✅ Automatic retry suggestions

---

## 📊 Technology Stack

### Frontend
- **Framework:** Next.js 14 (App Router)
- **UI Library:** React 18 + Tailwind CSS 4
- **Animations:** Framer Motion 11
- **Wallet:** Wagmi 2 + RainbowKit 2
- **Icons:** Lucide React
- **Notifications:** Sonner
- **Encryption:** crypto-js

### Backend
- **API:** Next.js API routes
- **Database:** Prisma 6.7 + SQLite (PostgreSQL ready)
- **Blockchain:** Hardhat 3, Polygon
- **Smart Contracts:** Solidity 0.8.28
- **Storage:** IPFS (Pinata)

### Deployment
- **Chain:** Polygon Amoy Testnet
- **RPC:** Amoy RPC
- **Deploy Tool:** Hardhat

---

## 🔐 Smart Contract Details

### EduAccessControl.sol
**Deployed Address:** `NEXT_PUBLIC_EDU_CONTRACT_ADDRESS`

**Key Functions:**
- `uploadContent(ipfsHash, releaseTime)` - Upload exam
- `hasContentAccess(contentId, wallet)` - Check NFT ownership
- `mintAccessNFT(centre, contentId)` - Grant access
- `getPendingContent(contentId)` - Retrieve exam metadata
- `getAllContentIds()` - List all exams

**Events:**
- `ContentUploaded(contentId, ipfsHash, releaseTime)`
- `ContentReleased(contentId)`
- `AccessGranted(centre, contentId, tokenId)`

---

## 📦 Database Schema

### `UploadRecord`
```
id          String (CUID)
createdAt   DateTime
contentId   String? (on-chain ID)
ipfsCid     String (IPFS hash)
releaseAt   DateTime
adminWallet String? (deployer address)
txHash      String? (blockchain tx)
chainName   String? (e.g., "polygon-amoy")
```
**Indexes:** createdAt, contentId, adminWallet

### `AccessRecord`
```
id              String (CUID)
createdAt       DateTime
contentId       String? (paper ID)
ipfsCid         String? (file hash)
centreWallet    String? (downloader address)
keyHash         String? (SHA256 of decryption key)
status          Enum (SUCCESS|DENIED_*)
message         String? (error details)
downloadedFile  String? (filename)
```
**Indexes:** createdAt, contentId, centreWallet, keyHash, status

---

## 🚀 Running the Project

### Development
```bash
npm install
npm run db:generate
npm run db:push
npm run dev
```
Open http://localhost:3000

### Building
```bash
npm run build
npm start
```

### Testing
```bash
npm run compile
npm test
```

### Deployment (See DEPLOYMENT.md for full guide)
```bash
npm run deploy:amoy
npm run grant:roles
# Deploy frontend to Vercel or self-hosted
```

---

## ✅ Quality Assurance

### Code Quality
- ✅ TypeScript strict mode (zero compilation errors)
- ✅ ESLint configured
- ✅ All API routes validated
- ✅ Error boundaries in place
- ✅ Input validation everywhere

### Testing Coverage
- ✅ Smart contract compiles without warnings
- ✅ All components render without errors
- ✅ API endpoints tested and working
- ✅ Encryption/decryption validated
- ✅ User flows tested end-to-end

### Security Audit
- ✅ Keys never logged (SHA-256 hashed)
- ✅ Private keys never stored on client
- ✅ HTTPS ready for production
- ✅ CORS configured properly
- ✅ Role-based access enforced

---

## 📈 Key Metrics

| Metric | Value |
|--------|-------|
| Smart Contract Lines | ~300 |
| Frontend Components | 4 main |
| API Endpoints | 5 |
| Database Tables | 2 |
| Encryption Standard | AES-256 |
| Audit Events Tracked | 5+ |
| Max Users Per Deploy | Unlimited |
| Avg Transaction Time | 30-60s |

---

## 🎯 Future Enhancements

### Phase 2 (Post-Hackathon)
- [ ] Multi-file exam bundles
- [ ] Real-time notifications
- [ ] Batch access grants
- [ ] Analytics dashboard
- [ ] Report generation
- [ ] Export audit logs to CSV

### Phase 3 (Scaling)
- [ ] Mainnet migration
- [ ] Layer 2 optimization
- [ ] Mobile app (React Native)
- [ ] Decentralized storage (Arweave)
- [ ] Governance tokens

---

## 📞 Support & Documentation

- **Setup:** See [README.md](README.md)
- **Deployment:** See [DEPLOYMENT.md](DEPLOYMENT.md)
- **Code:** All files documented with TypeScript
- **Smart Contracts:** See `contracts/EduAccessControl.sol`

---

## 👥 Team

Built for National Exam Chain Hackathon 2026

---

## 📄 License

ISC

---

## 🏁 Submission Checklist

- [x] Code compiles without errors
- [x] All features implemented and tested
- [x] Documentation complete
- [x] Deployment guide provided
- [x] Smart contract audited
- [x] Database schema finalized
- [x] UI/UX polished
- [x] Error handling comprehensive
- [x] Security measures in place
- [x] Ready for production deployment

---

**Status:** 🟢 READY FOR DEPLOYMENT  
**Last Updated:** April 13, 2026  
**Version:** 1.0.0
