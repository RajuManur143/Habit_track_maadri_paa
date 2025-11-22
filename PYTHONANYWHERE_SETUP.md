# PythonAnywhere Setup - Organized Project Structure

## ✅ Current Project Structure (Ready for Deployment)

```
Raju_habit_tracker/
├── 📁 app/                           ← MAIN APPLICATION FOLDER
│   ├── appraju.py                    ← Your Flask app (MAIN FILE)
│   ├── requirements.txt              ← Python dependencies
│   └── .gitignore                    ← Git ignore rules
│
├── 📁 docs/                          ← DOCUMENTATION (OPTIONAL)
│   ├── README.md                     ← Project overview
│   ├── 📁 deployment/                ← Deployment guides
│   │   ├── PYTHONANYWHERE_COMPLETE_GUIDE.md
│   │   ├── PYTHONANYWHERE_VISUAL_GUIDE.md
│   │   ├── PYTHONANYWHERE_TROUBLESHOOTING.md
│   │   ├── PYTHONANYWHERE_QUICK_START.md
│   │   ├── PYTHONANYWHERE_DEPLOYMENT_PACKAGE.md
│   │   ├── PYTHONANYWHERE_READY_TO_DEPLOY.md
│   │   ├── PYTHONANYWHERE_VS_RAILWAY_GUIDE.md
│   │   ├── RAILWAY_*.md (5 files)
│   │   ├── DEPLOYMENT_GUIDES_MASTER_INDEX.md
│   │   └── (other deployment guides)
│   │
│   └── (other documentation files)
│
├── 📁 venv/                          ← Virtual environment (optional local)
├── .git/                             ← Git repository
├── .gitignore                        ← Git ignore
├── Procfile                          ← Heroku/Railway config (reference)
├── Dockerfile                        ← Docker config (reference)
├── deploy.sh                         ← Deployment script (reference)
└── README.md                         ← Main README

```

---

## 🎯 For PythonAnywhere Deployment

### What PythonAnywhere Will See:

When you clone your repository on PythonAnywhere:

```bash
/home/yourusername/
└── Habit_track_maadri_paa/           ← Your cloned repo
    └── app/
        ├── appraju.py               ← YOUR MAIN APP FILE
        ├── requirements.txt         ← INSTALL THESE PACKAGES
        └── .gitignore
```

### PythonAnywhere Configuration:

**Web App Settings:**
- **Source code:** `/home/yourusername/Habit_track_maadri_paa/app`
- **Working directory:** `/home/yourusername/Habit_track_maadri_paa/app`
- **WSGI file path:** `/var/www/yourusername_pythonanywhere_com_wsgi.py`

**WSGI File Content (Important!):**
```python
import sys
import os

# Add app directory to path
path = '/home/yourusername/Habit_track_maadri_paa/app'
if path not in sys.path:
    sys.path.append(path)

# Environment variables
os.environ['FLASK_ENV'] = 'production'
os.environ['SECRET_KEY'] = 'your-secret-key-here'
os.environ['DATABASE_PATH'] = '/home/yourusername/Habit_track_maadri_paa/app/habits.db'

# Import Flask app
from appraju import app
application = app
```

---

## 🚀 PythonAnywhere Deployment Steps

### Step 1: Clone Repository
```bash
cd /home/yourusername
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
cd Habit_track_maadri_paa
```

### Step 2: Install Dependencies
```bash
cd /home/yourusername/Habit_track_maadri_paa/app
pip install --user -r requirements.txt
```

### Step 3: Configure PythonAnywhere Web App

In PythonAnywhere Web Tab:

1. **Source code:** `/home/yourusername/Habit_track_maadri_paa/app`
2. **Working directory:** `/home/yourusername/Habit_track_maadri_paa/app`
3. **Edit WSGI file** with paths from Step 1
4. **Add 3 environment variables:**
   - FLASK_ENV = production
   - SECRET_KEY = (generate with: python -c "import secrets; print(secrets.token_hex(32))")
   - DATABASE_PATH = /home/yourusername/Habit_track_maadri_paa/app/habits.db

### Step 4: Initialize Database
```bash
cd /home/yourusername/Habit_track_maadri_paa/app
python3 -c "from appraju import init_db; init_db()"
```

### Step 5: Reload Web App
- Go to Web tab
- Click "Reload yourusername.pythonanywhere.com"

### Step 6: Test
- Visit: https://yourusername.pythonanywhere.com
- Should see Habit Tracker with 4 sample habits

---

## 📁 Folder Purposes

### `/app` - APPLICATION CODE
**Contents:**
- `appraju.py` - Your Flask application (MAIN FILE)
- `requirements.txt` - Python package dependencies
- `.gitignore` - Git ignore rules
- `habits.db` - SQLite database (created at runtime)

**On PythonAnywhere:**
- This is where your code runs
- Database file created here
- All application code here

### `/docs` - DOCUMENTATION
**Contents:**
- Deployment guides for PythonAnywhere, Railway, Render, Fly.io
- Setup instructions
- Troubleshooting guides
- Reference documents

**On PythonAnywhere:**
- Not used at runtime
- Optional to clone
- For your reference

### Root Level Files
**Kept for reference:**
- `Procfile` - For Heroku/Railway (reference only)
- `Dockerfile` - For Docker (reference only)
- `deploy.sh` - Deployment script (reference only)
- `README.md` - Project overview
- `.git` - Git repository data

---

## ✅ What You Need on PythonAnywhere

**REQUIRED:**
- `/app/appraju.py` - Your Flask app
- `/app/requirements.txt` - Dependencies
- Environment variables (3 total)
- Database path configured

**OPTIONAL (for reference):**
- `/docs` folder with guides
- Other documentation files

**NOT NEEDED:**
- `venv` folder (PythonAnywhere doesn't use local venv)
- `Dockerfile` (unless using containerization)
- `Procfile` (unless using Heroku)

---

## 🎬 Quick Start on PythonAnywhere

### 1. Generate SECRET_KEY
```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```
Save the output!

### 2. Go to PythonAnywhere
- https://www.pythonanywhere.com
- Sign up with email
- Confirm email
- Login

### 3. Clone Your Repository
In Bash console:
```bash
cd /home/yourusername
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
cd Habit_track_maadri_paa
```

### 4. Install Dependencies
```bash
cd app
pip install --user -r requirements.txt
```

### 5. Create Web App
- Click Web tab
- Add new web app
- Manual configuration → Python 3.10

### 6. Configure Web App
- Source code: `/home/yourusername/Habit_track_maadri_paa/app`
- Working directory: `/home/yourusername/Habit_track_maadri_paa/app`
- Edit WSGI file (copy code from Step 3 above)
- Add 3 environment variables

### 7. Initialize Database
```bash
python3 -c "from appraju import init_db; init_db()"
```

### 8. Reload
- Click Reload button
- Wait 10-20 seconds

### 9. Test
- Visit: https://yourusername.pythonanywhere.com
- 🎉 Live!

---

## 📖 Need Help?

### For Detailed Steps:
- Open: `/docs/deployment/PYTHONANYWHERE_COMPLETE_GUIDE.md`
- Follow 12 detailed steps
- Takes 30 minutes

### For Visual Guide:
- Open: `/docs/deployment/PYTHONANYWHERE_VISUAL_GUIDE.md`
- ASCII diagrams & screenshots
- Same 12 steps with visuals

### For Troubleshooting:
- Open: `/docs/deployment/PYTHONANYWHERE_TROUBLESHOOTING.md`
- 10+ problems with solutions
- How to read error logs

### For Quick Reference:
- Open: `/docs/deployment/PYTHONANYWHERE_QUICK_START.md`
- Checklist format
- Quick facts

---

## 🔄 After Deployment

### To Update Your App

**Step 1:** Make changes locally
```
Edit appraju.py in VS Code
```

**Step 2:** Push to GitHub
```bash
git add .
git commit -m "Update: description"
git push origin main
```

**Step 3:** Pull on PythonAnywhere
```bash
cd /home/yourusername/Habit_track_maadri_paa
git pull origin main
```

**Step 4:** Reload
- Go to Web tab
- Click Reload button
- Takes 1 minute total

---

## ✨ Your App is Ready!

```
✅ Code organized in /app folder
✅ Dependencies in requirements.txt
✅ Documentation in /docs folder
✅ Git repository ready
✅ Ready to deploy on PythonAnywhere
✅ Will work 99% of the time if you follow steps
```

---

## 🎯 Next Steps

1. **Read:** `/docs/deployment/PYTHONANYWHERE_COMPLETE_GUIDE.md`
2. **Follow:** The 12 steps exactly
3. **Deploy:** Your app goes live in 30 minutes
4. **Enjoy:** Your Habit Tracker on the web! 🚀

---

**Everything is organized and ready for PythonAnywhere deployment!**

Good luck! 🎉
