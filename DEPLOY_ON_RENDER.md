# Deploy on Render (Recommended - FREE)

Render is the easiest free platform for deploying Flask applications. Perfect for your Habit Tracker!

## Prerequisites
- GitHub account (push your code there first)
- Free Render account (render.com)
- Your project files ready

## Step-by-Step Deployment

### 1. Push Code to GitHub

```bash
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"

git add .
git commit -m "Initial commit: Habit Tracker app ready for deployment"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/habit-tracker.git
git push -u origin main
```

### 2. Create Render Account
- Go to **render.com**
- Click "Sign Up"
- Sign up with GitHub for easier integration

### 3. Create Web Service

1. Click **"New +"** → **"Web Service"**
2. Select your GitHub repository
3. Fill in the details:
   - **Name**: `habit-tracker`
   - **Environment**: `Python 3`
   - **Region**: Select nearest to you
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app`

### 4. Add Environment Variables

In the "Environment" section, add:

```
FLASK_ENV=production
SECRET_KEY=<generate-with-python-c-import-secrets-print-secrets-token-hex-32>
DATABASE_PATH=/tmp/habits.db
```

**Generate SECRET_KEY:**
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

### 5. Deploy

Click the **"Deploy"** button and wait (usually 2-3 minutes)

### 6. Access Your App

Your app will be available at: `https://habit-tracker.onrender.com` (or custom name you chose)

---

## Important Notes for Render

⚠️ **Database Persistence Issue:**
- SQLite database stored in `/tmp/` will be cleared when app restarts
- **Solution 1 (Recommended)**: Upgrade to paid tier with persistent disk
- **Solution 2**: Use PostgreSQL add-on (free tier available)

### Upgrade to PostgreSQL (Recommended for Free)

1. In Render dashboard, click "New +" → "PostgreSQL"
2. Create a free instance
3. Update `appraju.py`:

```python
# Replace this line:
app.config['SQLALCHEMY_DATABASE_URI'] = f'sqlite:///{DATABASE_PATH}'

# With this:
import os
database_url = os.getenv('DATABASE_URL')
if database_url.startswith('postgres://'):
    database_url = database_url.replace('postgres://', 'postgresql://', 1)
app.config['SQLALCHEMY_DATABASE_URI'] = database_url
```

4. Add `psycopg2-binary==2.9.7` to `requirements.txt`
5. In Render, link the PostgreSQL database to your web service

---

## Monitoring and Logs

- Logs visible in Render dashboard
- Check for errors in "Events" tab
- Monitor resource usage

## Updating Your App

Just push changes to GitHub:
```bash
git add .
git commit -m "Update message"
git push origin main
```

Render automatically re-deploys!

---

## Troubleshooting

### Build fails with "gunicorn not found"
✅ Make sure `gunicorn==21.2.0` is in `requirements.txt`

### App crashes after deployment
- Check logs in Render dashboard
- Verify environment variables are set
- Ensure `appraju.py` is in root directory

### Database connection error
- Check DATABASE_PATH environment variable
- Verify /tmp/ directory permissions
- Consider switching to PostgreSQL

---

**Your app should be live in minutes! 🚀**
