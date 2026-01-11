# Git Repository - What to Include/Exclude

## ✅ MANDATORY FILES (Must Include)

### Core Source Code
- ✅ `backend/` - **REQUIRED** - All backend source code
- ✅ `dashboard/` - **REQUIRED** - Frontend React app
- ✅ `checkout/` - **REQUIRED** - Checkout service
- ✅ `checkout-widget/` - **REQUIRED** - SDK source code
- ✅ `docker-compose.yml` - **REQUIRED** - Docker configuration

### Configuration Files
- ✅ `.gitignore` - **REQUIRED** - Prevents committing unnecessary files
- ✅ All `package.json` files - **REQUIRED** - Dependencies
- ✅ All `Dockerfile` files - **REQUIRED** - Container builds
- ✅ `backend/src/db/schema.sql` - **REQUIRED** - Database schema

### Documentation
- ✅ `README.md` - **REQUIRED** - Main documentation

## 📝 RECOMMENDED (Good to Include)

- ✅ `IMPLEMENTATION_NOTES.md` - Helpful for understanding structure
- ✅ `backend/nginx.conf` - Configuration files
- ✅ `build-sdk.sh` / `build-sdk.bat` - Build scripts
- ✅ All source code files (`.js`, `.jsx`, `.css`, `.sql`)

## ⚠️ OPTIONAL (Can Include or Exclude)

- ⚠️ `TEST_RESULTS.md` - Testing documentation (optional)
- ⚠️ `FRONTEND_TESTING_GUIDE.md` - Testing guide (optional)
- ⚠️ `WEBHOOK_TESTING.md` - Webhook guide (optional)
- ⚠️ `GITHUB_SUBMISSION_GUIDE.md` - Submission guide (optional)
- ⚠️ `SUBMISSION_CHECKLIST.md` - Checklist (optional)
- ⚠️ `test-api.ps1` - Test script (optional)

## ❌ EXCLUDE (Automatically Ignored by .gitignore)

These are already in `.gitignore` and won't be committed:
- ❌ `node_modules/` - Dependencies (installed via npm install)
- ❌ `.env` files - Environment variables (sensitive data)
- ❌ `dist/` - Build outputs
- ❌ `build/` - Build outputs
- ❌ `*.log` - Log files
- ❌ `.DS_Store` - Mac system files

## 📋 Minimum Required Files Summary

**Absolute Minimum for Submission:**
1. ✅ All source code directories (backend/, dashboard/, checkout/, checkout-widget/)
2. ✅ docker-compose.yml
3. ✅ .gitignore
4. ✅ README.md
5. ✅ All package.json files
6. ✅ All Dockerfile files
7. ✅ Database schema (backend/src/db/schema.sql)

**Everything else is optional but recommended for clarity.**

## 🎯 Recommendation

**Include everything except:**
- Test/guide markdown files (optional - but harmless to include)
- Test scripts (optional - but harmless to include)

**The .gitignore already handles:**
- node_modules (won't be committed anyway)
- .env files (won't be committed anyway)
- Build artifacts (won't be committed anyway)

## ✅ Safe to Commit Everything

Since `.gitignore` is properly configured, you can safely:
```powershell
git add .
git commit -m "Payment Gateway Deliverable 2"
```

The `.gitignore` will automatically exclude:
- node_modules/
- .env files
- dist/ and build/ folders
- Log files

**So it's SAFE to commit everything - the .gitignore protects you!**
