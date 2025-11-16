# ✨ PromptPilot Pro

**AI-Powered Prompt Optimization Platform** - Transform vague prompts into crystal-clear, actionable AI instructions using Claude Sonnet 4.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Frontend](https://img.shields.io/badge/frontend-GitHub%20Pages-orange)
![Backend](https://img.shields.io/badge/backend-Railway-purple)

---

## 🚀 Live Demo

- **Frontend (GitHub Pages)**: [https://yourusername.github.io/promptpilot-pro/](https://yourusername.github.io/promptpilot-pro/)
- **Backend API (Railway)**: [https://promptpilot.up.railway.app](https://promptpilot.up.railway.app)

---

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Deployment Guide](#deployment-guide)
  - [Frontend (GitHub Pages)](#frontend-deployment-github-pages)
  - [Backend (Railway)](#backend-deployment-railway)
- [Local Development](#local-development)
- [API Documentation](#api-documentation)
- [Environment Variables](#environment-variables)
- [Contributing](#contributing)
- [License](#license)

---

## ✨ Features

### Core Features
- 🤖 **AI-Powered Optimization** - Uses Claude Sonnet 4 for intelligent prompt enhancement
- 📊 **Quality Scoring** - Detailed metrics (Clarity, Specificity, Structure)
- 📝 **Smart History** - Persistent local storage with 50-prompt limit
- 📈 **Analytics Dashboard** - Track success rates and improvement trends
- 📥 **Multi-Format Export** - JSON, Markdown, and TXT formats
- 🌓 **Dark/Light Theme** - Persistent theme preference
- ⚡ **Real-time Optimization** - 2-second average response time

### Accessibility & UX
- ♿ **WCAG 2.1 Compliant** - Full keyboard navigation and screen reader support
- ⌨️ **Keyboard Shortcuts** - `Ctrl+Enter` to optimize, `Ctrl+K` to focus
- 🎨 **Reduced Motion Support** - Respects user preferences
- 📱 **Fully Responsive** - Mobile-first design
- 🔄 **Offline Detection** - Graceful degradation when offline
- ⏱️ **Smart Rate Limiting** - Visual feedback and countdown timers

---

## 🛠️ Tech Stack

### Frontend
- **HTML5** - Semantic markup
- **Tailwind CSS** - Utility-first styling
- **Vanilla JavaScript** - No framework dependencies
- **Lucide Icons** - Beautiful SVG icons
- **LocalStorage** - Client-side persistence

### Backend
- **Python** - Core language
- **Anthropic Claude API** - AI model (Sonnet 4)
- **Railway** - Hosting platform

### Deployment
- **GitHub Pages** - Frontend hosting (free)
- **Railway** - Backend hosting (free tier available)
- **Custom Domain Support** - Optional

---

## 📁 Project Structure

```
promptpilot-pro/
├── index.html              # Main application (single-page app)
├── README.md               # This file
├── LICENSE                 # MIT License
├── .gitignore             # Git ignore rules
├── .github/
│   └── workflows/
│       └── deploy.yml     # Automated GitHub Pages deployment
└── backend/                # Backend code (separate repo recommended)
    ├── main.py
    ├── requirements.txt
    └── railway.json
```

---

## 🚀 Deployment Guide

### Frontend Deployment (GitHub Pages)

#### Option 1: Automatic Deployment (Recommended)

1. **Fork/Clone this repository:**
```bash
git clone https://github.com/yourusername/promptpilot-pro.git
cd promptpilot-pro
```

2. **Enable GitHub Pages:**
   - Go to repository **Settings** → **Pages**
   - Source: **Deploy from a branch**
   - Branch: **main** / **root**
   - Click **Save**

3. **Your site will be live at:**
```
https://yourusername.github.io/promptpilot-pro/
```

#### Option 2: GitHub Actions (Auto-deploy on push)

The included `.github/workflows/deploy.yml` automatically deploys on every push to `main`.

No configuration needed - just push your changes!

```bash
git add .
git commit -m "Update frontend"
git push origin main
```

### Backend Deployment (Railway)

#### Prerequisites
- Railway account ([railway.app](https://railway.app))
- Anthropic API key ([console.anthropic.com](https://console.anthropic.com))

#### Steps

1. **Create new Railway project:**
```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Initialize project
railway init
```

2. **Set environment variables in Railway dashboard:**
```env
ANTHROPIC_API_KEY=your_api_key_here
PORT=8000
```

3. **Deploy:**
```bash
railway up
```

4. **Get your backend URL:**
```
https://your-project.up.railway.app
```

5. **Update frontend API endpoint:**

Edit `index.html` line ~200:
```javascript
const BACKEND_URL = 'https://your-project.up.railway.app';
```

6. **Redeploy frontend** to GitHub Pages (automatic with Actions)

---

## 💻 Local Development

### Frontend

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/promptpilot-pro.git
cd promptpilot-pro
```

2. **Open in browser:**
```bash
# Using Python
python -m http.server 8000

# Using Node.js
npx http-server

# Or just open index.html in browser
open index.html
```

3. **Access at:**
```
http://localhost:8000
```

### Backend (Local Testing)

1. **Create virtual environment:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. **Install dependencies:**
```bash
pip install anthropic fastapi uvicorn python-dotenv
```

3. **Create `.env` file:**
```env
ANTHROPIC_API_KEY=your_api_key_here
```

4. **Run backend:**
```bash
uvicorn main:app --reload --port 8000
```

5. **Update frontend to use localhost:**
```javascript
const BACKEND_URL = 'http://localhost:8000';
```

---

## 📡 API Documentation

### Base URL
```
Production: https://promptpilot.up.railway.app
Local: http://localhost:8000
```

### Endpoints

#### `POST /optimize`

Optimize a user prompt using Claude Sonnet 4.

**Request:**
```json
{
  "input": "Write something good about AI"
}
```

**Response:**
```json
{
  "optimized": "As a technology analyst, write a comprehensive 500-word article...",
  "score": 9,
  "details": {
    "clarity": 9,
    "specificity": 9,
    "structure": 8,
    "domain": "Technology",
    "improvements": [
      "Added specific role context",
      "Defined clear output format",
      "Included measurable constraints"
    ]
  }
}
```

**Error Response:**
```json
{
  "error": "Rate limit exceeded. Please try again in 60 seconds."
}
```

#### `GET /health`

Health check endpoint.

**Response:**
```json
{
  "status": "ok",
  "version": "1.0.0"
}
```

---

## 🔐 Environment Variables

### Backend (Railway)

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `ANTHROPIC_API_KEY` | Your Anthropic API key | ✅ Yes | - |
| `PORT` | Server port | ❌ No | `8000` |
| `CORS_ORIGINS` | Allowed CORS origins | ❌ No | `*` |
| `RATE_LIMIT` | Requests per minute | ❌ No | `10` |

### Frontend

No environment variables needed! All configuration is in `index.html`.

To change the backend URL, edit line ~200:
```javascript
const BACKEND_URL = 'https://your-backend-url.com';
```

---

## 🎨 Customization

### Change Theme Colors

Edit the gradient in `index.html` (line ~50):
```css
.gradient-text {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

### Modify Storage Limit

Change the history limit (line ~350):
```javascript
localStorage.setItem(key, JSON.stringify(prompts.slice(0, 50))); // Change 50
```

### Update Meta Tags

Edit SEO meta tags (lines 7-27) for your domain and branding.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the repository**
2. **Create a feature branch:**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Commit your changes:**
   ```bash
   git commit -m "Add amazing feature"
   ```
4. **Push to branch:**
   ```bash
   git push origin feature/amazing-feature
   ```
5. **Open a Pull Request**

### Development Guidelines
- Follow existing code style
- Add comments for complex logic
- Test on multiple browsers
- Ensure accessibility (WCAG 2.1)
- Update documentation

---

## 🐛 Troubleshooting

### Common Issues

**❌ CORS Error**
```
Solution: Ensure your backend has CORS enabled for your GitHub Pages domain
```

**❌ API Key Invalid**
```
Solution: Check your ANTHROPIC_API_KEY in Railway environment variables
```

**❌ Rate Limit Exceeded**
```
Solution: Wait 60 seconds or upgrade your Anthropic API plan
```

**❌ Page Not Loading on GitHub Pages**
```
Solution: Check Settings → Pages → Branch is set to 'main'
```

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Anthropic** - For Claude Sonnet 4 API
- **Tailwind CSS** - For beautiful styling
- **Lucide** - For amazing icons
- **Railway** - For easy backend hosting
- **GitHub Pages** - For free frontend hosting

---

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/promptpilot-pro/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/promptpilot-pro/discussions)
- **Email**: support@promptpilot.pro

---

## 🗺️ Roadmap

- [ ] User authentication (Auth0/Clerk)
- [ ] Cloud sync across devices
- [ ] Team collaboration features
- [ ] API access for Pro users
- [ ] Chrome extension
- [ ] Mobile app (React Native)
- [ ] Prompt templates library
- [ ] A/B testing for prompts

---

## ⭐ Star History

If this project helped you, please consider giving it a ⭐ on GitHub!

---

**Made with ❤️ by [Oathan Rex](https://github.com/oathanrex)**


