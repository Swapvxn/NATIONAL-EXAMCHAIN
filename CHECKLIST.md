# ✅ Pre-Deployment Checklist

Use this checklist to verify your application is ready for final hackathon submission.

---

## 🔧 Environment & Setup

- [ ] `.env.local` created from `.env.example`
- [ ] All required environment variables filled:
  - [ ] `NEXT_PUBLIC_EDU_CONTRACT_ADDRESS` (contract deployed)
  - [ ] `NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID` (from WalletConnect)
  - [ ] `NEXT_PUBLIC_PINATA_JWT` or API keys configured
  - [ ] `DEPLOYER_PRIVATE_KEY` set
  - [ ] `ADMIN_WALLETS` configured
  - [ ] `EXAM_CENTER_WALLETS` configured
- [ ] Run verification: `npm run verify` or `.\verify.bat`

---

## 📦 Dependencies & Build

- [ ] Dependencies installed: `npm install`
- [ ] Prisma client generated: `npm run db:generate`
- [ ] Database initialized: `npm run db:push`
- [ ] Smart contracts compile: `npm run compile`
- [ ] No TypeScript errors: `npm run build`
- [ ] Local development starts: `npm run dev`
  - [ ] Opens at http://localhost:3000
  - [ ] No console errors

---

## 🔐 Smart Contract Deployment

- [ ] Contract compiles without warnings: `npm run compile`
- [ ] Contract deploys to Amoy: `npm run deploy:amoy`
  - [ ] Get contract address from output
  - [ ] Add to `.env.local` as `NEXT_PUBLIC_EDU_CONTRACT_ADDRESS`
- [ ] Roles granted: `npm run grant:roles -- --network amoy`
- [ ] Verify on Polygonscan: https://amoy.polygonscan.com/
  - [ ] Contract visible
  - [ ] ABI readable
  - [ ] Contract owner correct

---

## 🎨 Frontend Verification

### Home Page
- [ ] Landing page loads without errors
- [ ] "Connect Wallet" button visible
- [ ] MetaMask/RainbowKit modal works
- [ ] Routing works properly

### Admin Panel (After Wallet Connect)
- [ ] "Publish Exam Paper" section visible
- [ ] Upload area accepts files
- [ ] Release time picker works
- [ ] "Upload Paper" button functional
- [ ] Encryption progress shows
- [ ] Session key generates and displays
- [ ] Published Papers section shows papers
- [ ] Papers list updates after upload

### Centre Panel (After Wallet Connect)
- [ ] "Exam Access Cards" header shows
- [ ] Metrics display (Ready to Download, Downloaded)
- [ ] Global decryption key input works
- [ ] "Available Exam Papers" section shows released papers
- [ ] Papers show correct status (Ready/Locked)
- [ ] Download button disabled without key
- [ ] Download button enabled with key
- [ ] Invalid key shows error and disables button
- [ ] Valid key allows download
- [ ] Downloaded papers move to History
- [ ] History section shows downloaded papers

---

## 📊 Database & API

### Database
- [ ] SQLite database created or PostgreSQL connected
- [ ] Tables created: `UploadRecord`, `AccessRecord`
- [ ] Indexes created for performance
- [ ] No migration errors

### API Endpoints
- [ ] `POST /api/audit/uploads` - Upload records saved
- [ ] `GET /api/audit/uploads` - Upload history retrieved
- [ ] `POST /api/audit/accesses` - Access logs saved
- [ ] `GET /api/audit/accesses` - Access logs filtered correctly
- [ ] `GET /api/audit/papers` - Released papers listed

### Verify with curl:
```bash
# Check uploads
curl http://localhost:3000/api/audit/uploads

# Check papers
curl http://localhost:3000/api/audit/papers

# Should return JSON array
```

---

## 🔒 Security Features

### Wallet & Auth
- [ ] Wallet connection required for admin features
- [ ] Wallet connection required for centre features
- [ ] Published papers hidden when wallet disconnected
- [ ] Role-based access enforced

### Encryption
- [ ] Files encrypted with AES-256
- [ ] Keys hashed with SHA-256 (never stored raw)
- [ ] Decryption happens client-side only
- [ ] Failed decryption rejects with error

### Key Validation
- [ ] Single key cannot unlock multiple papers
- [ ] Wrong key shows "Invalid key for this paper"
- [ ] Invalid key attempt logged as DENIED_NO_KEY
- [ ] Button disables after invalid key attempt

### File Upload
- [ ] Files uploaded to IPFS via Pinata
- [ ] IPFS hash stored in database and contract
- [ ] Encrypted files cannot be manually downloaded

---

## ⚠️ Error Handling

### User-Friendly Messages
- [ ] MetaMask rejection shows helpful message
- [ ] Network errors show "Please check connection"
- [ ] Insufficient funds shows "Insufficient balance"
- [ ] IPFS errors show "Upload to IPFS failed"
- [ ] Timeout errors show "Operation timed out"

### Error Recovery
- [ ] Error banner displays with details
- [ ] Error banner can be dismissed
- [ ] Retrying after error works
- [ ] No raw technical errors shown

---

## 🎖️ UI/UX Polish

### Visual Design
- [ ] Glassmorphism theme applied
- [ ] Dark theme with emerald/cyan accents
- [ ] Smooth animations throughout
- [ ] Loading skeletons show during fetch
- [ ] Toast notifications appear correctly
- [ ] Responsive to different screen sizes

### Interactions
- [ ] Hover effects on buttons
- [ ] Click feedback visible
- [ ] Loading spinners show
- [ ] Transitions are smooth
- [ ] Mobile-friendly layout

---

## 📝 Documentation

- [ ] README.md complete and accurate
- [ ] DEPLOYMENT.md provides step-by-step guide
- [ ] SUBMISSION.md lists all features
- [ ] Code is well-documented
- [ ] All environment variables explained
- [ ] Smart contract functions documented

---

## 🚀 Production Readiness

### Code Quality
- [ ] No console.log() statements left (except intended)
- [ ] No TODO comments in production code
- [ ] All imports used and needed
- [ ] No unused variables
- [ ] TypeScript strict mode passes
- [ ] ESLint passes

### Performance
- [ ] Images optimized
- [ ] Fonts loaded efficiently
- [ ] API calls cached where appropriate
- [ ] Database queries indexed
- [ ] Bundle size reasonable

### Security
- [ ] No secrets in code (use .env)
- [ ] No hardcoded contract addresses
- [ ] No hardcoded private keys
- [ ] HTTPS ready
- [ ] CORS configured properly

---

## 🌐 Deployment Test

### Local Build
```bash
npm run build
npm start
```
- [ ] Build completes without errors
- [ ] Production build starts
- [ ] All features work in production mode

### Vercel/Hosting (Optional Testing)
- [ ] Can deploy to Vercel successfully
- [ ] Environment variables configured
- [ ] Domain accessible
- [ ] All features work on live URL

---

## 📋 Final Checklist

### Code
- [ ] All files committed to git
- [ ] No uncommitted changes
- [ ] `.gitignore` configured properly
- [ ] Node_modules not committed
- [ ] `.env.local` not committed (only `.env.example`)

### Documentation
- [ ] README.md updated
- [ ] DEPLOYMENT.md complete
- [ ] SUBMISSION.md lists features
- [ ] Comments in complex code
- [ ] Type definitions clear

### Testing
- [ ] Manual testing completed
- [ ] All user flows tested
- [ ] Error cases handled
- [ ] Different wallet addresses tested
- [ ] Multiple papers tested
- [ ] Download flow end-to-end verified

### Submission
- [ ] Project name clear
- [ ] Description in README accurate
- [ ] Contact/author info added
- [ ] License specified (ISC)
- [ ] All assets created/owned

---

## 🎯 Go/No-Go Decision

### Before You Deploy:

**Go if:**
- ✅ All checkboxes above are checked
- ✅ No errors in browser console
- ✅ No errors in terminal
- ✅ All features working as intended
- ✅ Database persisting data correctly

**No-Go if:**
- ❌ Any critical feature broken
- ❌ Unhandled errors in console
- ❌ Database not synced
- ❌ API endpoints returning 500 errors
- ❌ Smart contract not deployed

---

## 📞 If Something Breaks

Before submitting, check:

1. **Console Errors**
   - Open DevTools (F12)
   - Check Console tab for errors
   - Fix TypeScript errors

2. **Network Errors**
   - Check Network tab in DevTools
   - Verify API endpoints return data
   - Check IPFS connectivity

3. **Database Issues**
   - Verify `.env.local` has correct `DATABASE_URL`
   - Run `npm run db:push` again
   - Check `prisma/dev.db` exists

4. **Contract Issues**
   - Verify contract deployed: `npm run addresses`
   - Check contract address in `.env.local`
   - Ensure roles granted: `npm run grant:roles`

5. **Wallet Issues**
   - Switch to Amoy network in MetaMask
   - Disconnect and reconnect
   - Clear browser cache
   - Try different wallet address

---

## 🏁 Ready to Submit!

Once all checkboxes are complete:

1. **Create deployment package:**
   ```bash
   npm run build
   ```

2. **Document deployment:**
   - See DEPLOYMENT.md for step-by-step guide
   - Provide judges with deployment instructions
   - Include contract address

3. **Final verification:**
   - Run through checklist one more time
   - Test on fresh browser session
   - Verify all links work

4. **Submit:**
   - Upload code to GitHub/GitLab
   - Include README, DEPLOYMENT.md, SUBMISSION.md
   - Provide live demo link (if deployed)
   - Include contract address

---

**Last Updated:** April 13, 2026  
**Status:** Ready for Final Submission 🎉
