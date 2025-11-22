# Deploy on Railway (FREE - Easiest)

Railway is the absolute easiest platform - just connect GitHub and click deploy!

## Prerequisites
- GitHub account
- Free Railway account (railway.app)

## Step-by-Step Deployment

### 1. Push Code to GitHub

```bash
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"

git add .
git commit -m "Initial commit: Habit Tracker app"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/habit-tracker.git
git push -u origin main
```

### 2. Create Railway Account
- Go to **railway.app**
- Click "Login" → "Continue with GitHub"
- Authorize Railway access to your GitHub

### 3. Create New Project

1. Click **"New Project"**
2. Select **"Deploy from GitHub repo"**
3. Select your `habit-tracker` repository
4. Click **"Deploy"**

Railway automatically detects Python and installs from `requirements.txt`!

### 4. Add Environment Variables

In the Project settings:

1. Click on your service
2. Go to **"Variables"** tab
3. Add these variables:

```
FLASK_ENV=production
SECRET_KEY=<your-generated-key>
PORT=5000
```

**Generate SECRET_KEY:**
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

### 5. Set Start Command

In "Settings" tab, set:
- **Start Command**: `gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app`

### 6. View Your App

Once deployed, click "Open" to see your live app!

URL will be: `https://your-project-*.up.railway.app`

---

## Important: Database Persistence

⚠️ SQLite will reset on deploys. Choose one option:

### Option 1: Use Railway PostgreSQL (Recommended)
```bash
1. In Railway dashboard: "New" → "PostgreSQL"
2. Connect to your Python service
3. Railway auto-sets DATABASE_URL
4. Update requirements.txt to add: psycopg2-binary==2.9.7
5. Update appraju.py connection string
```

### Option 2: Keep SQLite (Data Loss)
- Works as-is
- Data resets on each deployment
- Fine for testing

---

## Getting Your App URL

1. Go to Railway dashboard
2. Click your project
3. In "Environments", click your service
4. Look for "Public URL" or "Domain"
5. Share that link with anyone!

Example: `https://habit-tracker-production-*.up.railway.app`

---

## Auto-Deploy from GitHub

Railway automatically re-deploys when you push to main:

```bash
git add .
git commit -m "Update feature"
git push origin main
```

Watch the deployment in Railway dashboard!

---

## Monitoring

- **Logs**: Click service → "Logs" tab
- **Metrics**: Click service → "Metrics" tab
- **Health**: Green indicator = running

---

## Troubleshooting

### "Module not found" errors
- Verify `requirements.txt` has all packages
- Check Python version (should auto-select 3.11)

### App won't start
- Check logs in Railway dashboard
- Verify environment variables are set
- Make sure `appraju.py` exists in root

### Can't connect to database
- Check DATABASE_PATH is writable
- Or switch to PostgreSQL add-on
- See section "Database Persistence" above

---

## Cost
- **FREE**: $5 free credit per month
- Most simple apps use way less than $5/month
- Perfect for development and testing!

---

**Your app will be live in minutes! 🚀**
