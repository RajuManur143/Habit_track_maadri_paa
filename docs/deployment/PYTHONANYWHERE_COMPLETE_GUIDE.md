# PythonAnywhere Complete Deployment Guide

## Overview
This guide walks you through deploying your Habit Tracker on PythonAnywhere step-by-step. PythonAnywhere is perfect because it's:
- ✅ Python-focused hosting
- ✅ Free tier available
- ✅ No Docker needed
- ✅ Built-in database support
- ✅ Simple deployment process

**Time Required:** 25-30 minutes
**Cost:** Free tier available

---

## Prerequisites Checklist

Before starting, make sure you have:
- [ ] GitHub account (you already have this)
- [ ] Your code pushed to GitHub (done: `Habit_track_maadri_paa`)
- [ ] 5 minutes to follow each step
- [ ] A text editor open for generating SECRET_KEY

---

## Step 1: Generate Your SECRET_KEY

This is a security key your Flask app needs.

**What to do:**
1. Open PowerShell on your computer
2. Run this command:
```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```

**What you'll see:**
```
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0
```

**What to do with it:**
- Copy this long string
- Save it in a notepad (you'll need it in Step 6)
- Never share it with anyone

✅ **Mark as complete:** You have your SECRET_KEY saved

---

## Step 2: Create PythonAnywhere Account

**What to do:**
1. Go to https://www.pythonanywhere.com
2. Click **"Sign Up"** button (top right)
3. Choose **Free account**
4. Enter your email (can be same as GitHub)
5. Create a password
6. Click **"Register"**

**What happens:**
- You'll get a confirmation email
- Click the confirmation link
- You'll be logged into PythonAnywhere

**What you'll see:**
- Dashboard with buttons and links
- "Web" tab on left sidebar
- "Consoles" tab on left sidebar

✅ **Mark as complete:** You're logged into PythonAnywhere

---

## Step 3: Link Your GitHub Account

**What to do:**
1. From PythonAnywhere dashboard, click **"Web"** on the left
2. Scroll down to **"Account"** section
3. Find **"Source code management"**
4. Click **"Link to GitHub"**
5. Authorize PythonAnywhere to access GitHub
6. You'll see your GitHub username

**What happens:**
- PythonAnywhere can now see your repositories
- You'll be able to deploy from GitHub directly

**What you'll see:**
- Your GitHub username displayed
- Success message

✅ **Mark as complete:** GitHub is linked

---

## Step 4: Create Web App

**What to do:**
1. In PythonAnywhere, click **"Web"** on left sidebar
2. Click **"Add a new web app"** button
3. Select **"Manual configuration"** (NOT bottle, NOT flask quick start)
4. Choose **"Python 3.10"** (or 3.11 if available)

**Important:** Select "Manual configuration" not the Flask quick start!

**What happens:**
- PythonAnywhere creates a web app structure
- You'll get a domain name like: `yourusername.pythonanywhere.com`

**What you'll see:**
- Your domain name at the top
- Configuration page with sections for:
  - Source code
  - Working directory
  - WSGI configuration
  - Environment variables

**Your domain will be:**
- `[yourusername].pythonanywhere.com`
- Example: `raju.pythonanywhere.com`

✅ **Mark as complete:** Web app is created

---

## Step 5: Clone Your Repository

**What to do:**
1. Click **"Consoles"** on left sidebar
2. Click **"Start a new Bash console"**
3. You'll see a terminal prompt like: `01:23 ~ $`
4. Copy and paste this command:
```bash
cd /home/yourusername
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
```

**Replace:** `yourusername` with your actual PythonAnywhere username

**What happens:**
- Your code downloads from GitHub
- You'll see output like:
```
Cloning into 'Habit_track_maadri_paa'...
remote: Enumerating objects: 15, done.
...
```

**What you'll see:**
- Command completed successfully
- Your code is now on PythonAnywhere servers

✅ **Mark as complete:** Code is cloned

---

## Step 6: Install Dependencies

**What to do:**
1. In the same Bash console, run:
```bash
cd /home/yourusername/Habit_track_maadri_paa
pip install --user -r requirements.txt
```

**What happens:**
- PythonAnywhere downloads and installs:
  - Flask
  - Flask-SQLAlchemy
  - Flask-WTF
  - Gunicorn (web server)
  - SQLAlchemy
  - Werkzeug

**What you'll see:**
- Progress bars for each package
- Final message: `Successfully installed ...`

**This takes:** 1-2 minutes

✅ **Mark as complete:** Dependencies installed

---

## Step 7: Configure Web App Settings

**What to do:**
1. Go back to **"Web"** tab
2. Click on your domain name (the one created in Step 4)
3. Scroll to **"Code"** section
4. In **"Source code:"** field, enter:
```
/home/yourusername/Habit_track_maadri_paa
```

5. In **"Working directory:"** field, enter:
```
/home/yourusername/Habit_track_maadri_paa
```

6. Scroll to **"Virtualenv"** section
7. Leave it EMPTY (we used --user flag in pip install)

**What you're doing:**
- Telling PythonAnywhere where your code is
- Setting the working directory

✅ **Mark as complete:** Paths are configured

---

## Step 8: Configure WSGI File

**What to do:**
1. Still in the **Web** tab, find **"WSGI configuration file"**
2. Click on the path (looks like `/var/www/...wsgi.py`)
3. A code editor will open
4. Delete ALL the code (select all, delete)
5. Paste this code:

```python
import sys
import os

# Add your project to the path
path = '/home/yourusername/Habit_track_maadri_paa'
if path not in sys.path:
    sys.path.append(path)

# Set environment variables
os.environ['FLASK_ENV'] = 'production'
os.environ['SECRET_KEY'] = 'YOUR_SECRET_KEY_HERE'
os.environ['DATABASE_PATH'] = '/home/yourusername/Habit_track_maadri_paa/habits.db'

# Import the Flask app
from appraju import app
application = app
```

**Important replacements:**
- Replace `yourusername` with your actual PythonAnywhere username (2 places)
- Replace `YOUR_SECRET_KEY_HERE` with the SECRET_KEY you generated in Step 1
- Keep the quotes around the secret key

**Example:**
```python
os.environ['SECRET_KEY'] = 'a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0'
```

6. Click **"Save"** button

**What happens:**
- PythonAnywhere loads this file when starting your app
- Flask app is imported and configured
- Environment variables are set

✅ **Mark as complete:** WSGI file is configured

---

## Step 9: Add Web App Environment Variables

**What to do:**
1. In the **Web** tab, scroll to **"Web app sections"**
2. Find **"Environment variables"**
3. Click **"Add a new environment variable"**
4. Add these 3 variables:

**Variable 1:**
- Name: `FLASK_ENV`
- Value: `production`

**Variable 2:**
- Name: `SECRET_KEY`
- Value: (paste your SECRET_KEY from Step 1)

**Variable 3:**
- Name: `DATABASE_PATH`
- Value: `/home/yourusername/Habit_track_maadri_paa/habits.db`

Replace `yourusername` with your actual username

**What you'll see:**
- Each variable appears as a row
- ✓ checkmark when saved

✅ **Mark as complete:** Environment variables are set

---

## Step 10: Initialize Database

**What to do:**
1. Click **"Consoles"** again
2. Click **"Start a new Bash console"**
3. Run these commands:

```bash
cd /home/yourusername/Habit_track_maadri_paa
python3 -c "from appraju import init_db; init_db()"
```

**What happens:**
- Your SQLite database is created
- Sample habits are loaded
- You'll see output like:
```
Database initialized successfully!
Sample habits loaded!
```

**What you'll have:**
- `habits.db` file in your project directory
- 4 sample habits ready to track

✅ **Mark as complete:** Database initialized

---

## Step 11: Reload Web App

**What to do:**
1. Go to **"Web"** tab
2. At the top, find the green **"Reload yourusername.pythonanywhere.com"** button
3. Click it

**What happens:**
- PythonAnywhere restarts your web app
- Your Flask app loads
- The database connects
- Your site goes live

**This takes:** 10-20 seconds

**What you'll see:**
- Button shows "Reloading..." then turns green
- You'll get a confirmation

✅ **Mark as complete:** App is reloaded and live!

---

## Step 12: Test Your App

**What to do:**
1. Click on your domain name at the top: `yourusername.pythonanywhere.com`
2. OR go to: `https://yourusername.pythonanywhere.com`
3. You should see the Habit Tracker interface

**Test these features:**
- [ ] Page loads without errors
- [ ] You can see "Your Habits" section
- [ ] You see 4 sample habits
- [ ] Click a checkbox to mark habit complete
- [ ] Click "+" to add a new habit
- [ ] Calendar view shows correctly
- [ ] Colors and emojis display

**If something's wrong:**
See "Troubleshooting" section below

✅ **Mark as complete:** App is tested and working!

---

## SUCCESS! 🎉

Your Habit Tracker is now live on PythonAnywhere!

**Your live URL:**
```
https://yourusername.pythonanywhere.com
```

**What's running:**
- ✅ Flask web server
- ✅ SQLite database
- ✅ All features working
- ✅ Accessible 24/7

**Share your URL with friends!**

---

## Updating Your App

When you make changes and push to GitHub:

**Option 1: Quick Update (Recommended)**
1. Go to Consoles
2. Start Bash console
3. Run:
```bash
cd /home/yourusername/Habit_track_maadri_paa
git pull origin main
```
4. Go to Web tab
5. Click Reload button

**Option 2: Full Update**
1. Delete the folder:
```bash
rm -rf /home/yourusername/Habit_track_maadri_paa
```
2. Repeat Step 5 (clone repository)
3. Repeat Step 6 (install dependencies)
4. Click Reload

---

## Troubleshooting

### Problem: "404 Not Found" or "Page doesn't exist"

**Cause:** App didn't start properly

**Fix:**
1. Go to Web tab
2. Click **"Error log"** button
3. Look for red error messages
4. Most common: wrong path in WSGI file
5. Check Step 8 - make sure username is correct

### Problem: "ImportError: No module named 'flask'"

**Cause:** Dependencies not installed

**Fix:**
1. Go to Consoles → Bash
2. Run:
```bash
cd /home/yourusername/Habit_track_maadri_paa
pip install --user -r requirements.txt
```
3. Go to Web → Reload

### Problem: "SECRET_KEY not found" or blank page

**Cause:** Environment variables not set

**Fix:**
1. Go to Web tab
2. Scroll to "Environment variables"
3. Check all 3 are present:
   - FLASK_ENV = production
   - SECRET_KEY = (your long string)
   - DATABASE_PATH = (correct path)
4. Click Reload

### Problem: Database errors / "Cannot write to database"

**Cause:** Database initialization failed

**Fix:**
1. Go to Consoles → Bash
2. Run:
```bash
cd /home/yourusername/Habit_track_maadri_paa
rm habits.db
python3 -c "from appraju import init_db; init_db()"
```
3. Go to Web → Reload

### Problem: App crashes after a few seconds

**Cause:** Usually SECRET_KEY issues

**Fix:**
1. Check SECRET_KEY has no spaces or special characters
2. Regenerate if needed:
```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```
3. Update in WSGI file (Step 8)
4. Update in Environment variables (Step 9)
5. Go to Web → Reload

### Problem: "ModuleNotFoundError" for appraju

**Cause:** Wrong path in WSGI or Source code

**Fix:**
1. Verify your username is correct (case-sensitive)
2. Verify folder name is exact: `Habit_track_maadri_paa`
3. Check Step 7 and Step 8 paths match exactly
4. Go to Web → Reload

---

## How to Check Logs

When something goes wrong:

1. Go to **Web** tab
2. Click **"Error log"** button
3. Look at the last entries (bottom)
4. Red text = errors
5. Yellow text = warnings
6. Green text = success

**Common log messages:**
```
✓ Loaded app module = Good!
✓ Listening on ... = Good!
✗ ModuleNotFoundError = Bad, fix imports
✗ FileNotFoundError = Bad, fix path
✗ ValueError = Bad, check environment variables
```

---

## Advanced: Upgrade to Paid Account

If you want to upgrade to paid:
1. Go to Account settings
2. Click "Billing"
3. Choose a paid plan
4. Benefits: More storage, more CPU, PostgreSQL support, custom domains

For this free tier, you get:
- 512 MB storage (enough for habits!)
- Shared CPU
- SQLite database
- `yourusername.pythonanywhere.com` domain

---

## Next Steps

### Option 1: Upgrade Database (Advanced)
If you want data to persist permanently, upgrade to PostgreSQL:
1. Create a Paid account or free tier MySQL
2. Update connection string in Flask app
3. Migrate database

### Option 2: Custom Domain (Advanced)
Add your own domain name:
1. Buy domain from GoDaddy, Namecheap, etc.
2. In Web tab, click "Add a domain"
3. Point DNS to PythonAnywhere
4. Takes 24 hours to activate

### Option 3: Share Your Success
- Share your live URL with friends
- Show your Habit Tracker working
- Get feedback on features

---

## Summary

| Step | Action | Time |
|------|--------|------|
| 1 | Generate SECRET_KEY | 1 min |
| 2 | Create account | 2 min |
| 3 | Link GitHub | 1 min |
| 4 | Create web app | 1 min |
| 5 | Clone repository | 2 min |
| 6 | Install dependencies | 2 min |
| 7 | Configure paths | 2 min |
| 8 | Configure WSGI | 3 min |
| 9 | Add env variables | 2 min |
| 10 | Initialize database | 2 min |
| 11 | Reload app | 1 min |
| 12 | Test app | 2 min |
| **TOTAL** | **Live app!** | **25-30 min** |

---

## Quick Reference Card

```
PythonAnywhere Account: yourusername
Live URL: https://yourusername.pythonanywhere.com
Project Folder: /home/yourusername/Habit_track_maadri_paa
Database File: /home/yourusername/Habit_track_maadri_paa/habits.db
WSGI File: /var/www/yourusername_pythonanywhere_com_wsgi.py

After updates:
1. git pull origin main (in console)
2. Click Reload button (in Web tab)
```

---

## Key Differences from Local

| Aspect | Local | PythonAnywhere |
|--------|-------|-----------------|
| **Code location** | C:\Users\... | /home/yourusername/... |
| **Database location** | Same folder | /home/yourusername/... |
| **URL** | localhost:5000 | yourusername.pythonanywhere.com |
| **Server** | Flask dev | Gunicorn |
| **Uptime** | While PC on | 24/7 |
| **Accessible from** | Only you | Everyone |

---

## You're Done! 🎉

Your Habit Tracker is deployed and live!

**Next time you update:**
1. Push to GitHub
2. Go to PythonAnywhere Console
3. `git pull origin main`
4. Click Reload
5. Done in 1 minute!

**Enjoy tracking habits! 📊**
