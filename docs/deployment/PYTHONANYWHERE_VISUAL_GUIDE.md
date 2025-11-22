# PythonAnywhere Visual Guide & Step-by-Step Walkthrough

## Visual Deployment Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    YOUR DEPLOYMENT JOURNEY                  │
│                                                              │
│  START                                                      │
│   │                                                         │
│   ├─→ [1] Generate SECRET_KEY (1 min)                      │
│   │   └─→ Copy long security string                       │
│   │                                                         │
│   ├─→ [2] Create PythonAnywhere Account (2 min)            │
│   │   └─→ Sign up with email                              │
│   │                                                         │
│   ├─→ [3] Link GitHub (1 min)                             │
│   │   └─→ Authorize PythonAnywhere                        │
│   │                                                         │
│   ├─→ [4] Create Web App (1 min)                          │
│   │   └─→ Choose Python 3.10 + Manual config             │
│   │                                                         │
│   ├─→ [5] Clone Repository (2 min)                        │
│   │   └─→ git clone your GitHub repo                     │
│   │                                                         │
│   ├─→ [6] Install Dependencies (2 min)                    │
│   │   └─→ pip install -r requirements.txt                │
│   │                                                         │
│   ├─→ [7] Configure Paths (2 min)                         │
│   │   └─→ Set source code & working dir                  │
│   │                                                         │
│   ├─→ [8] Configure WSGI (3 min)                          │
│   │   └─→ Paste WSGI code with your settings             │
│   │                                                         │
│   ├─→ [9] Add Environment Variables (2 min)               │
│   │   └─→ FLASK_ENV, SECRET_KEY, DATABASE_PATH           │
│   │                                                         │
│   ├─→ [10] Initialize Database (2 min)                    │
│   │   └─→ Create habits.db with sample data              │
│   │                                                         │
│   ├─→ [11] Reload Web App (1 min)                         │
│   │   └─→ Click green "Reload" button                    │
│   │                                                         │
│   └─→ [12] Test Your App (2 min)                          │
│       └─→ Visit yourusername.pythonanywhere.com ✅         │
│                                                              │
│  DONE! 🎉 Total: 25-30 minutes                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Dashboard Overview

### When you log into PythonAnywhere, you'll see:

```
PythonAnywhere Dashboard
┌────────────────────────────────────────────────────────────┐
│                                                            │
│  Left Sidebar:                    Main Area:              │
│  ┌──────────────┐                ┌────────────────────┐  │
│  │ Dashboard    │                │  [Account] [Web]   │  │
│  │ Web ✓        │                │  [Consoles]        │  │
│  │ Consoles     │                │  [Files]           │  │
│  │ Files        │                │                    │  │
│  │ Account      │                │  Your Web Apps:    │  │
│  │ Help         │                │  yourusername...   │  │
│  └──────────────┘                └────────────────────┘  │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## Step-by-Step Visual Walkthrough

### STEP 1: Generate SECRET_KEY

**Screen you see:**
```
PowerShell / Command Prompt
─────────────────────────────────────────
C:\Users\YourName> python -c "import secrets; print(secrets.token_hex(32))"
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6
C:\Users\YourName> █
```

**What to do:**
- Copy the long string (WITHOUT the newline)
- Save it in a notepad file named "MY_SECRET_KEY.txt"

**You now have:** Your SECRET_KEY 🔐

---

### STEP 2: Create Account

**Screen you see:**
```
https://www.pythonanywhere.com
┌─────────────────────────────────┐
│  PythonAnywhere                 │
│                                 │
│  [ Sign Up ]  [ Sign In ]       │
│                                 │
│  ┌───────────────────────────┐  │
│  │ Email: your@email.com     │  │
│  │ Password: ••••••••••••   │  │
│  │ Confirm: ••••••••••••   │  │
│  │                           │  │
│  │ [ Create Account ]        │  │
│  └───────────────────────────┘  │
│                                 │
└─────────────────────────────────┘
```

**What to do:**
1. Click "Sign Up" button
2. Choose "Beginner" account (free)
3. Fill in email & password
4. Click "Create Account"
5. Check email for confirmation link
6. Click confirmation link

**You now have:** PythonAnywhere account 🎯

---

### STEP 3: Link GitHub

**Screens you'll see:**

```
After confirming email:
┌──────────────────────────────────┐
│  Welcome to PythonAnywhere!      │
│                                  │
│  Linked accounts:                │
│  □ GitHub                        │
│  □ Facebook                      │
│                                  │
│  [ Link to GitHub ]              │
│                                  │
└──────────────────────────────────┘

Then:
┌──────────────────────────────────┐
│  GitHub Authorization            │
│                                  │
│  PythonAnywhere wants access to: │
│  • Read public repository        │
│  • Clone repositories            │
│                                  │
│  [ Authorize PythonAnywhere ]   │
│                                  │
└──────────────────────────────────┘

Success:
┌──────────────────────────────────┐
│  ✓ GitHub account linked!        │
│  Username: RajuManur143          │
└──────────────────────────────────┘
```

**You now have:** GitHub linked ✓

---

### STEP 4: Create Web App

**Screen you see:**

```
PythonAnywhere Dashboard
┌──────────────────────────────────────────┐
│  Web                                     │
│                                          │
│  [ Add a new web app ]                   │
│                                          │
│  Configuration:                          │
│  ○ Manually configured                   │
│  ○ Bottle (don't choose)                 │
│  ○ Flask (don't use quick start)        │
│  ○ Django                                │
│                                          │
│  Select: ◉ Manually configured           │
│                                          │
│  Python version:                         │
│  ○ Python 2.7                            │
│  ◉ Python 3.10                           │
│  ○ Python 3.11                           │
│                                          │
│  [ Continue ]                            │
└──────────────────────────────────────────┘

After Continue:
┌──────────────────────────────────────────┐
│  ✓ Web app created!                      │
│                                          │
│  Your domain:                            │
│  yourusername.pythonanywhere.com         │
│                                          │
│  [ Reload | Settings | SSL | ... ]       │
└──────────────────────────────────────────┘
```

**You now have:** Web app created with domain 🌐

---

### STEP 5: Clone Repository

**Screen you see:**

```
PythonAnywhere → Consoles
┌─────────────────────────────────────────────┐
│  [ Start a new Bash console ]               │
│                                             │
│  Bash console                               │
│  ┌──────────────────────────────────────┐  │
│  │ 12:34 ~ $ █                          │  │
│  └──────────────────────────────────────┘  │
│                                             │
│  Type command here: ▼                       │
└─────────────────────────────────────────────┘
```

**What you type:**
```bash
cd /home/yourusername
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
```

**What happens:**
```
12:34 ~ $ cd /home/yourusername
12:34 ~ $ git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
Cloning into 'Habit_track_maadri_paa'...
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (12/12), done.
Unpacking objects: 100% (15/15), done.
12:34 ~ $ █
```

**You now have:** Code on PythonAnywhere 📥

---

### STEP 6: Install Dependencies

**What you type in console:**
```bash
cd /home/yourusername/Habit_track_maadri_paa
pip install --user -r requirements.txt
```

**What happens:**
```
12:34 ~ $ cd /home/yourusername/Habit_track_maadri_paa
12:34 ~ $ pip install --user -r requirements.txt
Collecting Flask==2.3.3
  Downloading Flask-2.3.3-py3-none-any.whl (95 kB)
     |████████████████████| 95 kB 2.3 MB/s
Collecting Flask-SQLAlchemy==3.0.5
  Downloading Flask_SQLAlchemy-3.0.5-py3-none-any.whl (21 kB)
     |████████████████████| 21 kB 5.1 MB/s
... (more packages)
Successfully installed Flask-2.3.3 Flask-SQLAlchemy-3.0.5 ... (6 packages total)
12:34 ~ $ █
```

**You now have:** All packages installed 📦

---

### STEP 7: Configure Paths

**Screen you see:**

```
PythonAnywhere Web Tab
┌──────────────────────────────────────────────┐
│  Web                                         │
│  yourusername.pythonanywhere.com             │
│                                              │
│  Code                                        │
│  ┌──────────────────────────────────────┐   │
│  │ Source code:                         │   │
│  │ /home/yourusername/Habit_track...    │   │
│  │                                      │   │
│  │ Working directory:                   │   │
│  │ /home/yourusername/Habit_track...    │   │
│  │                                      │   │
│  │ [ Save ]                             │   │
│  └──────────────────────────────────────┘   │
│                                              │
│  Virtualenv                                  │
│  [ (leave empty) ]                          │
│                                              │
│  [ Save ]                                    │
└──────────────────────────────────────────────┘
```

**What to do:**
1. Click in "Source code" field
2. Clear it and type: `/home/yourusername/Habit_track_maadri_paa`
3. Click in "Working directory" field
4. Clear it and type: `/home/yourusername/Habit_track_maadri_paa`
5. Leave Virtualenv empty
6. Click "Save"

**You now have:** Paths configured ✓

---

### STEP 8: Configure WSGI File

**Screen you see:**

```
PythonAnywhere Web Tab
┌─────────────────────────────────────────────┐
│  WSGI configuration file                    │
│                                             │
│  /var/www/yourusername_pythonanywhere...   │
│  [ Click to edit ]                          │
│                                             │
│  [ Edit ]                                   │
└─────────────────────────────────────────────┘

Click edit:
┌──────────────────────────────────────────────┐
│  Code Editor                                 │
│  ┌───────────────────────────────────────┐  │
│  │ # Everything is in here...            │  │
│  │ # Delete ALL and paste new code:      │  │
│  │                                       │  │
│  │ import sys                            │  │
│  │ import os                             │  │
│  │                                       │  │
│  │ path = '/home/yourusername/Habit...' │  │
│  │ if path not in sys.path:              │  │
│  │     sys.path.append(path)            │  │
│  │                                       │  │
│  │ os.environ['FLASK_ENV'] = ...         │  │
│  │ ...                                   │  │
│  │                                       │  │
│  │ from appraju import app               │  │
│  │ application = app                     │  │
│  │                                       │  │
│  │              [ Save ]                 │  │
│  └───────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

**What to do:**
1. Select ALL text (Ctrl+A)
2. Delete it
3. Paste the WSGI code from Step 8 of the complete guide
4. Replace `yourusername` (3 places)
5. Replace `YOUR_SECRET_KEY_HERE` with your actual key
6. Click "Save"

**You now have:** WSGI configured ✓

---

### STEP 9: Add Environment Variables

**Screen you see:**

```
PythonAnywhere Web Tab
┌──────────────────────────────────────────────┐
│  Environment variables                       │
│                                              │
│  ┌────────────────────────────────────────┐ │
│  │ FLASK_ENV = production                │ │
│  │ SECRET_KEY = a1b2c3d4e5f6g7h8i9j0k... │ │
│  │ DATABASE_PATH = /home/yourusername/... │ │
│  └────────────────────────────────────────┘ │
│                                              │
│  [ Add a new environment variable ]         │
└──────────────────────────────────────────────┘
```

**What to do:**
1. Click "Add a new environment variable"
2. Add each one:

```
Name: FLASK_ENV
Value: production
[Add]

Name: SECRET_KEY
Value: (paste your key from Step 1)
[Add]

Name: DATABASE_PATH
Value: /home/yourusername/Habit_track_maadri_paa/habits.db
[Add]
```

**You now have:** Environment variables set ✓

---

### STEP 10: Initialize Database

**What you type in console:**
```bash
cd /home/yourusername/Habit_track_maadri_paa
python3 -c "from appraju import init_db; init_db()"
```

**What happens:**
```
12:34 ~ $ cd /home/yourusername/Habit_track_maadri_paa
12:34 ~ $ python3 -c "from appraju import init_db; init_db()"
Database initialized successfully!
Sample habits loaded!
12:34 ~ $ █
```

**What was created:**
```
/home/yourusername/Habit_track_maadri_paa/
├── appraju.py
├── requirements.txt
├── habits.db ← NEW FILE!
├── .gitignore
└── Procfile
```

**You now have:** Database initialized 🗄️

---

### STEP 11: Reload Web App

**Screen you see:**

```
PythonAnywhere Web Tab (Top)
┌──────────────────────────────────────────────┐
│  yourusername.pythonanywhere.com             │
│                                              │
│  [ ↻ Reload | ⚙ Settings | ... ]             │
│                                              │
│  Click: ↻ Reload                             │
│                                              │
│  Result: Button shows "Reloading..." then    │
│  turns green ✓ with "Last reloaded: now"     │
└──────────────────────────────────────────────┘
```

**What happens:**
1. Click "Reload" button
2. Button shows spinning icon
3. Button turns green
4. Your app starts up
5. Takes 10-20 seconds

**You now have:** App reloaded and live! 🚀

---

### STEP 12: Test Your App

**What you see:**

```
Browser Address Bar:
https://yourusername.pythonanywhere.com

Page loads:
┌─────────────────────────────────────────┐
│ 🎯 Habit Tracker                        │
│                                         │
│ Your Habits                             │
│ ┌─────────────────────────────────────┐ │
│ │ ✓ Exercise        💪 (blue)         │ │
│ │ ✓ Read Book       📚 (green)        │ │
│ │ □ Meditate        🧘 (yellow)       │ │
│ │ □ Journal         📝 (purple)       │ │
│ │                                   │ │
│ │ [ + Add Habit ]                   │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ November 2025 Calendar:                 │
│ ┌─────────────────────────────────────┐ │
│ │ Mo Tu We Th Fr Sa Su                │ │
│ │              1  2                   │ │
│ │ 3  4  5  6  7  8  9                 │ │
│ │ ...                                 │ │
│ └─────────────────────────────────────┘ │
│                                         │
│ Progress Chart:                         │
│ [Chart showing habits tracked]          │
│                                         │
└─────────────────────────────────────────┘
```

**Test these:**
1. ✓ Page loads (no errors)
2. ✓ You see 4 sample habits
3. ✓ Click checkbox → it marks complete
4. ✓ Click "+" → add new habit dialog appears
5. ✓ Calendar shows correctly
6. ✓ Colors and emojis display

**Success screen:**
```
┌──────────────────────────────────────┐
│ ✅ Everything Works!                 │
│                                      │
│ Your Habit Tracker is LIVE!         │
│                                      │
│ Share: yourusername.pythonanywhere.. │
│                                      │
│ Start tracking habits! 📊             │
└──────────────────────────────────────┘
```

**You now have:** Working Habit Tracker! 🎉

---

## Timeline Visualization

```
0:00 ────┬─→ 1:00 ────┬─→ 2:00 ────┬─→ 3:00 ────┬─→ 4:00
        │            │            │            │
     [1] START    [2] Create   [3] Link    [4] Web app
     Sec Key      Account      GitHub      created
        │
     Copy & Save
                │
                Account ready
                     │
                  Link done
                          │
                       Domain assigned
                               │
                            yourusername.pythonanywhere.com


5:00 ────┬─→ 6:00 ────┬─→ 7:00 ────┬─→ 8:00 ────┬─→ 9:00
        │            │            │            │
     [5] Clone    [6] Install  [7] Config  [8] WSGI
        Code       Packages      Paths       File
        │
     Code on
     server
                │
                Packages ready
                     │
                  Paths set
                          │
                       WSGI configured


10:00 ───┬─→ 11:00 ──┬─→ 12:00 ──┬─→ 13:00 ──┬─→ 15:00
        │           │           │          │
     [9] Env    [10] Init   [11] Reload [12] TEST
     Variables  Database    App
        │
     Vars added
                │
                DB created
                    │
                 App reloading
                       │
                    🎉 LIVE! 🎉


🎯 Total Timeline: 25-30 minutes to go live!
```

---

## Where to Find Everything

### PythonAnywhere Tabs
```
Dashboard
├── Web
│   ├── Your domain (yourusername.pythonanywhere.com)
│   ├── Code section (Source code, Working directory)
│   ├── WSGI configuration file
│   └── Environment variables
│
├── Consoles
│   └── Bash console (for git, pip, python commands)
│
├── Files
│   └── File browser (can view/edit files here)
│
└── Account
    └── Account settings
```

### Important Files
```
Your Project (/home/yourusername/Habit_track_maadri_paa):
├── appraju.py (your Flask app - don't edit usually)
├── requirements.txt (dependencies)
├── habits.db (database - created in Step 10)
├── Procfile (for deployment)
└── .gitignore

PythonAnywhere Files:
├── WSGI file (/var/www/yourusername_pythonanywhere_com_wsgi.py)
└── Web app settings (in Web tab)
```

---

## Quick Fixes Diagram

```
Something's Wrong?

"404 Page not found"
    ↓
Check Error log:
  Web tab → Error log button
    ↓
Look for red text

"Module not found"
    ↓
Check paths in Step 7
  Verify username spelling
    ↓
Check WSGI file in Step 8
  Verify paths and username (3 places)

"Database error"
    ↓
Check database file exists:
  /home/yourusername/Habit_track_maadri_paa/habits.db
    ↓
If not, run Step 10 again

"Secret key error"
    ↓
Check environment variables
  FLASK_ENV, SECRET_KEY, DATABASE_PATH
    ↓
Edit and save each one

"Module 'flask' not found"
    ↓
Run Step 6 again:
  pip install --user -r requirements.txt
    ↓
Click Reload

Still stuck?
    ↓
See PYTHONANYWHERE_TROUBLESHOOTING.md
```

---

## Summary Card

```
┌─────────────────────────────────────────┐
│  PythonAnywhere Deployment Summary      │
├─────────────────────────────────────────┤
│ Account:   yourusername                │
│ Domain:    yourusername.pythonanywhere. │
│            com                          │
│ Project:   /home/yourusername/Habit... │
│ Database:  habits.db                    │
│ Port:      Auto (port 80/443)          │
│                                         │
│ Key files:                              │
│ • WSGI:    /var/www/.../wsgi.py        │
│ • Code:    /home/username/Habit...     │
│ • DB:      /home/username/Habit.../... │
│                                         │
│ Environment Variables (3):              │
│ 1. FLASK_ENV = production               │
│ 2. SECRET_KEY = (your long key)         │
│ 3. DATABASE_PATH = /home/...habits.db   │
│                                         │
│ Update Process:                         │
│ 1. Push to GitHub                       │
│ 2. Bash: git pull origin main            │
│ 3. Web: Click Reload                    │
│ 4. Done! (1 minute)                     │
└─────────────────────────────────────────┘
```

---

## You're Ready! 🚀

Follow the 12 steps and your Habit Tracker will be live in 25-30 minutes!

**Questions? See PYTHONANYWHERE_TROUBLESHOOTING.md**
