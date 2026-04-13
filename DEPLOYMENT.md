# 🚀 National ExamChain - Deployment Guide

## Project Status: ✅ READY FOR FINAL SUBMISSION

All components verified and tested. This guide will walk you through deploying to production.

---

## 📋 Pre-Deployment Checklist

- [x] All TypeScript files compile without errors
- [x] Prisma schema defined (UploadRecord + AccessRecord)
- [x] Smart contracts ready (EduAccessControl.sol)
- [x] API endpoints implemented (`/api/audit/*`)
- [x] Frontend components finalized
- [x] Error handling and validation complete
- [x] Database migrations prepared

---

## 🔧 Step 1: Environment Setup

### Create `.env.local` from template:
```bash
cp .env.example .env.local
```

### Fill required values in `.env.local`:

**Core Settings:**
```bash
DATABASE_URL="file:./prisma/dev.db"  # or use PostgreSQL for production
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID="get_from_walletconnect.com"
```

**IPFS (Pinata):**
- Get JWT from: https://pinata.cloud/keys
- API Key + Secret for pinning scope (if not using JWT)
```bash
NEXT_PUBLIC_PINATA_JWT="your_jwt_token"
NEXT_PUBLIC_PINATA_API_KEY="your_api_key"
NEXT_PUBLIC_PINATA_SECRET="your_secret"
```

**Blockchain:**
```bash
AMOY_RPC_URL="https://rpc-amoy.polygon.technology"
DEPLOYER_PRIVATE_KEY="0x..."  # 64 hex chars without leading 0x
ADMIN_WALLETS="0xAdmin1,0xAdmin2"
EXAM_CENTER_WALLETS="0xCentre1,0xCentre2"
```

---

## 💾 Step 2: Install & Initialize Database

```bash
# Install all dependencies
npm install

# Generate Prisma Client
npm run db:generate

# Create/sync database schema
npm run db:push
```

**Note:** First time setup creates `prisma/dev.db`. For production, switch to PostgreSQL:
```
DATABASE_URL="postgresql://user:pass@host:5432/edtech"
```

---

## 🔐 Step 3: Deploy Smart Contract

### Test locally first:
```bash
npm test
npm run compile
```

### Deploy to Amoy testnet:
```bash
npm run deploy:amoy
```

**Output will save:**
- Contract address
- Transaction hash
- Deployment receipt

### Copy contract address:
```bash
NEXT_PUBLIC_EDU_CONTRACT_ADDRESS="0x..."  # Add to .env.local
```

---

## 👥 Step 4: Grant Roles

### Configure admin & centre wallets:
Edit `.env.local`:
```bash
ADMIN_WALLETS="0x..."
EXAM_CENTER_WALLETS="0x..."
CONTRACT_ADDRESS="0x..."  # from step 3
```

### Grant roles on-chain:
```bash
npm run grant:roles -- --network amoy
```

---

## 🚀 Step 5: Build & Run

### Local testing:
```bash
npm run dev
# Open http://localhost:3000
```

### Production build:
```bash
npm run build
npm start
```

---

## 📦 Step 6: Deploy to Hosting

### Option A: Vercel (Recommended for Next.js)
```bash
npm install -g vercel
vercel
# Follow prompts, import .env.local values
```

### Option B: Self-hosted Server

**Prerequisites:**
- Node.js >= 18
- PostgreSQL (for production)

**Steps:**
```bash
# Clone/push to server
git clone <repo> && cd edtech-dapp

# Install & build
npm install
npm run db:push
npm run build

# Start with PM2 (recommended)
npm install -g pm2
pm2 start "npm start" --name "edtech"
pm2 save
pm2 startup
```

**Nginx reverse proxy example:**
```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Option C: Docker

**Dockerfile:**
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

**Build & run:**
```bash
docker build -t edtech-dapp .
docker run -p 3000:3000 \
  -e NEXT_PUBLIC_EDU_CONTRACT_ADDRESS=0x... \
  -e DATABASE_URL=postgresql://... \
  edtech-dapp
```

---

## ✅ Post-Deployment Verification

### 1. Check App Health
```bash
curl https://your-domain.com
# Should return HTML (200 OK)
```

### 2. Test Wallet Connection
- Visit app
- Connect MetaMask to Amoy network
- Should show your wallet address

### 3. Test Key Features
- [ ] Admin can upload exam papers
- [ ] Session key generates correctly
- [ ] Papers appear in Published Papers
- [ ] Centre can see released papers
- [ ] Download with decryption key works
- [ ] Papers move to History after download
- [ ] Audit logs recorded in database

### 4. Verify Database
```bash
# Local SQLite:
sqlite3 prisma/dev.db
SELECT COUNT(*) FROM "UploadRecord";
SELECT COUNT(*) FROM "AccessRecord";

# PostgreSQL:
psql postgresql://user:pass@host/dbname -c "SELECT COUNT(*) FROM \"UploadRecord\";"
```

---

## 🔍 Production Checklist

Before going live:

- [ ] Database backed up
- [ ] HTTPS enabled (free with Vercel/Let's Encrypt)
- [ ] Environment variables secured (no hardcoding)
- [ ] Rate limiting configured (if needed)
- [ ] Error monitoring set up (Sentry optional)
- [ ] Analytics enabled (optional)
- [ ] Smart contract verified on Polygonscan
- [ ] Role grants verified on-chain

---

## 🆘 Troubleshooting

### "Contract not found" error
```bash
# Ensure CONTRACT_ADDRESS is set and deployed
grep NEXT_PUBLIC_EDU_CONTRACT_ADDRESS .env.local
```

### Database connection fails
```bash
# Check PostgreSQL is running
psql postgresql://user:pass@host/dbname -c "SELECT 1"

# Or recreate SQLite
rm prisma/dev.db
npm run db:push
```

### MetaMask can't connect
- Ensure wallet is connected to Amoy network
- Check Amoy RPC is accessible
- Verify contract address is correct

### Upload fails with Pinata error
```bash
# Test Pinata JWT is valid
curl -H "Authorization: Bearer YOUR_JWT" https://api.pinata.cloud/data/testAuthentication
# Should return 200 with auth info
```

### Build fails
```bash
# Clean and rebuild
rm -rf .next node_modules
npm install
npm run build
```

---

## 📞 Support & Resources

- **Polygon Docs:** https://polygon.technology/developers
- **Pinata Docs:** https://docs.pinata.cloud
- **Next.js Docs:** https://nextjs.org/docs
- **Wagmi Docs:** https://wagmi.sh
- **Prisma Docs:** https://www.prisma.io/docs

---

## 🎉 Deployment Complete!

Your National ExamChain dapp is now live!

**Key Features Deployed:**
- ✅ Decentralized exam paper management
- ✅ Secure encryption & IPFS storage
- ✅ NFT-based access control
- ✅ Persistent audit logging
- ✅ Time-locked paper releases
- ✅ Wallet authentication

**Next Steps:**
- Monitor audit logs
- Collect user feedback
- Plan feature enhancements
- Consider mainnet migration

---

**Version:** 1.0.0  
**Last Updated:** April 13, 2026  
**Chain:** Polygon Amoy Testnet
