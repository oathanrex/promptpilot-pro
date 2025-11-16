# ⚡ Quick Start Guide - PromptPilot Pro

**Get your app deployed in 5 minutes!**

---

## 🎯 Deploy Frontend (2 minutes)

### 1️⃣ Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOURUSERNAME/promptpilot-pro.git
git push -u origin main
```

### 2️⃣ Enable GitHub Pages

1. Go to **Settings** → **Pages**
2. Select **main** branch
3. Click **Save**
4. ✅ Live at: `https://YOURUSERNAME.github.io/promptpilot-pro/`

---

## 🚂 Deploy Backend (3 minutes)

### 1️⃣ Get Anthropic API Key

1. Visit [console.anthropic.com](https://console.anthropic.com)
2. Create account / Login
3. Go to **API Keys** → **Create Key**
4. Copy your key: `sk-ant-api03-...`

### 2️⃣ Deploy to Railway

1. Visit [railway.app](https://railway.app) and login
2. Click **New Project** → **Deploy from GitHub**
3. Select your backend repo
4. Add environment variables:
   ```
   ANTHROPIC_API_KEY=sk-ant-api03-...
   PORT=8000
   ```
5. ✅ Get your URL: `https://YOUR-PROJECT.up.railway.app`

### 3️⃣ Connect Frontend to Backend

Edit `index.html` (line ~200):
```javascript
const BACKEND_URL = 'https://YOUR-PROJECT.up.railway.app';
```

Push changes:
```bash
git add index.html
git commit -m "Connect to backend"
git push
```

---

## 🎉 Done!

Your app is now live:
- **Frontend**: `https://YOURUSERNAME.github.io/promptpilot-pro/`
- **Backend**: `https://YOUR-PROJECT.up.railway.app`

### Test It:
1. Visit your frontend URL
2. Click **Launch App**
3. Enter a prompt: "Write something good"
4. Click **Optimize Prompt**
5. ✅ Should return optimized version!

---

## 🐛 Issues?

- **CORS Error**: Add your GitHub Pages URL to backend CORS settings
- **404 Error**: Wait 1-2 minutes for GitHub Pages to deploy
- **API Error**: Check Railway environment variables

---

## 📚 Next Steps

- Read [DEPLOYMENT.md](DEPLOYMENT.md) for detailed guide
- Customize theme colors in `index.html`
- Add custom domain (optional)
- Monitor usage in Railway dashboard

---

**Need help?** Open an issue on GitHub!
