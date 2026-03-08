# Zidly — Deployment Guide

## What Changed from ClosrAI

1. **All "ClosrAI" branding → "Zidly"** (navbar, footer, testimonials, email, copyright, component names)
2. **API calls secured** — frontend now calls `/api/chat` (Vercel serverless function) instead of Anthropic directly. Your API key is never exposed to the browser.
3. **Added `vercel.json`** — routes API calls to serverless function
4. **Added `/api/chat.js`** — Vercel serverless proxy that adds your API key server-side

## Files Modified
- `index.html` — title updated
- `src/main.jsx` — component name updated
- `src/App.jsx` — all branding + API endpoints updated
- `package.json` — project name updated

## Files Added
- `api/chat.js` — serverless API proxy (THIS IS CRITICAL — keeps your API key safe)
- `vercel.json` — Vercel routing config
- `.env.example` — shows required environment variable

---

## Deployment Steps (15 minutes)

### Step 1: Push to GitHub

Replace your existing repo contents or create a new repo:

```bash
# Option A: Update existing repo
cd your-demo-repo
# Replace all files with the new ones
git add .
git commit -m "Rebrand to Zidly + secure API proxy"
git push

# Option B: New repo
# Create new repo on GitHub, then:
cd zidly
git init
git add .
git commit -m "Initial Zidly launch"
git remote add origin https://github.com/YOUR_USERNAME/zidly.git
git push -u origin main
```

### Step 2: Deploy on Vercel

1. Go to **vercel.com** → Import your GitHub repo
2. Framework preset: **Vite** (Vercel should auto-detect)
3. Click **Deploy**

### Step 3: Add Your API Key (CRITICAL)

1. In Vercel dashboard → your project → **Settings** → **Environment Variables**
2. Add:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** your Anthropic API key (starts with `sk-ant-...`)
   - **Environment:** Production (and Preview if you want)
3. Click **Save**
4. **Redeploy** the project (Deployments → three dots → Redeploy)

### Step 4: Connect Your Domain (zidly.ai)

1. In Vercel dashboard → your project → **Settings** → **Domains**
2. Add: `zidly.ai`
3. Vercel will show you DNS records to add
4. Go to **Spaceship** (your registrar) → DNS settings for zidly.ai
5. Add the records Vercel tells you (usually an A record or CNAME)
6. Wait 5-30 minutes for DNS propagation
7. Vercel auto-provisions SSL (HTTPS)

---

## Testing

After deployment:

1. Visit `https://zidly.ai`
2. Enter any dental practice website URL in the demo
3. Verify the scanning animation works
4. Verify the chatbot responds with practice-specific info
5. Check that navbar says "Zidly" not "ClosrAI"
6. Check footer says "© 2025 Zidly"
7. Check CTA email link goes to alaa@zidly.ai

## Cost

- API calls go through your Anthropic account
- Each demo scan: ~$0.03-0.08
- Each chat message: ~$0.01-0.03
- Average demo session: ~$0.15-0.35

## Security Notes

- Your API key is ONLY on the server (Vercel environment variable)
- The browser never sees it
- The `/api/chat` proxy caps `max_tokens` at 1500 to prevent abuse
- Consider adding rate limiting later if you get heavy traffic
