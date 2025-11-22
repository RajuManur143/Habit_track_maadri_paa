# Deploy on PythonAnywhere (FREE - Best for Flask)

PythonAnywhere is specifically designed for Python apps and has excellent Flask support.

## Prerequisites
- GitHub account (optional, can upload files directly)
- Free PythonAnywhere account (pythonanywhere.com)

## Step-by-Step Deployment

### 1. Create PythonAnywhere Account
- Go to **pythonanywhere.com**
- Click "Sign Up"
- Choose **Free account**
- Complete registration

### 2. Upload Your Code

#### Option A: Upload via Web Interface
1. In PythonAnywhere, go to "Files"
2. Create folder: `habit-tracker`
3. Upload all your files:
   - `appraju.py`
   - `requirements.txt`
   - `.env.example`

#### Option B: Clone from GitHub
1. Go to "Bash" console
2. Run:
```bash
git clone https://github.com/YOUR_USERNAME/habit-tracker.git
cd habit-tracker
```

### 3. Create Virtual Environment

In PythonAnywhere Bash console:

```bash
mkvirtualenv --python=/usr/bin/python3.11 habit-tracker
pip install -r requirements.txt
pip install gunicorn
```

### 4. Create Web App

1. Click "Web" in top menu
2. Click "Add a new web app"
3. Choose "Manual configuration"
4. Select **Python 3.11**
5. Click "Next"

### 5. Configure Web App

In the Web App settings:

**Source code location:**
```
/home/your-username/habit-tracker
```

**Virtualenv path:**
```
/home/your-username/.virtualenvs/habit-tracker
```

**WSGI configuration file:**

Click the WSGI file link and replace content with:

```python
import sys
import os
from pathlib import Path

# Add project to path
project_home = '/home/your-username/habit-tracker'
if project_home not in sys.path:
    sys.path.insert(0, project_home)

# Set environment variables
os.environ['FLASK_ENV'] = 'production'
os.environ['SECRET_KEY'] = 'your-secure-key-here'

# Import and create Flask app
from appraju import app as application
```

### 6. Set Environment Variables

In PythonAnywhere Web app settings, add to the top of WSGI file:

```python
os.environ['FLASK_ENV'] = 'production'
os.environ['SECRET_KEY'] = 'your-generated-secret-key'
os.environ['DATABASE_PATH'] = '/home/your-username/habit-tracker/habits.db'
```

**Generate SECRET_KEY:**
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

### 7. Reload Web App

1. In Web tab, click **"Reload"** button
2. Wait for green checkmark
3. Your app is now live!

### 8. Access Your App

Your app will be at:
```
https://your-username.pythonanywhere.com
```

---

## Important Notes

### Database Storage
✅ Good news! PythonAnywhere has persistent file storage
- Your SQLite database will persist across reloads
- No need to upgrade to PostgreSQL

### File Upload Limits
- Free tier: 512MB storage
- Enough for SQLite database + app code

### Request Limits
- Free tier: 100 requests per day (plenty for testing)
- Resets daily at midnight UTC

---

## Managing Your App

### View Logs
1. Click "Web" 
2. Scroll down to "Log files"
3. Check error log if app crashes

### Restart App
1. Click "Web"
2. Click **"Reload"** button

### Update Code
1. Go to "Files"
2. Edit or re-upload files
3. Click "Reload" to restart

---

## If You Want Custom Domain

- Upgrade to Paid account (cheap!)
- Point domain to: `http://your-username.pythonanywhere.com`

---

## Free Account Limitations

| Feature | Free Tier |
|---------|-----------|
| Python versions | 2.7, 3.8, 3.9, 3.10, 3.11 ✅ |
| Web apps | 1 ✅ |
| Disk space | 512 MB ✅ |
| Requests/day | 100 (resets daily) |
| Always-on | ❌ (sleeps after 3 months) |
| Custom domain | ❌ |
| HTTPS | ✅ Free with pythonanywhere.com |

---

## Troubleshooting

### "ModuleNotFoundError: No module named 'flask'"
- Check virtual environment is activated
- Run: `pip install -r requirements.txt` in virtualenv

### "Permission denied" errors
- Use full paths in WSGI file
- Make sure file permissions are correct

### Database not persisting
- Check DATABASE_PATH points to correct location
- Verify file permissions in Files tab

### App won't start after reload
- Check error logs (Web → Log files)
- Verify WSGI file syntax
- Check SECRET_KEY is set

---

## Next Steps

1. ✅ Create account
2. ✅ Upload/clone code
3. ✅ Create virtual environment
4. ✅ Configure web app
5. ✅ Set environment variables
6. ✅ Reload
7. 🎉 Your app is live!

---

**Perfect for learning and production Python apps! 🚀**
