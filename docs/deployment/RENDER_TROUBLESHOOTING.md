# Render Troubleshooting Guide

Complete troubleshooting for all common Render deployment issues with solutions.

## Table of Contents
1. [Build Issues](#build-issues)
2. [Runtime Issues](#runtime-issues)
3. [Database Issues](#database-issues)
4. [Performance Issues](#performance-issues)
5. [Access Issues](#access-issues)
6. [Debugging Tools](#debugging-tools)

---

## Build Issues

### Issue 1: "Build Failed" - Command exited with non-zero status

**Symptoms:**
```
Build log shows:
error: Command 'pip install -r app/requirements.txt' exited with code 1
Build failed
```

**Cause:** Wrong path or missing file

**Solution:**

Check your build command:
```
Current: pip install -r requirements.txt  ❌ WRONG
Correct: pip install -r app/requirements.txt  ✓ CORRECT
```

**Steps to fix:**
1. Go to Service Settings
2. Find "Build Command"
3. Change to: `pip install -r app/requirements.txt`
4. Click "Save"
5. Click "Manual Deploy" → "Latest"

**Verify in logs:**
```
12:34 PM Running build command...
12:35 PM Installing collected packages: flask, sqlalchemy, ...
12:36 PM Successfully installed flask-2.3.3 ...
12:37 PM Build succeeded ✓
```

---

### Issue 2: "pip: command not found"

**Symptoms:**
```
Build log shows:
/bin/sh: pip: command not found
```

**Cause:** Python not in environment

**Solution:**

Make sure Python 3 is selected:
1. Go to Service Settings
2. Find "Environment"
3. Change to: `Python 3`
4. Click "Save"
5. Redeploy

---

### Issue 3: "No module named 'flask'"

**Symptoms:**
```
Build log shows:
Successfully installed... (shows completion)
But app fails with: ModuleNotFoundError: No module named 'flask'
```

**Cause:** Dependencies installed but app can't find them

**Solution:**

Check Start Command uses full path:
```
Current: gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app  ❌ WRONG
Correct: cd app && gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app  ✓ CORRECT
```

**Steps to fix:**
1. Go to Service Settings
2. Find "Start Command"
3. Change to: `cd app && gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app`
4. Click "Save"
5. Click "Manual Deploy" → "Latest"

---

### Issue 4: "requirements.txt not found"

**Symptoms:**
```
Build log shows:
ERROR: [Errno 2] No such file or directory: 'app/requirements.txt'
```

**Cause:** File doesn't exist or wrong name

**Solution:**

**Check 1:** Verify file exists in your repo:
```
Your repo structure should be:
Raju_habit_tracker/
├── app/
│   ├── appraju.py
│   ├── requirements.txt  ← This file must exist
│   └── .gitignore
├── docs/
...
```

**Check 2:** If using file at root level:
```
If requirements.txt is at root (not in /app):
Build Command: pip install -r requirements.txt
```

**Check 3:** Push to GitHub:
```bash
git add app/requirements.txt
git commit -m "Add requirements.txt"
git push origin main
```

**Check 4:** Force rebuild in Render:
1. Go to Service
2. Click "Manual Deploy" → "Latest"

---

### Issue 5: "Git clone failed"

**Symptoms:**
```
Build log shows:
fatal: could not read Username for 'https://github.com': No such file or directory
```

**Cause:** GitHub connection issue

**Solution:**

**Step 1:** Verify GitHub connection:
1. Go to Service Settings
2. Find "Repository"
3. Check it shows your correct repo
4. If wrong, click "Disconnect" and reconnect

**Step 2:** If still failing:
1. Go to Render Account Settings
2. Click "GitHub"
3. Verify permissions are correct
4. Re-authorize Render access to GitHub

**Step 3:** Force redeploy:
1. Make small change to repo (add space to comment)
2. Push to GitHub
3. Render auto-detects and rebuilds

---

## Runtime Issues

### Issue 6: "503 Service Unavailable"

**Symptoms:**
```
Browser shows:
503 Service Unavailable
```

**Cause:** App is crashing or not starting

**Solution:**

**Step 1:** Check logs:
1. Go to your Service
2. Click "Logs" tab
3. Look for error messages

**Step 2:** Common causes and fixes:

```
Error: "ModuleNotFoundError: No module named 'appraju'"
Fix: Start Command should be: cd app && gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app

Error: "Address already in use"
Fix: Change port in start command (Render sets $PORT automatically, don't hardcode)

Error: "No such file or directory: 'app/habits.db'"
Fix: Initialize database using Shell tab (see Issue 11)

Error: "SECRET_KEY not found"
Fix: Check environment variables (see Issue 13)

Error: "Permission denied: '/opt/render/project/..'"
Fix: Contact Render support, likely permission issue
```

**Step 3:** If still failing:
1. Click "Manual Deploy" → "Latest"
2. Wait 2-3 minutes
3. Check logs again

---

### Issue 7: "Connection refused" on localhost

**Symptoms:**
```
When testing locally before pushing:
ConnectionRefusedError: [Errno 111] Connection refused
```

**Cause:** Local Flask app not running

**Solution:**

**If testing locally:**
```bash
cd app
export FLASK_ENV=development
export SECRET_KEY=test-key
python appraju.py
```

Then visit: http://localhost:5000

**If on Render:**
```
Don't try localhost, use your Render URL:
https://habit-tracker.onrender.com
```

---

### Issue 8: "Port 5000 already in use"

**Symptoms:**
```
OSError: [Errno 98] Address already in use
```

**Cause:** Another process using port 5000

**Solution:**

**For local development:**
```bash
# Find what's using port 5000
lsof -i :5000

# Kill it
kill -9 <PID>

# Or use different port
export PORT=5001
python appraju.py
```

**For Render:**
- Render automatically assigns port via $PORT variable
- Your start command must use $PORT:
```
cd app && gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app
```

---

### Issue 9: "H12 Request Timeout"

**Symptoms:**
```
Request timeout after 30 seconds
App seems to hang
```

**Cause:** App taking too long to respond

**Solution:**

**Check your code:**
- No infinite loops
- No long blocking operations
- Database queries are optimized

**Check logs:**
1. Click "Logs" tab
2. Look for slow requests
3. Optimize those endpoints

**For Habit Tracker app:**
- All endpoints should respond in <1 second
- If slow, database not initialized properly

---

### Issue 10: "Application log limit exceeded"

**Symptoms:**
```
Service stops responding
"Log limit exceeded" in Render dashboard
```

**Cause:** Too many logs being generated

**Solution:**

**Step 1:** Reduce logging:
Edit your `appraju.py`, find logging section:
```python
# Reduce log level from INFO to WARNING
logging.basicConfig(level=logging.WARNING)  # Changed from INFO
```

**Step 2:** Push and redeploy:
```bash
git add app/appraju.py
git commit -m "Reduce logging verbosity"
git push origin main
```

**Step 3:** Clear old logs:
1. Go to Service Settings
2. Click "Soft Restart"
3. Service restarts and logs reset

---

## Database Issues

### Issue 11: "No such table: habit"

**Symptoms:**
```
Browser shows error:
(sqlite3.OperationalError) no such table: habit
```

**Cause:** Database not initialized

**Solution:**

**Step 1:** Use Shell to initialize:
1. Go to your Service
2. Click "Shell" tab
3. Paste this command:
```bash
cd /opt/render/project/src/app && python -c "from appraju import init_db; init_db()"
```
4. Press Enter
5. See success message:
```
Database initialized successfully!
Sample habits loaded!
```

**Step 2:** Test in browser:
- Go to: https://habit-tracker.onrender.com
- Should see 4 sample habits
- No errors

**If still not working:**

**Step 3:** Check DATABASE_PATH env var:
1. Go to Service Settings
2. Find "Environment Variables"
3. Verify DATABASE_PATH is set:
```
DATABASE_PATH=/opt/render/project/src/app/habits.db
```
4. If missing, add it

**Step 4:** Manual database file creation:
```bash
# In Shell tab
cd /opt/render/project/src/app
touch habits.db
python -c "from appraju import init_db; init_db()"
```

---

### Issue 12: "Database is locked"

**Symptoms:**
```
Error: database is locked
Multiple requests fail randomly
```

**Cause:** SQLite doesn't handle concurrent writes well

**Solution:**

**Option 1:** (Temporary) Soft restart:
1. Go to Service
2. Click "Settings"
3. Click "Soft Restart"

**Option 2:** (Long-term) Upgrade to PostgreSQL:

Instead of SQLite, use Render's PostgreSQL:

1. Go to Render Dashboard
2. Click "New +"
3. Select "PostgreSQL"
4. Click "Create"
5. Copy connection string
6. Update DATABASE_URL in env vars
7. Redeploy your app

(See full guide in RENDER_POSTGRESQL_SETUP.md)

---

### Issue 13: "Habit counts don't match"

**Symptoms:**
```
Database seems corrupted
Habit counts wrong
Completions not showing
```

**Cause:** Stale database or initialization issue

**Solution:**

**Step 1:** Check database:
```bash
# In Shell tab
cd /opt/render/project/src/app
python -c "from appraju import db; print(len(db.session.query(Habit).all()))"
```

**Step 2:** Reinitialize if needed:
```bash
# Delete old database
rm habits.db

# Create new one
python -c "from appraju import init_db; init_db()"
```

**Step 3:** Verify:
- Visit app URL
- See 4 fresh sample habits
- All counts reset to 0

---

## Performance Issues

### Issue 14: "App is very slow"

**Symptoms:**
```
Pages take 5+ seconds to load
Charts load slowly
```

**Cause:** Missing database indexes or free tier resource limits

**Solution:**

**Check 1:** Indexes are in place:
Your code already has indexes:
```python
# In models
__table_args__ = (Index('idx_habit_name', 'name'),)
__table_args__ = (Index('idx_completion_habit_date', 'habit_id', 'date'),)
```

**Check 2:** Upgrade instance (pay option):
1. Go to Service Settings
2. Find "Instance Type"
3. Change from "Free" to "Standard"
4. Cost: $7/month (but much faster)

**Check 3:** Optimize Flask:
1. Use Flask caching (optional upgrade)
2. Compress responses:
```python
from flask_compress import Compress
Compress(app)
```

**Check 4:** Monitor metrics:
1. Click "Metrics" tab
2. Look at CPU and Memory usage
3. If constantly maxed out, upgrade instance

---

### Issue 15: "Memory usage keeps increasing"

**Symptoms:**
```
Service gradually uses more memory
Eventually crashes with: MemoryError
```

**Cause:** Memory leak in app

**Solution:**

**Check 1:** Database connections:
Ensure you're closing DB connections:
```python
# Your code should have this (already does)
db.session.close()  # After each request
```

**Check 2:** Restart app periodically:
1. Go to Service Settings
2. Click "Soft Restart"
3. Clears memory, app starts fresh

**Check 3:** Check for circular imports:
- Make sure imports don't create loops
- Check if models are imported correctly

---

## Access Issues

### Issue 16: "Can't access the app from phone/outside network"

**Symptoms:**
```
Works on your computer
But friends can't access it
"Connection refused" on mobile
```

**Cause:** Usually not a real issue - it's working!

**Solution:**

**Step 1:** Verify it's working:
1. Open browser
2. Visit: https://habit-tracker.onrender.com
3. Should see your Habit Tracker

**Step 2:** Share the correct URL:
- Full URL: `https://habit-tracker.onrender.com`
- NOT: `localhost:5000`
- NOT: `127.0.0.1:5000`

**Step 3:** If still not working:
- Wait 5 minutes (deployment might still finishing)
- Check Status shows "Live ✓"
- Hard refresh browser: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)

**Step 4:** Check firewall:
- On your computer, check if firewall blocking access
- Usually not needed for external URL

---

### Issue 17: "SSL Certificate Error"

**Symptoms:**
```
Browser shows:
"Your connection is not private"
"SSL_ERROR_BAD_CERT_DOMAIN"
```

**Cause:** Using HTTP instead of HTTPS or certificate issue

**Solution:**

**Always use HTTPS:**
```
❌ http://habit-tracker.onrender.com
✓ https://habit-tracker.onrender.com
```

**Verify certificate:**
1. Click the lock icon in address bar
2. Should show Render's certificate
3. Domain should match your service name

**If still showing error:**
1. Go to Service Settings
2. Find "SSL/TLS"
3. Should show "Automatic (Let's Encrypt)"
4. Wait 10 minutes for certificate to issue
5. Refresh browser

---

### Issue 18: "Domain not available"

**Symptoms:**
```
Service created but no URL shown
or
"Waiting for domain..."
```

**Cause:** Domain assignment in progress

**Solution:**

**Wait 2-3 minutes**, then refresh:
1. Go to Service page
2. Press F5 to refresh
3. URL should appear:
```
https://habit-tracker.onrender.com
```

**If still not showing:**
1. Go to Service Settings
2. Check "Service Name" field
3. Should match what you entered
4. Click "Save"
5. Refresh dashboard

---

### Issue 19: "404 Not Found on custom domain"

**Symptoms:**
```
https://habit-tracker.onrender.com works
But https://myhabitapp.com shows 404
```

**Cause:** Custom domain not properly configured

**Solution:**

**Step 1:** Add custom domain:
1. Go to Service Settings
2. Find "Domains"
3. Click "Add Domain"
4. Enter: `myhabitapp.com`
5. Click "Add"

**Step 2:** Update DNS records:
1. Go to your domain registrar (GoDaddy, Namecheap, etc.)
2. Add CNAME record:
```
Name:  @
Type:  CNAME
Value: habit-tracker.onrender.com
```

**Step 3:** Wait for propagation:
- DNS changes take 15-60 minutes
- After that, custom domain works

---

## Debugging Tools

### How to Check Logs

**1. View Logs:**
```
Service Dashboard → [Logs] tab

You'll see output like:
12:34 PM Building and deploying...
12:35 PM Installing dependencies...
12:36 PM Starting service...
12:37 PM [2024-01-10 12:37:15] Starting Flask app
12:37 PM GET / HTTP/1.1" 200 2341
12:37 PM All systems go ✓
```

**2. Filter logs:**
- Type in search box to find specific messages
- Scroll to see older entries

**3. Export logs:**
1. Click "Logs" tab
2. Look for "Export" button
3. Download as text file

---

### How to Use Shell

**1. Access Shell:**
```
Service Dashboard → [Shell] tab
```

**2. Basic commands:**
```bash
# Check Python version
python --version

# List files
ls -la

# Check environment variables
env

# Install package
pip install packagename

# Run Python command
python -c "code here"

# Check database
sqlite3 app/habits.db ".tables"
```

**3. Initialize database:**
```bash
cd /opt/render/project/src/app
python -c "from appraju import init_db; init_db()"
```

**4. Test Flask app:**
```bash
cd app
python appraju.py --help
```

---

### How to Check Metrics

**1. View Metrics:**
```
Service Dashboard → [Metrics] tab
```

**2. Metrics shown:**
- **CPU Usage**: How much processing power being used
- **Memory Usage**: RAM consumption
- **Requests/Minute**: Traffic to your app
- **Response Time**: How fast app responds

**3. Understand limits (Free tier):**
- CPU: ~2 cores (shared)
- Memory: 512 MB (watch this!)
- Bandwidth: Unlimited
- Storage: 500 MB (local disk)

**4. What to do if usage high:**
- Upgrade to Standard instance
- Optimize database queries
- Clear old logs
- Soft restart

---

### How to Redeploy

**1. Automatic redeploy:**
- Push changes to GitHub
- Render automatically detects
- Redeploy happens in 2-3 minutes
- No manual action needed

**2. Manual redeploy:**
1. Go to Service
2. Click "Manual Deploy"
3. Click "Latest"
4. Wait 2-3 minutes for completion

**3. Soft restart (clears memory):**
1. Go to Service Settings
2. Click "Soft Restart"
3. App restarts, keeps same code
4. Takes 30-60 seconds

**4. Full restart (rebuild):**
1. Go to Service Settings
2. Click "Restart"
3. Rebuilds from GitHub
4. Takes 3-5 minutes

---

## Emergency Fixes

### "Everything is broken!"

**Quick recovery steps:**

```
1. Check Status:
   Service Dashboard → Status should show "Live ✓"
   If not, click "Manual Deploy" → "Latest"

2. Check Logs:
   Service Dashboard → [Logs] tab
   Look for error messages

3. Identify issue:
   - Build error → fix build command
   - Runtime error → check start command
   - Database error → reinitialize database
   - Missing env var → add to environment variables

4. Fix and redeploy:
   - Update code
   - Push to GitHub
   - Render auto-deploys
   - Wait 3-5 minutes

5. Still not working?
   - Soft restart: Service Settings → "Soft Restart"
   - Wait 5 minutes
   - Hard refresh browser: Ctrl+Shift+R
   - Try again

6. Last resort:
   - Delete service
   - Recreate from scratch (takes 5 minutes)
   - Use this guide again
```

---

### Contact Render Support

**If nothing works:**

1. Go to Render.com
2. Click help icon (?) in bottom right
3. Click "Contact Support"
4. Describe issue with:
   - Service name
   - Error message from logs
   - Steps you've tried

Render support usually responds in 2-4 hours.

---

## Prevention Tips

### Don't let issues happen!

```
1. ✓ Always test locally before pushing
   $ cd app
   $ python appraju.py
   $ Visit http://localhost:5000

2. ✓ Check logs regularly
   Look for warnings or errors

3. ✓ Keep backups
   Your GitHub repo is your backup ✓

4. ✓ Monitor metrics
   Check if CPU/Memory staying low

5. ✓ Update dependencies regularly
   Quarterly check for security updates

6. ✓ Use environment variables
   Never hardcode secrets or passwords

7. ✓ Document your setup
   Write down your commands for future reference

8. ✓ Test updates before production
   Create test service on Render first
```

---

## Summary

| Issue | Fix |
|-------|-----|
| Build failed | Check build command path (must include "app/") |
| 503 error | Check start command (must have "cd app" first) |
| Database error | Run init command in Shell tab |
| 404 on landing page | Check if database initialized |
| App very slow | Upgrade instance or optimize code |
| Memory leak | Soft restart or fix database connections |
| Can't access from phone | Use HTTPS URL, not localhost |
| Custom domain not working | Update DNS records (wait 15-60 min) |

---

**Most issues are solved by:**
1. Checking the logs
2. Verifying commands are correct
3. Reinitializing database
4. Redeploying

**You've got this! 🚀**
