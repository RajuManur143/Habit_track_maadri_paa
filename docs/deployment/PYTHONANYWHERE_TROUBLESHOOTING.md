# PythonAnywhere Troubleshooting & FAQ

## Quick Problem Finder

**Your problem:**
- [Page won't load (404)](#page-wont-load-404)
- [Page shows blank/no content](#page-shows-blank-or-no-content)
- [Internal Server Error (500)](#internal-server-error-500)
- [Module not found error](#module-not-found-error)
- [Database errors](#database-errors)
- [App crashes immediately](#app-crashes-immediately)
- [Cannot write to database](#cannot-write-to-database)
- [Website says "Service Unavailable"](#website-says-service-unavailable)
- [Git clone failed](#git-clone-failed)
- [Page loads but styling/emojis missing](#page-loads-but-styling-emojis-missing)

---

## How to Read the Error Log

### Finding the Error Log

1. Go to **Web** tab
2. Click on your domain name: `yourusername.pythonanywhere.com`
3. Scroll down to find **"Error log"** button
4. Click it

### What You'll See

```
Error log for yourusername.pythonanywhere.com
─────────────────────────────────────────────

[timestamp] Starting web app at https://yourusername.pythonanywhere.com
[timestamp] Loaded app module
[timestamp] Listening on ...

ERROR MESSAGES (if any):
[timestamp] ModuleNotFoundError: No module named 'flask'
[timestamp] FileNotFoundError: [Errno 2] No such file...
[timestamp] ValueError: ...
```

### Color Meanings

- **Green text** = Good! Everything OK
- **Yellow text** = Warning, might need attention
- **Red text** = Error! Something's broken
- **Timestamps** = When the message occurred

### Useful Lines to Look For

```
✓ "Successfully loaded Flask" = Good!
✓ "Listening on" = App started!
✗ "ModuleNotFoundError" = Package not installed
✗ "FileNotFoundError" = Path is wrong
✗ "ValueError" = Configuration issue
✗ "SECRET_KEY" = Missing environment variable
```

---

## Problem 1: Page Won't Load (404)

### Symptoms
```
Website shows:
404 Not Found
The requested URL /... was not found on this server.
```

### Root Causes

**Cause 1: Wrong domain**
- Make sure you're visiting: `https://yourusername.pythonanywhere.com`
- Not `http://` (needs `https://`)
- Not `localhost:5000`

**Cause 2: App didn't start**
- Flask app failed to initialize
- Server won't serve requests

**Cause 3: WSGI configuration is wrong**
- Path to Flask app is incorrect
- Variable names are wrong

### Solution Steps

**Step 1: Check your URL**
```
❌ Wrong: http://yourusername.pythonanywhere.com
❌ Wrong: yourusername.pythonanywhere.com (no https)
✓ Correct: https://yourusername.pythonanywhere.com
```

**Step 2: Check error log**
1. Web tab → Error log
2. Look at last 10 lines
3. See what error is reported

**Step 3: Check WSGI file**
1. Web tab → WSGI configuration file
2. Verify:
   ```python
   path = '/home/yourusername/Habit_track_maadri_paa'
   ```
   - Replace `yourusername` with actual username
   - Folder name exactly: `Habit_track_maadri_paa`

**Step 4: Check paths**
1. Web tab → Code section
2. Source code: `/home/yourusername/Habit_track_maadri_paa`
3. Working directory: `/home/yourusername/Habit_track_maadri_paa`
4. Click Save

**Step 5: Reload**
1. Click green "Reload" button
2. Wait 10 seconds
3. Visit URL again

### If Still Not Working

1. Bash console: `ls /home/yourusername/Habit_track_maadri_paa`
   - Should see: `appraju.py`, `requirements.txt`, `habits.db`
2. If missing, run Step 5 from complete guide (clone repository)

---

## Problem 2: Page Shows Blank or No Content

### Symptoms
```
Website loads but:
- All blank (just white page)
- No habits shown
- No calendar
- No styling
```

### Root Causes

**Cause 1: Database not initialized**
- `habits.db` file not created
- No data in database

**Cause 2: Import path wrong**
- Flask can't find your app
- App module not loading

**Cause 3: Static files not found**
- CSS/JavaScript not loading
- But page still loads (just unstyled)

### Solution Steps

**Step 1: Initialize database**
1. Bash console:
```bash
cd /home/yourusername/Habit_track_maadri_paa
python3 -c "from appraju import init_db; init_db()"
```

**Expected output:**
```
Database initialized successfully!
Sample habits loaded!
```

**Step 2: Check database exists**
1. Bash console:
```bash
ls -la /home/yourusername/Habit_track_maadri_paa/habits.db
```

**Expected output:**
```
-rw-r--r-- 1 yourusername yourusername 12288 Nov 22 10:30 habits.db
```

If not found, database creation failed. Check Step 1 output for errors.

**Step 3: Check WSGI imports**
1. Web tab → WSGI configuration file
2. Verify this line exists and is correct:
```python
from appraju import app
application = app
```

**Step 4: Reload**
1. Click Reload button
2. Wait 15 seconds
3. Visit URL and refresh (Ctrl+F5)

---

## Problem 3: Internal Server Error (500)

### Symptoms
```
Website shows:
500 Internal Server Error
The server encountered an internal error and was unable to complete your request.
```

### Root Causes

**Most Common: SECRET_KEY not set**
- Flask requires a SECRET_KEY
- Without it, app crashes immediately

**Other causes:**
- Database file has bad permissions
- WSGI path variables wrong
- Python syntax error in appraju.py

### Solution Steps

**Step 1: Check SECRET_KEY**
1. Web tab → Environment variables
2. Look for: `SECRET_KEY`
3. Make sure it's there and NOT empty
4. Should look like: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6...`

**If not set:**
1. Click "Add a new environment variable"
2. Name: `SECRET_KEY`
3. Value: (paste from Step 1 of complete guide)
4. Click Add
5. Reload web app

**Step 2: Check FLASK_ENV**
1. Environment variables
2. Find: `FLASK_ENV`
3. Should be: `production`

**If not set:**
1. Click "Add a new environment variable"
2. Name: `FLASK_ENV`
3. Value: `production`
4. Click Add
5. Reload web app

**Step 3: Check DATABASE_PATH**
1. Environment variables
2. Find: `DATABASE_PATH`
3. Should be: `/home/yourusername/Habit_track_maadri_paa/habits.db`

**If not set:**
1. Click "Add a new environment variable"
2. Name: `DATABASE_PATH`
3. Value: `/home/yourusername/Habit_track_maadri_paa/habits.db`
4. Click Add
5. Reload web app

**Step 4: Reload**
1. Click green "Reload" button
2. Wait 20 seconds
3. Visit URL
4. Try again

**Step 5: Check error log**
1. Web tab → Error log
2. If still 500 error, look for specific error message
3. Search for that error below

---

## Problem 4: Module Not Found Error

### Symptoms
```
Error log shows:
ModuleNotFoundError: No module named 'flask'
ModuleNotFoundError: No module named 'sqlalchemy'
ModuleNotFoundError: No module named 'appraju'
```

### Root Causes

**Cause 1: Dependencies not installed**
- `pip install` wasn't run
- Requirements.txt not installed

**Cause 2: Wrong path to project**
- Python can't find `appraju.py`
- Path in WSGI or code section is wrong

**Cause 3: Python version mismatch**
- pip installed for Python 2.7
- Web app using Python 3.10

### Solution Steps

**Step 1: Reinstall dependencies**
1. Bash console:
```bash
cd /home/yourusername/Habit_track_maadri_paa
pip install --user -r requirements.txt
```

**Look for:**
```
Successfully installed Flask-2.3.3 Flask-SQLAlchemy-3.0.5 ... (6 packages)
```

**Step 2: Verify Flask is installed**
1. Bash console:
```bash
python3 -c "import flask; print(flask.__version__)"
```

**Should show:**
```
2.3.3
```

**If error:**
- Flask not installed correctly
- Run Step 1 again

**Step 3: Verify appraju can be imported**
1. Bash console:
```bash
cd /home/yourusername/Habit_track_maadri_paa
python3 -c "from appraju import app; print('Success!')"
```

**Should show:**
```
Success!
```

**If error:**
- Python can't find appraju.py
- Check Step 7 of complete guide (paths are wrong)

**Step 4: Reload**
1. Click Reload button
2. Reload browser page (Ctrl+F5)

---

## Problem 5: Database Errors

### Symptoms
```
Error log shows:
sqlite3.OperationalError: disk I/O error
sqlite3.OperationalError: attempt to write a readonly database
FileNotFoundError: [Errno 2] No such file or directory: '/home/.../habits.db'
```

### Root Causes

**Cause 1: Database file doesn't exist**
- Step 10 (initialize database) wasn't run
- Or it failed

**Cause 2: Wrong database path**
- Path in code doesn't match actual location
- Environment variable wrong

**Cause 3: Permission issues**
- PythonAnywhere user can't read/write file

### Solution Steps

**Step 1: Check if database exists**
1. Bash console:
```bash
ls -la /home/yourusername/Habit_track_maadri_paa/habits.db
```

**If it exists:**
- Shows file info with size

**If it doesn't exist:**
- Shows `No such file or directory`
- Go to Step 2

**Step 2: Initialize database**
1. Bash console:
```bash
cd /home/yourusername/Habit_track_maadri_paa
python3 -c "from appraju import init_db; init_db()"
```

**Should show:**
```
Database initialized successfully!
Sample habits loaded!
```

**If error:**
- Check error message
- Usually means appraju.py has syntax error
- Check Step 6 of complete guide

**Step 3: Verify path matches**
1. Check WSGI file has correct path:
```python
os.environ['DATABASE_PATH'] = '/home/yourusername/Habit_track_maadri_paa/habits.db'
```

2. Check environment variable DATABASE_PATH:
```
/home/yourusername/Habit_track_maadri_paa/habits.db
```

3. Both must match exactly

**Step 4: Reload**
1. Click Reload button
2. Visit URL

---

## Problem 6: App Crashes Immediately

### Symptoms
```
Error log shows:
[timestamp] Loaded app module
[timestamp] Traceback (most recent call last):
    ...
[timestamp] Connection refused
[timestamp] App crashed
```

### Root Causes

**Cause 1: SECRET_KEY not set**
- Flask crashes without it

**Cause 2: Database can't be accessed**
- Wrong path
- File doesn't exist
- Permission denied

**Cause 3: Import error in appraju.py**
- Syntax error in code
- Missing import statement

### Solution Steps

**Step 1: Check all 3 environment variables are set**
1. Web tab → Environment variables
2. Must have ALL three:
   ```
   FLASK_ENV = production
   SECRET_KEY = (long string)
   DATABASE_PATH = /home/yourusername/Habit_track_maadri_paa/habits.db
   ```

**Step 2: Regenerate SECRET_KEY**
1. Bash console:
```bash
python3 -c "import secrets; print(secrets.token_hex(32))"
```

2. Copy output
3. Update environment variable SECRET_KEY with new value
4. Also update WSGI file

**Step 3: Initialize database**
1. Bash console:
```bash
cd /home/yourusername/Habit_track_maadri_paa
rm habits.db  # Delete old one
python3 -c "from appraju import init_db; init_db()"
```

**Step 4: Check WSGI file for syntax errors**
1. Web tab → WSGI configuration
2. Look for red underlines or errors
3. Common mistakes:
   - Missing `:` at end of lines
   - Wrong indentation
   - Typos in variable names

**Step 5: Reload**
1. Click Reload
2. Check error log for specific error
3. Fix and try again

---

## Problem 7: Cannot Write to Database

### Symptoms
```
Error log shows:
sqlite3.OperationalError: attempt to write a readonly database
IOError: [Errno 13] Permission denied: '...habits.db'
```

### Root Causes

**Cause 1: File permissions wrong**
- File created with wrong owner
- PythonAnywhere user can't write

**Cause 2: File in read-only directory**
- Unlikely on PythonAnywhere
- But possible if you moved file

### Solution Steps

**Step 1: Delete and recreate database**
1. Bash console:
```bash
cd /home/yourusername/Habit_track_maadri_paa
rm habits.db
python3 -c "from appraju import init_db; init_db()"
```

2. Should create new file with correct permissions

**Step 2: Verify file permissions**
1. Bash console:
```bash
ls -la /home/yourusername/Habit_track_maadri_paa/habits.db
```

**Should show:**
```
-rw-r--r-- 1 yourusername yourusername 12288 Nov 22 10:30 habits.db
```

- First `rw-` means you (owner) can read and write ✓
- If shows `-r--r--r--` (read-only), fix with:
```bash
chmod 644 /home/yourusername/Habit_track_maadri_paa/habits.db
```

**Step 3: Reload**
1. Click Reload
2. Try adding a habit or checking a box
3. Should work now

---

## Problem 8: Website Says "Service Unavailable"

### Symptoms
```
Browser shows:
503 Service Unavailable
The server is temporarily unable to serve your request.
```

### Root Causes

**Cause 1: App is reloading**
- You just clicked Reload button
- Perfectly normal, wait 10 seconds

**Cause 2: Too many requests**
- Free tier has CPU limits
- Very rare if you're alone

**Cause 3: Web app is disabled**
- Account issue or quota exceeded

### Solution Steps

**Step 1: Wait**
- Click Reload button was just clicked?
- Wait 15-20 seconds
- Try again

**Step 2: Check Web app is enabled**
1. Web tab
2. At the top, make sure app shows as "ON" (green)
3. If red/OFF, click to turn ON

**Step 3: Reload again**
1. Click Reload button
2. Wait 20 seconds
3. Visit URL with Ctrl+F5 (refresh cache)

**Step 4: Check account status**
1. Account tab
2. Check if account is active
3. Free tier has usage limits
4. Usually not a problem

---

## Problem 9: Git Clone Failed

### Symptoms
```
Bash console shows:
fatal: could not read Username for 'https://github.com': ...
fatal: Authentication failed
```

### Root Causes

**Cause 1: GitHub not linked**
- Step 3 wasn't done properly
- Or PythonAnywhere lost authorization

**Cause 2: Repository URL wrong**
- Typo in git clone command

**Cause 3: Repository is private**
- Need GitHub token (not in this guide)

### Solution Steps

**Step 1: Verify GitHub is linked**
1. Account tab (in PythonAnywhere)
2. Look for "Linked accounts"
3. Should show GitHub username: `RajuManur143`

**If not linked:**
1. Click "Link to GitHub"
2. Authorize PythonAnywhere
3. Should now show username

**Step 2: Verify URL is correct**
```bash
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
```

- Check: `RajuManur143` (your GitHub username)
- Check: `Habit_track_maadri_paa` (repository name)

**Step 3: Try cloning again**
1. Bash console:
```bash
cd /home/yourusername
rm -rf Habit_track_maadri_paa  # Remove old folder if exists
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
```

2. Should now work

**Step 4: If still fails**
- Repository might be private
- Add GitHub token (advanced topic)
- Or temporarily make repo public

---

## Problem 10: Page Loads But Styling/Emojis Missing

### Symptoms
```
Website loads and you see:
- All text, no colors
- No emojis (just boxes or weird symbols)
- No nice formatting
- Charts missing
- But app is functional
```

### Root Causes

**Cause 1: CSS not loading**
- Static files path wrong
- But Flask still serves page

**Cause 2: Browser cache**
- Old version of page cached
- With different styling

**Cause 3: Tailwind CSS didn't load**
- Internet connection issue when page loaded
- Or Tailwind CDN temporarily down

### Solution Steps

**Step 1: Hard refresh browser**
1. Press: `Ctrl + Shift + Delete` (Windows)
2. Or: `Cmd + Shift + Delete` (Mac)
3. Clear cache
4. Reload page: `Ctrl + F5`

**Step 2: Try different browser**
1. Try Chrome, Firefox, Edge
2. If it works in one browser, it's a cache issue

**Step 3: Check internet connection**
1. Reload page
2. Make sure internet is working
3. Try on different WiFi or mobile hotspot

**Step 4: Check HTML source**
1. Right-click on page → "View Page Source"
2. Look for `<style>` tags
3. Look for `<script>` tags
4. If missing, check error log

### Usually Resolves In 1 Minute

- Just a caching/temporary issue
- Hard refresh usually fixes it

---

## How to Read Error Log for Debugging

### Good Signs ✓

```
[timestamp] Starting web app at https://yourusername.pythonanywhere.com
```
✓ App is starting

```
[timestamp] Loaded app module
```
✓ Flask app loaded successfully

```
[timestamp] Listening on ...
```
✓ Ready to accept requests

```
[timestamp] 200 [timestamp] /api/habits
```
✓ Request successful

### Bad Signs ✗

```
[timestamp] ModuleNotFoundError: No module named 'flask'
```
✗ Dependencies not installed - Run: `pip install --user -r requirements.txt`

```
[timestamp] FileNotFoundError: [Errno 2] No such file or directory: '...'
```
✗ Path wrong or file missing - Check Step 7 and 8

```
[timestamp] ValueError: SECRET_KEY is not set
```
✗ Environment variable missing - Add FLASK_ENV, SECRET_KEY, DATABASE_PATH

```
[timestamp] sqlite3.OperationalError: attempt to write a readonly database
```
✗ Database permission issue - Delete and recreate: `rm habits.db && python3 -c "from appraju import init_db; init_db()"`

### How to Search Logs

1. Error log page has search box (usually)
2. Search for keywords:
   - `ERROR` - shows all errors
   - `ModuleNotFoundError` - package missing
   - `FileNotFoundError` - path wrong
   - `ValueError` - configuration wrong
   - `sqlite3` - database issue

3. Look at timestamp to find most recent error

---

## Verification Checklist

After deployment, verify all these are correct:

### ✓ Paths
- [ ] Source code: `/home/yourusername/Habit_track_maadri_paa`
- [ ] Working directory: `/home/yourusername/Habit_track_maadri_paa`
- [ ] Database path: `/home/yourusername/Habit_track_maadri_paa/habits.db`

### ✓ Environment Variables (3 total)
- [ ] FLASK_ENV = `production`
- [ ] SECRET_KEY = `(long hex string)`
- [ ] DATABASE_PATH = `/home/yourusername/Habit_track_maadri_paa/habits.db`

### ✓ WSGI File
- [ ] Has: `import sys`
- [ ] Has: `import os`
- [ ] Has: `path = '/home/yourusername/Habit_track_maadri_paa'`
- [ ] Has: `sys.path.append(path)`
- [ ] Has: `os.environ['FLASK_ENV'] = 'production'`
- [ ] Has: `os.environ['SECRET_KEY'] = '(your key)'`
- [ ] Has: `os.environ['DATABASE_PATH'] = '...'`
- [ ] Has: `from appraju import app`
- [ ] Has: `application = app`

### ✓ Files
- [ ] `/home/yourusername/Habit_track_maadri_paa/appraju.py` exists
- [ ] `/home/yourusername/Habit_track_maadri_paa/requirements.txt` exists
- [ ] `/home/yourusername/Habit_track_maadri_paa/habits.db` exists
- [ ] Can import Flask: `python3 -c "import flask"`
- [ ] Can import app: `python3 -c "from appraju import app"`

### ✓ Website
- [ ] Can visit: `https://yourusername.pythonanywhere.com`
- [ ] Page loads (no 404 or 500)
- [ ] See 4 sample habits
- [ ] Can click checkbox
- [ ] Can add new habit
- [ ] Calendar displays correctly

---

## Still Stuck?

### Try This Checklist

1. **Reload the app**
   - Web tab → Click green "Reload" button
   - Wait 20 seconds
   - Refresh page (Ctrl+F5)

2. **Clear browser cache**
   - Ctrl + Shift + Delete
   - Clear all
   - Reload page

3. **Check error log**
   - Web tab → Error log button
   - Look at last 10 lines
   - Search for "Error" or "ModuleNotFoundError"

4. **Check all environment variables**
   - Must have: FLASK_ENV, SECRET_KEY, DATABASE_PATH
   - All three must be present
   - None can be empty

5. **Verify database exists**
   - Bash: `ls /home/yourusername/Habit_track_maadri_paa/habits.db`
   - If not found: recreate it (see Problem 5)

6. **Reinstall dependencies**
   - Bash: `pip install --user -r requirements.txt --force-reinstall`

7. **Check WSGI file**
   - Open and re-save (even if no changes)
   - Sometimes fixes permission issues

8. **Reload and wait**
   - Patience is important
   - Web apps take 10-20 seconds to start
   - Wait before trying again

### If ALL else fails

1. Delete everything and start over:
```bash
cd /home/yourusername
rm -rf Habit_track_maadri_paa
git clone https://github.com/RajuManur143/Habit_track_maadri_paa.git
cd Habit_track_maadri_paa
pip install --user -r requirements.txt
python3 -c "from appraju import init_db; init_db()"
```

2. Follow Step 7-11 of complete guide again

3. Reload web app

---

## Success Indicators

Your deployment is successful when:

✓ Website loads without errors
✓ See "Habit Tracker" title
✓ See 4 sample habits with colors and emojis
✓ Can click checkbox to mark habit complete
✓ Can click "+" to add new habit
✓ Calendar shows correct month
✓ Charts display correctly
✓ No red errors in error log
✓ URL is `https://yourusername.pythonanywhere.com`

**When you see all these, you're DONE! 🎉**

---

## Common Questions (FAQ)

**Q: Do I have to keep my computer on?**
A: No! PythonAnywhere servers keep your app running 24/7.

**Q: Can others access my Habit Tracker?**
A: Yes! Share your URL: `https://yourusername.pythonanywhere.com`

**Q: Will I lose my habits if I restart?**
A: No! Data is saved in habits.db file on PythonAnywhere servers.

**Q: How do I update my code?**
A: Push to GitHub, then in PythonAnywhere: `git pull origin main`, then click Reload.

**Q: Can I use a custom domain?**
A: Yes! On paid account. See complete guide advanced section.

**Q: Can I upgrade to PostgreSQL?**
A: Yes! For persistent data. Advanced topic, in complete guide.

**Q: Is my SECRET_KEY safe?**
A: Yes, it's only on PythonAnywhere servers. Never shared.

**Q: Can I see who's using my app?**
A: Yes, PythonAnywhere shows access logs.

**Q: What if I break something?**
A: See troubleshooting guide. Usually fixable in 5 minutes.

---

**You've got this! 🚀**

Most issues are solved by one of these:
1. Reload app
2. Check environment variables
3. Check error log
4. Check paths are correct

If you follow the complete guide exactly, it works 99% of the time!
