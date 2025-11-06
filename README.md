# PDF Signer Frontend - Deployment Repository

**⚠️ DO NOT EDIT THIS REPOSITORY DIRECTLY ⚠️**

This repository contains **pre-built static files** for deployment only.

## What is this?

This is the **deployment repository** for the PDF Signer web application. It contains:
- Pre-built static files from Vite (in `dist/`)
- A minimal `package.json` to serve those files
- Configuration for Railway hosting

## Source of Truth

The **actual source code** lives in:
- **Repository**: [`willackerly/pdf-signer-web`](https://github.com/willackerly/pdf-signer-web)
- **Branch**: `main-dev`
- **Location**: `packages/web/`

## Deployment Architecture

```
┌─────────────────────────────────────────┐
│  pdf-signer-web (main-dev)              │
│  - Source code                          │
│  - Development                          │
│  - Tests                                │
│  - pnpm monorepo                        │
└──────────────┬──────────────────────────┘
               │
               │ pnpm vite build
               ↓
┌─────────────────────────────────────────┐
│  /tmp/railway-deploy/                   │
│  - Staging area                         │
│  - Copy built dist/ here                │
└──────────────┬──────────────────────────┘
               │
               │ git push
               ↓
┌─────────────────────────────────────────┐
│  pdf-signer-frontend (THIS REPO)        │
│  - Pre-built static files               │
│  - Simple package.json                  │
│  - No build step needed                 │
└──────────────┬──────────────────────────┘
               │
               │ Railway auto-deploy
               ↓
┌─────────────────────────────────────────┐
│  https://pdf-signer-frontend            │
│  -production.up.railway.app/            │
│  - Live production site                 │
└─────────────────────────────────────────┘
```

## How Updates Work

### Manual Process:

1. **Build** in the main repo:
   ```bash
   cd /Users/will/Documents/GitHub/pdf-signer-web/packages/web
   pnpm vite build
   ```

2. **Copy** to staging:
   ```bash
   rm -rf /tmp/railway-deploy/dist/*
   cp -r dist/* /tmp/railway-deploy/dist/
   ```

3. **Push** to this repo:
   ```bash
   cd /tmp/railway-deploy
   git add dist/
   git commit -m "Update: [your changes]"
   git push
   ```

4. **Railway** automatically deploys (takes ~1 minute)

### Automated Process:

Use the deploy script in `pdf-signer-web`:
```bash
cd /Users/will/Documents/GitHub/pdf-signer-web
./scripts/deploy.sh
```

## Railway Configuration

- **Service**: pdf-signer-frontend
- **Project**: melodious-tenderness
- **Branch**: main
- **Build**: `npm install`
- **Start**: `npm start` (runs `serve dist -s -l 3000`)
- **Port**: 3000

## Live URL

https://pdf-signer-frontend-production.up.railway.app/

## What's Deployed

```
dist/
├── index.html              # Main HTML file
├── assets/
│   ├── index-*.js         # React app bundle
│   ├── index-*.css        # Compiled styles
│   └── pdf.worker-*.mjs   # PDF.js web worker
└── test-pdfs/             # Sample PDFs for testing
```

## Why Two Repositories?

The main `pdf-signer-web` repo is a **pnpm monorepo** with workspace dependencies that Railway struggles to deploy via CLI. This separate repo contains only the final build output, making deployment simple and reliable.

## Need to Make Changes?

👉 Go to [`willackerly/pdf-signer-web`](https://github.com/willackerly/pdf-signer-web)

See `DEPLOYMENT.md` in that repo for full documentation.

---

**Last Updated**: 2025-11-06
**Deployed From**: `pdf-signer-web` @ main-dev
