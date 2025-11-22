# Render Quick Start Checklist

Fast-track deployment in 15 minutes. Just follow the steps!

---

## Pre-Deployment (5 minutes)

### ☐ Step 1: Generate SECRET_KEY
```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```
Copy the output. Save in notepad.

### ☐ Step 2: Verify GitHub repo is ready
- Repository: `Habit_track_maadri_paa`
- Branch: `main`
- Structure:
  ```
  ├── app/
  │   ├── appraju.py
  │   ├── requirements.txt
  │   └── .gitignore
  ```

### ☐ Step 3: Latest code pushed to GitHub
```bash
git status  # Should show "nothing to commit"
git log --oneline -3  # Show latest commits
```

---

## Deployment (10 minutes)

### ☐ Step 4: Create Render Web Service

| Setting | Value |
|---------|-------|
| **Service Name** | `habit-tracker` |
| **Environment** | `Python 3` |
| **Branch** | `main` |
| **Build Command** | `pip install -r app/requirements.txt` |
| **Start Command** | `cd app && gunicorn -w 4 -b 0.0.0.0:$PORT appraju:app` |
| **Instance Type** | `Free` |

### ☐ Step 5: Add 3 Environment Variables

| Variable | Value |
|----------|-------|
| `FLASK_ENV` | `production` |
| `SECRET_KEY` | *(paste from Step 1)* |
| `DATABASE_PATH` | `/opt/render/project/src/app/habits.db` |

### ☐ Step 6: Click "Create Web Service"
Wait 3-5 minutes for build and deployment.

**Status check:**
- View Logs → Should see "Build succeeded"
- Status badge → Should show "Live ✓"
- URL → Should show `https://habit-tracker.onrender.com`

---

## Post-Deployment (5 minutes)

### ☐ Step 7: Initialize Database

1. Go to Service Dashboard
2. Click **[Shell]** tab
3. Paste:
   ```bash
   cd /opt/render/project/src/app && python -c "from appraju import init_db; init_db()"
   ```
4. Press Enter
5. See success: "Database initialized successfully!"

### ☐ Step 8: Test Your App

**Visit:** https://habit-tracker.onrender.com

**Verify:**
- ✓ Page loads (no errors)
- ✓ 4 sample habits visible
- ✓ Calendar works
- ✓ Click checkbox → habit marked complete
- ✓ Chart updates
- ✓ Color and emoji display correctly

---

## Verification Checklist

Before declaring success, verify:

```
Frontend:
☐ Page title: "🎯 Habit Tracker"
☐ 4 sample habits loaded
☐ Calendar widget visible
☐ Progress chart visible
☐ Add button works
☐ Colors display correctly
☐ Emojis visible

Backend:
☐ API responds (check browser console)
☐ No 404 errors
☐ No 500 errors
☐ Database persists data

Performance:
☐ Page loads in <2 seconds
☐ Clicking habits is instant
☐ No lag or slowness
```

---

## Troubleshooting (if needed)

| Problem | Solution |
|---------|----------|
| **Build fails** | Check build command: `pip install -r app/requirements.txt` |
| **503 error** | Check start command has `cd app` first |
| **Database error** | Run init command in Shell tab |
| **Page not loading** | Wait 5 min, check Logs tab |
| **Still stuck?** | See RENDER_TROUBLESHOOTING.md |

---

## Update Process

When you want to deploy updates:

1. Make changes locally
2. Test: `cd app && python appraju.py`
3. Push: `git add . && git commit -m "message" && git push`
4. Render auto-deploys (2-3 minutes)
5. Done!

No manual steps needed after initial setup.

---

## Success! 🎉

Your Habit Tracker is now live!

**Share this URL:**
```
https://habit-tracker.onrender.com
```

**Next steps:**
- Add your own habits
- Track your progress
- Share with friends
- Deploy to custom domain (optional)

---

## Reference

**Useful links:**
- Dashboard: https://dashboard.render.com
- Your service: https://dashboard.render.com/services
- Render docs: https://render.com/docs

**Commands for later:**
```bash
# View logs
Service Dashboard → [Logs] tab

# Access shell
Service Dashboard → [Shell] tab

# Manual redeploy
Service Dashboard → [Manual Deploy] → [Latest]

# Check metrics
Service Dashboard → [Metrics] tab
```

---

**Questions? See:**
- RENDER_COMPLETE_GUIDE.md (detailed walkthrough)
- RENDER_VISUAL_GUIDE.md (screenshots & diagrams)
- RENDER_TROUBLESHOOTING.md (all issues & fixes)

**Done! Your app is live! 🚀**
