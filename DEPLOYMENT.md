# 🚀 PromptPilot Pro - Deployment Guide

**Complete step-by-step deployment instructions for GitHub Pages + Railway**

---

## 📋 Prerequisites

Before deploying, make sure you have:

- ✅ GitHub account ([github.com](https://github.com))
- ✅ Railway account ([railway.app](https://railway.app))
- ✅ Anthropic API key ([console.anthropic.com](https://console.anthropic.com))
- ✅ Git installed on your computer

---

## 🌐 Part 1: Frontend Deployment (GitHub Pages)

### Step 1: Create GitHub Repository

```bash
# Navigate to your project folder
cd promptpilot-pro

# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit - PromptPilot Pro v1.0"

# Create repository on GitHub (via web interface)
# Then connect and push:
git remote add origin https://github.com/yourusername/promptpilot-pro.git
git branch -M main
git push -u origin main
```

### Step 2: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** (top menu)
3. Click **Pages** (left sidebar)
4. Under "Build and deployment":
   - Source: **Deploy from a branch**
   - Branch: **main** / **/(root)**
   - Click **Save**

### Step 3: Wait for Deployment

- GitHub will automatically deploy your site
- Check the **Actions** tab to see deployment progress
- Usually takes 1-2 minutes

### Step 4: Access Your Site

Your site will be live at:
```
https://yourusername.github.io/promptpilot-pro/
```

### Step 5: Custom Domain (Optional)

If you have a custom domain (e.g., `promptpilot.com`):

1. In **Settings** → **Pages** → **Custom domain**
2. Enter your domain: `promptpilot.com`
3. Click **Save**
4. Add CNAME record in your DNS:
   ```
   Type: CNAME
   Name: @
   Value: yourusername.github.io
   ```

---

## 🚂 Part 2: Backend Deployment (Railway)

### Step 1: Prepare Backend Code

Create a new folder `backend/` (or separate repository):

```
backend/
├── main.py
├── requirements.txt
├── railway.json (optional)
└── .env (local only, don't commit!)
```

**main.py** (example):
```python
from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
import anthropic
import os

app = FastAPI()

# CORS setup
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # In production, restrict this
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

class PromptRequest(BaseModel):
    input: str

@app.post("/optimize")
async def optimize_prompt(request: PromptRequest):
    client = anthropic.Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))
    
    # Your optimization logic here
    message = client.messages.create(
        model="claude-sonnet-4-20250514",
        max_tokens=2000,
        messages=[{
            "role": "user",
            "content": f"Optimize this prompt: {request.input}"
        }]
    )
    
    return {
        "optimized": message.content[0].text,
        "score": 9,
        "details": {
            "clarity": 9,
            "specificity": 9,
            "structure": 8,
            "domain": "General",
            "improvements": ["Enhanced clarity"]
        }
    }

@app.get("/health")
async def health():
    return {"status": "ok", "version": "1.0.0"}
```

**requirements.txt**:
```
fastapi==0.104.1
uvicorn==0.24.0
anthropic==0.25.0
python-dotenv==1.0.0
pydantic==2.5.0
```

**railway.json** (optional):
```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "uvicorn main:app --host 0.0.0.0 --port $PORT",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

### Step 2: Deploy to Railway

#### Option A: Using Railway Web Interface (Easiest)

1. Go to [railway.app](https://railway.app)
2. Click **"New Project"**
3. Select **"Deploy from GitHub repo"**
4. Authorize Railway to access your GitHub
5. Select your backend repository
6. Railway will auto-detect Python and deploy

#### Option B: Using Railway CLI

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Navigate to backend folder
cd backend

# Initialize project
railway init

# Link to new project
railway link

# Deploy
railway up
```

### Step 3: Set Environment Variables

In Railway Dashboard:

1. Click your project
2. Go to **Variables** tab
3. Add:
   ```
   ANTHROPIC_API_KEY=sk-ant-api03-...
   PORT=8000
   ```
4. Click **Save**

### Step 4: Get Your Backend URL

After deployment:
1. Go to **Settings** tab
2. Under **Domains**, click **Generate Domain**
3. Your URL will be: `https://your-project.up.railway.app`

### Step 5: Enable CORS (if needed)

Make sure your backend allows your frontend domain:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "https://yourusername.github.io",
        "https://your-custom-domain.com"
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

## 🔗 Part 3: Connect Frontend to Backend

### Step 1: Update Backend URL

Edit `index.html` (around line 200):

```javascript
const BACKEND_URL = 'https://your-project.up.railway.app';
```

### Step 2: Test Locally

```bash
# Open index.html in browser
open index.html

# Or use a local server
python -m http.server 8000
```

Try optimizing a prompt to verify the connection works.

### Step 3: Commit and Push

```bash
git add index.html
git commit -m "Update backend URL"
git push origin main
```

GitHub Actions will automatically redeploy your frontend!

---

## ✅ Verification Checklist

After deployment, verify everything works:

### Frontend (GitHub Pages)
- [ ] Site loads at `https://yourusername.github.io/promptpilot-pro/`
- [ ] Landing page displays correctly
- [ ] Theme toggle works
- [ ] Navigation works
- [ ] No console errors

### Backend (Railway)
- [ ] Backend is running (check Railway dashboard)
- [ ] Health check works: `https://your-backend.up.railway.app/health`
- [ ] CORS is configured correctly
- [ ] API key is set in environment variables

### Integration
- [ ] Clicking "Optimize Prompt" connects to backend
- [ ] Prompt optimization returns results
- [ ] Quality scores display correctly
- [ ] History saves to localStorage
- [ ] Export features work (JSON, Markdown, TXT)

---

## 🐛 Troubleshooting

### Issue: CORS Error

**Error:** `Access to fetch at 'https://...' from origin 'https://...' has been blocked by CORS policy`

**Solution:**
```python
# In main.py, update CORS origins:
allow_origins=["https://yourusername.github.io"]
```

### Issue: 404 Not Found on GitHub Pages

**Error:** Page shows 404

**Solution:**
1. Check **Settings** → **Pages** → Branch is set to `main`
2. Wait 1-2 minutes for deployment
3. Check **Actions** tab for build errors
4. Ensure `index.html` is in root directory

### Issue: Backend Not Responding

**Error:** `Failed to fetch` or timeout

**Solution:**
1. Check Railway dashboard - is service running?
2. Check Railway logs for errors
3. Verify `ANTHROPIC_API_KEY` is set correctly
4. Test health endpoint: `https://your-backend.up.railway.app/health`

### Issue: Rate Limit Errors

**Error:** `429 Too Many Requests`

**Solution:**
- Wait 60 seconds
- Check Anthropic API usage limits
- Upgrade Anthropic API plan if needed

---

## 🔄 Making Updates

### Update Frontend

```bash
# Make changes to index.html
git add index.html
git commit -m "Update feature X"
git push origin main

# GitHub Actions will auto-deploy
```

### Update Backend

```bash
# Make changes to main.py
cd backend
git add main.py
git commit -m "Update optimization logic"
git push origin main

# Railway will auto-deploy (if connected to GitHub)
# OR use Railway CLI:
railway up
```

---

## 📊 Monitoring

### GitHub Pages Status
- Check: **Settings** → **Pages**
- Build logs: **Actions** tab

### Railway Status
- Dashboard: [railway.app](https://railway.app)
- Logs: Click project → **Deployments** → View logs
- Metrics: CPU, Memory, Network usage

---

## 💰 Cost Breakdown

### GitHub Pages
- **Cost:** Free forever
- **Bandwidth:** 100GB/month
- **Storage:** 1GB
- **Build time:** 10 minutes per job

### Railway
- **Free Tier:** $5 credit/month (500 hours)
- **Pro Plan:** $20/month (unlimited)
- **Resources:** 8GB RAM, 8 vCPU

### Anthropic API
- **Free Tier:** Limited (check current limits)
- **Pay as you go:** ~$0.003 per 1K tokens (input)
- **Estimate:** 10,000 optimizations ≈ $15

---

## 🎯 Production Best Practices

### Security
- [ ] Use HTTPS only
- [ ] Restrict CORS to your domains
- [ ] Never commit API keys
- [ ] Use environment variables
- [ ] Add rate limiting

### Performance
- [ ] Enable Railway caching
- [ ] Minimize API calls
- [ ] Use CDN for static assets
- [ ] Compress images

### Monitoring
- [ ] Set up error tracking (Sentry)
- [ ] Monitor API usage (Anthropic console)
- [ ] Track uptime (UptimeRobot)
- [ ] Analytics (Plausible/GA)

---

## 🆘 Need Help?

- **GitHub Issues:** Report bugs or request features
- **Railway Discord:** [discord.gg/railway](https://discord.gg/railway)
- **Anthropic Support:** [support.anthropic.com](https://support.anthropic.com)

---

**Happy Deploying! 🚀**

*Last updated: January 2025*
